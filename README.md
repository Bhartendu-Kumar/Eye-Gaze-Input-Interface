# Eye-Gaze-Input-Interface

Project has following directories:
```
.
|--media/
|  |--<sample_media_files>
|
|--src/
|  |--face_detection.py
|  |--facial_landmarks_detection.py
|  |--head_pose_estimation.py
|  |--gaze_estimation.py
|  |--input_feeder.py
|  |--mouse_controller.py
|  |--test_models.py
|  |--main.py
|  |--benchmark.py
|
|--requirements.txt
|--download_models.sh
```

Models used for making inference are IRs from OpenVino Model Zoo.
These can be found here:
* [face-detection-adas-0001](https://docs.openvinotoolkit.org/latest/_models_intel_face_detection_adas_0001_description_face_detection_adas_0001.html)
* [landmarks-regression-retail-0009](https://docs.openvinotoolkit.org/latest/_models_intel_landmarks_regression_retail_0009_description_landmarks_regression_retail_0009.html)
* [head-pose-estimation-adas-0001](https://docs.openvinotoolkit.org/latest/_models_intel_head_pose_estimation_adas_0001_description_head_pose_estimation_adas_0001.html)
* [gaze-estimation-adas-0002](https://docs.openvinotoolkit.org/latest/_models_intel_gaze_estimation_adas_0002_description_gaze_estimation_adas_0002.html)

OpenVino's [Model Downloader](https://docs.openvinotoolkit.org/latest/_tools_downloader_README.html) was used to download all the models.<br>

**Note:** If not using `download_models.sh` script specify the output directory as `models` to `downloader.py` model downloader script of openvino, while running command in project root directory, so that paths used in `main.py` remain correct.


## Demo
After installing all the required dependencies and model files with correct precisions, run following command for a quick demo:
```
python3 src/main.py -t video -i media/demo.mp4
```

## Documentation

Code base is moduler with each module having seperate concerns:<br>
- `face_detection.py`: Class for utilizing Face Detection model to extract box coordinates of face of the person in frame. These coordinates are used to crop face from frame.
- `facial_landmarks_detection.py`: Class for utilizing Facial Landmarks Detection model to get the facial landmarks coordinates from face. However, for the app only required eye landmarks are returned which are later used to extract left and right eye.
- `head_pose_estimaion.py`: Class for utilizing Head Pose Estimation model to extract, from face, the head pose angles- yaw, pitch and roll as list with indices in order respectively. These angles are later required in pipeline.
- `gaze_estimation.py`: Class for utilizing Gaze Estimation model which given left and right eye images as well as head pose angles, yields the gaze vectors. Gaze vectors define direction of person's gaze.
- `input_feeder.py`: Convenient class for reading and feeding frames from input media.
- `mouse_controller.py`: Convenient class for controlling mouse pointer.
- `test_models.py`: Script written for purpose of individual testing of models for correct output. Appropriate function can be run to check working of model.
- `main.py`: Script, which is the starting point for the app.
- `benchmark.py`: Script used to benchmark the models.
- `download_models.sh`: Bash script to download all required models from model zoo automatically.


== Prerequisites

To run the application in this tutorial, the OpenVINO™ toolkit and its dependencies must already be installed and verified using the included demos. Installation instructions may be found at: https://software.intel.com/en-us/articles/OpenVINO-Install-Linux or https://github.com/udacity/nd131-openvino-fundamentals-project-starter/blob/master/linux-setup.md

The below steps are tested on Ubuntu 16.04:



### 1. Install OpenVino
```
wget http://registrationcenter-download.intel.com/akdlm/irc_nas/16612/l_openvino_toolkit_p_2020.2.120.tgz
tar -xvf l_openvino_toolkit_p_2020.2.120.tgz
cd l_openvino_toolkit_p_2020.2.120
sed -i 's/decline/accept/g' silent.cfg
sudo ./install.sh -s silent.cfg
```
### 2.System dep
```
sudo apt update
sudo apt-get install python3-pip
pip3 install numpy
pip3 install paho-mqtt
sudo apt install libzmq3-dev libkrb5-dev
sudo apt install ffmpeg
sudo apt-get install cmake
sudo apt-get install python3-venv
```
### 3.Create a Virtual env
```
python3 -m venv openvino-env
source openvino-env/bin/activate
```
### 4.Project dep
```
pip3 install -r requirements.txt
```
----
