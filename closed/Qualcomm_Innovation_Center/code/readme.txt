######
###### Build mlperf_app.apk with QTI backend support for testing on Android
######

git clone https://github.com/mlcommons/submissions_mobile_app_v4.0.git
cd submissions_mobile_app_v4.0
git checkout Qualcomm

# Qualcomm App source code => https://github.com/mlcommons/submissions_mobile_app_v4.0/tree/Qualcomm

#download the snpe sdk 2.20.0.240223 from this path
https://qpm.qualcomm.com/#/main/tools/details/qualcomm_neural_processing_sdk (Version 2.20.0)
# Select "Linux" and "Version 2.20.0.240223" from this path.
# Instructions to install Qualcomm Package Manager 3 is also in the same page above.

Install QPM3 from https://qpm.qualcomm.com/#/main/tools/details/QPM3

From terminal, run below commands:

1. qpm-cli --login <username>
2. qpm-cli --license-activate qualcomm_neural_processing_sdk
3. qpm-cli --extract qualcomm_neural_processing_sdk (or)
   qpm-cli --extract <full path to downloaded .qik file>

# rename the folder extracted above to qaisw-2.20.0.240223

# copy the snpe sdk to mobile_back_qti
# make sure to copy qaisw-2.20.0.240223,
# make sure to set up firebase as instructed in https://github.com/mlcommons/mobile_app_open/blob/master/docs/environment-setup/env-setup-firebase.md
make OFFICIAL_BUILD=false FLUTTER_BUILD_NUMBER=1 WITH_QTI=1 docker/flutter/android/release

# If there is any issue in regards to copying of the apk to output/android-apks, the release apk can be obtained from flutter/build/app/outputs

# Install app to device
adb install output/android-apks/<date>_mlperfbench-<commit_id>-qt.apk
#Open the app once on the device (to create the app's directory)

# Generate the DLCs
cd mobile_back_qti/DLC/
make SNPE_SDK=<path to unzipped qaisw-2.20.0.240223>

# Push the models to the device
adb push output/DLC/mlperf_models /sdcard/Android/data/org.mlcommons.android/mlperfbench/files/

# Push the datatsets to the device (required only for accuracy)
adb push mlperf_datasets /sdcard/Android/data/org.mlcommons.android/mlperfbench/files/

# select submission mode and press GO
