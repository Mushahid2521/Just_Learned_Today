### Capturing image from Camera  
First we need to set the permission in Manifest  
```
<manifest>
...
      <uses-feature android:name="android.hardware.camera"
                  android:required="true" />
...
</manifest>
```  
  
Then we need to call the intent to capture image.  
```
private static final int REQUEST_CAPTURE_IMAGE = 100;

private void openCameraIntent() {
    Intent pictureIntent = new Intent(
                              MediaStore.ACTION_IMAGE_CAPTURE
                           );
    if(pictureIntent.resolveActivity(getPackageManager()) != null) { 
           startActivityForResult(pictureIntent,
                              REQUEST_CAPTURE_IMAGE);
    }
}
```  

Get the result from the following function.  
```
@Override
protected void onActivityResult(int requestCode, int resultCode,
                                              Intent data) {
    if (requestCode == REQUEST_CAPTURE_IMAGE && 
                              resultCode == RESULT_OK) {
        if (data != null && data.getExtras() != null) {
        Bitmap imageBitmap = (Bitmap) data.getExtras().get("data");
        mImageView.setImageBitmap(imageBitmap);
        }
    }
}
```  
  
If we want to send the file to the server through imagePath using MultiPartRequest, we need to save the file in a directory as temporary file.To do so we will save the file in a temporary directory and get the URI of it.    
```
<manifest>
     ...
<uses-permission
       android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    ...
</manifest>
```  

We need to get a file name to save the file.  
```
String imageFilePath;
private File createImageFile() throws IOException {
        String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss",  Locale.getDefault()).format(new Date());
        String imageFileName = "IMG_" + timeStamp + "_";
        File storageDir = getExternalFilesDir(Environment.DIRECTORY_PICTURES);
        File image = File.createTempFile(
                imageFileName,  /* prefix */
                ".jpg",         /* suffix */
                storageDir      /* directory */
        );

        mTempPhotoPath = image.getAbsolutePath();
        return image;
    }
```    

We need to add the following in Manifest file application section.  
```
<provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="${applicationId}.provider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths" />
        </provider>
```  

We need to create a ```Android Resource Directory``` with ```xml``` type in the res directory and create a xml file named ```file_paths.xml```. Add the following in the xml file created.  
```
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
    <external-path
        name="external_files"
        path="." />
</paths>
```  

Get the URI as follows.  
```
Uri photoUri = FileProvider.getUriForFile(Objects.requireNonNull(getApplicationContext()),
                        BuildConfig.APPLICATION_ID + ".provider", photoFile);
```

  

































