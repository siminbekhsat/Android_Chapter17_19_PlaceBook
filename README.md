# Android_PlaceBook


******************** PlaceBook Application ******************
*********************** Simin Bekhsat ***********************
****** PLaceBook Github Link: https://github.com/siminbekhsat/Android_PlaceBook.git *******


First create an app named PlaceBook from Google Maps Activity and also get the API key from google maps platform and copy it into google_maps_api.xml.
MapsActivity is the start up kotlin class which is created from the Maps template and Oncreate function in MapsActivity loads the activity_maps.xml, and finds the map Fragment from the layout and uses it to initialize the map by getMapAsync() function.
OnMapsReady() is a function to let you know when the map is ready and it is overriding a method from the base class.

In next chapter (chapter 14) for creating the location service client we used the fused location API and create a function named setUpLocationClient() and call it at OnCreate() function.
I added <uses-permission> tag to the app's manifest file to reach outside app sandbox and access protected features. And for the location accuracy I used access_fine_location to get all location sources including GPS.
Then by adding requestLocationPermissions() function we could prompt the user to accept or deny the permission.
Then I have added the companion object to the MapsActivity includes request location which is a request code passed to requestPermission() to identify the specific permission and TAG which is passed to Log.e method to print info to help debugging.
getCurrentLocation() is a private function to get user’s current location and move the map to that location by checking the permission and if permission denied, get the permission by requestLocationPermissions().
Then we update the onMapReady() to call the getCurrentLocation and also add the onRequestPermissionsResult() to display permission dialog and handle the user response.

To get the google places in chapter 15 we need to enable the Places API and add the Places API dependency to gradle.
For selecting the point of interest we need to add the map.setOnPoiClickListener() into MapsActivity and provide it a lambda to implement onPoiClick().
For loading place details we need to import the places client library and the PlacesClient and then set up the places client by setupPlacesClient() function to pass the app context to initialize the places library and then call this method in onCreate() function.
For fetching the place details we have added the displayPoi() function with the Name, Phone Number, Adress, and etc… information about the place. Then we update setOnPoiClickListener() to call displayPoi() when a place on the map is tapped.
In the next step for fetching the place photo we need to add displayPoiGetPhotoStep() method to get the PhotoMetaData object if it is available with determining the maximum width and height for that image with creating a dimen value resorce xml file.
In next step I have added displayPoiDisplayStep() in MapsActivity to create iconPhoto from the photo if the photo is not null.
displayPoiDisplayStep(place, null) passess the place and a null bitmap image and displayPoiDisplayStep(place, bitmap) passes the place object and the bitmap image.
InfoWindowAdapter class created for custom info which has two implemented methods:
getInfoWindow() and getInfoContents(). The first one returns custom view for the full info window and the second one returns custom view for interior content of the window.
In displayPoiDisplayStep(), when we replace the call to addMarker() with marker tag,  it provides the tag property as a means to associate the marker with data you are managing in the app and addMarker() returns a Marker object and we assign it to marker.

In next chapter (chapter 16) we created a model package to have the @Entity in it to tell Room that this is a database entity class.
We also created the db package with the kotlin file named BookmarkDao and @Dao tells Room that this is a data access object. We also have @Query annotation to define an SQL statement to read all of the bookmarks from the database and return them.
We also added another abstract class named PlaceBookDatabase in db package with @Database annotation which is uses to identify a database class to Room.
Next step is creating a repository. For this step I have created a repository package and a BookmarkRepo class in it. The addBookmark() method allows a single Bookmark to be added to the repository and returns the null or unique id of the newly saved bookmark. The createBookmark() is another method to return a freshly initialized
Bookmark object.
The MapsViewModel class in viewmodel package which inherit from AndroidViewModel includes addBookmark(), addBookmarkFromPlace(), getBookmarkViews(), mapBookmarksToBookmarkView(), getPlaceCategory(), and bookmarkToBookmarkView() methods.
addBookmark(): Takes in a LatLng location and creates a new bookmark at the given location and returns the new bookmark ID.
addBookmarkFromPlace(): Takes a Google Place and a Bitmap image and called by the MapsActivity when it wants to create a bookmark for a Google Place that has been identified by the user.
getBookmarkViews(): Returns the LiveData object that will be observed by MapsActivity.
mapBookmarksToBookmarkView(): Maps the LiveData objects provided by BookmarkRepo into LiveData objects that can be used by MapsActivity.
getPlaceCategory(): onverts a place type to a bookmark category.
bookmarkToBookmarkView(): Converts a Bookmark model to a BookmarkDetailsView model.
handleInfoWindowClick(marker: Marker) handles taps on a place info window.
addPlaceMarker(): A helper method to add a single blue marker to the map based on a BookmarkMarkerView.
displayAllBookmarks(): Walks through a list of BookmarkMarkerView objects and calls
addPlaceMarker() for each one.
saveBitmapToFile(): Takes in a Context, Bitmap and filename as string,and saves the Bitmap to permanent storage.
saveBytesToFile(): takes in a Context, ByteArray, and a String for the filename and saves the bytes to a file.
setImage(image: Bitmap, context: Context): Provides the public interface for saving an image for a Bookmark.
loadBitmapFromFile(context: Context, filename: String): Returns a Bitmap image by loading the image from the specified filename. A File object is used to combine the files directory for the given context with the filename and the filePath is constructed from the absolute path of the File. 
startBookmarkDetails(bookmarkId: Long): Starts the BookmarkDetailsActivity by using an explicit Intent.
handleInfoWindowClick(marker: Marker): This method was saving the bookmark to database but in this step it saves the bookmark if it hasn’t saved before  or starts the bookmark details activity if the bookmark has been saved.
getLiveBookmark(bookmarkId: Long): Returns a live bookmark from the bookmark DAO.
populateImageView(): Loads the image from bookmarkView and then uses it to set the imageViewPlace.
onCreateOptionsMenu(menu: android.view.Menu): Provide items for the Toolbar by loading in the menu_bookmark_details menu.

