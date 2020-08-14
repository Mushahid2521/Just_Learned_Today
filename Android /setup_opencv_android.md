### Below is the step by steps for integrating OpenCv for Android Development  
**Setp 1:**  
Download the opencv latest releases from this official [releases](https://opencv.org/releases/) and extract it.   
  
**Step 2:**  
Open Android Studio and Create new project.   

**Step 3:**  
Create a new directory in the root directory of the project with naming opencv ```_EX: opencvproject/opencv_```  
  
**Step 4:**  
Copy all the contents of the ```opencv_release_folder/sdk/java``` to the created directory in the AS project.  
  
**Step 5:**  
Create a ```build.gradle``` file inside that directory.   
  
**Step 6:**  
Copy the following contents in the guild.gradle file  
```
apply plugin: 'com.android.library'

def openCVersionName = "4.4.0-pre"
def openCVersionCode = ((4 * 100 + 4) * 100 + 0) * 10 + 0

buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.0.1"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.0"

    defaultConfig {
        minSdkVersion 16
        targetSdkVersion 30
        versionCode openCVersionCode
        versionName openCVersionName
    }

    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDirs = ['src']
            resources.srcDirs = ['src']
            res.srcDirs = ['res']
            aidl.srcDirs = ['src']
        }
    }
}
```  
You will get the versionname and versioncode from the gradle file in the downloaded release folder.  
  
**Step 7:**  
Include ```include ':opencv'``` in the ```settings.gradle``` file and **Sync** the project.  

**Step 8:**  
Open ```Project Structure``` from file->Project Structure and move to the dependency section. Click ```app``` module and then Click the ```+``` sign, from there select the ```Module Dependency```. Add the ```opencv``` library.   
  
**Step 9:**  
Enjoy OpenCv by importing with ```org.opencv.*```. 