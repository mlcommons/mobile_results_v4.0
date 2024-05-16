# Instructions for building mlperf app with Samsung backend support 

## Clone the repo 
git clone https://github.com/mlcommons/mobile_app_open
cd mobile_app_open

## Switch to submission v4.0 freeze branch
git checkout f008ece19b439bd65b011c56c800d62141830a12

## Apply patch for Samsung updated backend 
git apply samsung_submission_v4.0.patch 

## Copy new Samsung backend files 
cp mbe_config_2400.hpp `mobile_back_samsung/samsung/lib/public/include/`


cp mbe_config_2400.pbtxt `mobile_back_samsung/samsung/lib/public/include/`

## Copy libraries:
from: https://github.com/mlcommons/mobile_back_samsung/tree/submission_v4.0_samsung_backend/samsung_libs 


to: `mobile_back_samsung/samsung/lib/internal/`

## Build the app
make WITH_SAMSUNG=1 flutter/android/release

## Push app to device
adb install `output/android-apks/2024-MM-DD_mlperfbench-f008ece-.apk`

## Launch the app once to create the cache folder
adb shell "am start -n org.mlcommons.android.mlperfbench/org.mlcommons.android.mlperfbench.MainActivity"

## Push samsung models to the device cache folder
Create a directory MLPerf_sideload and copy models to it
- Option1: Get models from  `closed/Samsung/code/ExynosNN/models`
- Option2: Generate models using the provided Samsung SDK


adb push path/to/MLPerf_sideload `/sdcard/Android/data/org.mlcommons.android.mlperfbench/files/`

## Prepare accuracy/validation dataset and put into mlperf_datasets.zip 

## Push the datatsets to the device (for accuracy measurement)
adb push mlperf_datasets.zip `/sdcard/Android/data/org.mlcommons.android.mlperfbench/files/`


adb shell "cd `/sdcard/Android/data/org.mlcommons.android.mlperfbench/files/` && unzip mlperf_datasets.zip"

# Now you can launch the app, select submission mode and press GO
