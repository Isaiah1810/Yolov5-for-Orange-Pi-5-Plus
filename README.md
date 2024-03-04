# Yolov5-for-Orange-Pi-5-Plus
Aggregation of packages and steps for using YOLOv5 Object Detection on Orange Pi 5 Plus NPU

pip install ultralytics


This repository is an aggregator and tutorial for getting YOLOv5 object detection running with the NPU on an Orange Pi 5 Plus. 

This tutorial was designed for the Orange Pi 5 Plus, but should likely work with the Orange Pi 5 and 5B, and might work with other RK3588 boards.

Image: Ubuntu 20.04, downloaded from official Orange Pi website [https://drive.google.com/drive/folders/1wOmKUla8CwUPTfxvfCGutj8lbMZFtFCm](https://drive.google.com/drive/folders/1wOmKUla8CwUPTfxvfCGutj8lbMZFtFCm)

NOTE: RKNPU2 is only supported for Linux Kernel version 5.10. It will not work for version 6

PC for conversions: x86 Windows laptop with Ubuntu 20.04 running through wsl2

Install default tools, python packages and toolkit repo

NOTE: Version conflicts might exist for pandas for rknn_toolkit and cv2, but they didn't cause problems

Check if your installation has RKNPU installed

If any of these are miss or out of date, you have to download them and move them to the proper locations

Next, install the rknn_toolbox2_lite package and api

Finally, place yolov5n.rknn and rknn_yolov5n_test.py in the same directory, and run the script with a webcam. Python script can be modified to work off of single images relatively easily

If you want to convert your own models:

NOTE: rknn-toolkit2 (not lite) only runs on x86 machines, and debian-based distros(debian, ubuntu)

NOTE2: if the pip install fails, your current pc setup might not work. The install failed when ran on an Ubuntu VirtualBox, but your mileage may vary

Next, download your desired model from the [rknn_model_zoo](https://github.com/airockchip/rknn_model_zoo/tree/main) in .onnx form, and place it in your working directory

NOTE: Not all YOLO models will work well with the rknn_yolov5n_test.py script’s data formatting. See specific model for details

Open up test.py in an editor, and change the path for the onnx model to your downloaded model, and write the path for the rknn model to be downloaded to. The script might throw an error, but if the file appears in the directory, it is converted successfully.

NOTE: If when run with a working webcam, and the failure comes from in output size/out of bounds errors when trying to convert a different YOLO model, .onnx file likely isn’t formatted properly 

ERRORS

Invalid Runtime. Caused by a problem with the driver. We solved it by reflashing the distro and re-updating drivers
