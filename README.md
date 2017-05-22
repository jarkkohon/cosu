# cosu

This folder contains the source code for a Google I/O 2016 codelab.

### License

```
Copyright 2016 Google, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements. See the NOTICE file distributed with this work for
additional information regarding copyright ownership. The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License. You may obtain a copy of
the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
License for the specific language governing permissions and limitations under
the License.
```

### Build and Signing
https://developer.android.com/studio/publish/app-signing.html
```
./gradlew assembleRelease

#This creates an APK named module_name-unsigned.apk in project_name/module_name/build/outputs/apk/. The APK is unsigned and unaligned at this point—it can't be installed until signed with your private key.
#Align the unsigned APK using zipalign:
zipalign -v -p 4 app/build/outputs/apk/app-release-unsigned.apk my-app-unsigned-aligned.apk

#zipalign ensures that all uncompressed data starts with a particular byte alignment relative to the start of the file, which may reduce the amount of RAM consumed by an app.
#Sign your APK with your private key using apksigner:
apksigner sign --ks my-release-key.jks --out my-app-release.apk my-app-unsigned-aligned.apk

#This example outputs the signed APK at my-app-release.apk after signing it with a private key and certificate that are stored in a single KeyStore file: my-release-key.jks.
#The apksigner tool supports other signing options, including signing an APK file using separate private key and certificate files, and signing an APK using multiple signers. For more details, see the apksigner reference.
#Note: To use the apksigner tool, you must have revision 24.0.3 or higher of the Android SDK Build Tools installed. You can update this package using the SDK Manager.

#Verify that your APK is signed:
apksigner verify my-app-release.apk

```

### Installation and run

http://stackoverflow.com/a/17289998/1227867

```
# install
adb install my-app-release.apk

# or reinstall
adb install -r my-app-release.apk

# run
./adb-run.sh my-app-release.apk

# get pacakge name from apk
aapt dump badging my-app-release.apk|awk -F" " '/package/ {print $2}'|awk -F"'" '/name=/ {print $2}'
```


### Uninstall and device owner removal

1. Uncomment the device owner removal code from MainActivity.java: "UNCOMMENT TO REMOVE DEVICE OWNER"
2. Rebuild whole app
3. Reinstall
4. Run
5. Check from logcat that it was done: `adb logcat *:S COSU:*`, also can check under Settings -> Security -> Other sec settings -> device owners
6. Uninstall<BR>
`adb uninstall com.google.codelabs.cosu`

### Notes for setupping device account for cosu with emm:
https://developers.google.com/android/work/prov-devices#final_setup_steps_for_all_modes_of_operation

- During device setup, the user enters a special DPC-specific token when they are prompted to add an account. A token is in the format “afw#DPC_IDENTIFIER.

EMM needs to support these features?
  https://developers.google.com/android/work/requirements/purpose-built

wso2 emm example?:
https://github.com/wso2/product-emm