In next step in chapter 18, we have added data binding to MapActivity and added the setupToolbar() that is required when we use the library version of the toolbar as the action bar.
I have added a new kotlin class named BookmarkListAdapter to the adapter package that includes a ViewHolder class is defined to hold the view widgets, setBookmarkData which is called when the bookmark data changes and refreshes the RecyclerView by calling notifyDataSetChanged(), onCreateViewHolder to create a ViewHolder by inflating the bookmark_item layout and passing in the mapsActivity and finally the getItemCount() method to return the number of items in the bookmarkData.
setupNavigationDrawer(): sets up the adapter for the bookmark recycler view by getting the RecyclerView from the Layout, sets a default LinearLayoutManager and creates a new BookmarkListAdapter to assign to the RecyclerView.
canPick(context: Context) : By creating an intent for picking images and and then  checking to see if the Intent can be resolved, this method determines if the device can pick an image from a gallery.
canCapture(): Determines if the device has a camera to capture a new image.
replaceImage(): this method will be called when the user taps on the bookmark image and if newFragment is not null it will create the PhotoOptionDialogFragment fragment.
calculateInSampleSize(): Calculates the inSampleSize that can be used to resize an image.
rotateImage(img: Bitmap, degree: Float): Creates a new bitmap and rotates it by the given degrees.
setImage(context: Context, image: Bitmap): Takes and save a Bitmap image to the associated image file for the current BookmarkView.
updateImage(image: Bitmap): assigns an image to the imageViewPlace and saves it to the bookmark image file using bookmarkDetailsView.setImage().
getImageWithPath(filePath: String): Loads the downsampled image and returns it by using the new decodeFileSize method.
onActivityResult(): First the resultCode checks if the photo capture has not been cancelled by user and then checks to see which call is returning a result and continue the process if the requestCode matches with request_capture_image. Revoke the permission and then getImageWithPath() is called to get the image from the new path.
decodeUriStreamToSize(): Reads the size of image from the Uri stream, calculates the sample size and loads in the downsampled image.
getImageWithAuthority(uri: Uri): Uses the new decodeUriStreamToSize method to load the downsampled image and return it.

In chapter 19 we did some details to improve apps’s look and usability.
buildCategoryMap(): Builds a HashMap that relates Place types to category names.
placeTypeToCategory(placeType: Place.Type): Takes in a Place type and converts it to a valid category. Default is “Other” and if it is match with one of the place types,it will be assigned to category.
buildCategories(): Builds a HashMap that relates the category names to the category icon resource IDs.
getCategoryResourceId(placeCategory: String): Converts a category name to a resource ID.
getPlaceCategory(place: Place): Converts a place type to a bookmark category.
getCategories() : Rturns the categories list from the bookmark repository.
populateCategoryList(): If bookmarkDetailsView is null, this method returns immediately and if it is not null imageViewCategory will update to category icon.
searchAtCurrentLocation(): Define the attributes for each place, computes the bounds of the region that is visible now, build up the intent and finally starts the Activity and passes a request code of AUTOCOMPLETE_REQUEST_CODE.
newBookmark(latLng: LatLng): Creates a new bookmark from a location and let us to edit the new Bookmark.
deleteFile(context: Context, filename: String): Deletes a file from the main files directory.
deleteImage(context: Context): Deletes the current bookmark image file.
deleteBookmark(bookmark: Bookmark): Deletes the bookmark from the database and the bookmark image.
disableUserInteraction(): Prevents user touches by setting a flag on the main window.
enableUserInteraction(): Clears the disableUserInteraction flag.
showProgress(): Makes the progress bar visible and disables user interaction.
hideProgress(): Hides the progress bar and enables user interaction.








