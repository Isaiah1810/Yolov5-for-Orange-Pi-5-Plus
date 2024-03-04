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

```
sudo apt update
sudo apt upgrade
sudo apt install git
sudo apt install python3-pip
pip install cv2
pip install numpy
git clone https://github.com/rockchip-linux/rknn-toolkit2.git
cd rknn-toolkit2
```

Check if your installation has RKNPU installed

```
#If rknn_server, restart_rknn.sh, and start_rknn.sh show up, driver installed
ls /usr/bin | grep -i rknn
#Check for version
dmesg | grep -i rknpu
#Version for testing was 0.8.2, however later versions likely will work

#Check server version, version for testing was 2.1.0
rknn_server
#Press enter
```

If any of these are miss or out of date, you have to download them and move them to the proper locations

```
mv rknn-toolkit2/rknpu2/runtime/Linux/librknn_api/aarch64 /usr/lib
cd rknn-toolkit2/rknpu2/runtime/Linux/rknn_server/aarch64/usr/bin/
mv rknn_server /usr/bin
mv start_rknn.sh /usr/bin
mv restart_rknn.sh /usr/bin
```

Next, install the rknn_toolbox2_lite package and api

```
cd rknn-toolkit2/rknn_toolkit2_lite/packages

#Install api, where x is your current python version
pip install knn_toolkit_lite2-1.6.0-cp3x-cp3xm-linux_aarch64.whl
```

Finally, place yolov5n.rknn and rknn_yolov5n_test.py in the same directory, and run the script with a webcam. Python script can be modified to work off of single images relatively easily

If you want to convert your own models:

NOTE: rknn-toolkit2 (not lite) only runs on x86 machines, and debian-based distros(debian, ubuntu)

NOTE2: if the pip install fails, your current pc setup might not work. The install failed when ran on an Ubuntu VirtualBox, but your mileage may vary

```
git clone https://github.com/rockchip-linux/rknn-toolkit2.git
cd rknn-toolkit2/rknn-toolkit2/packages
pip install rknn_toolkit2-1.6.0+81f21f4d-cp3xx-cp3xx-linux_x86_64.whl
```

Next, download your desired model from the [rknn_model_zoo](https://github.com/airockchip/rknn_model_zoo/tree/main) in .onnx form, and place it in your working directory

NOTE: Not all YOLO models will work well with the rknn_yolov5n_test.py script’s data formatting. See specific model for details

Open up test.py in an editor, and change the path for the onnx model to your downloaded model, and write the path for the rknn model to be downloaded to. The script might throw an error, but if the file appears in the directory, it is converted successfully.

NOTE: If when run with a working webcam, and the failure comes from in output size/out of bounds errors when trying to convert a different YOLO model, .onnx file likely isn’t formatted properly 

ERRORS

Invalid Runtime. Caused by a problem with the driver. We solved it by reflashing the distro and re-updating drivers
```
--> Load RKNN model
done
--> Init runtime environment
E RKNN: [02:57:49.400] failed to open rknpu module, need to insmod rknpu dirver!
E RKNN: [02:57:49.400] failed to open rknn device!
E Catch exception when init runtime!
E Traceback (most recent call last):
  File "/home/pi/.local/lib/python3.10/site-packages/rknnlite/api/rknn_lite.py", line 148, in init_runtime
	self.rknn_runtime.build_graph(self.rknn_data, self.load_model_in_npu)
  File "rknnlite/api/rknn_runtime.py", line 919, in rknnlite.api.rknn_runtime.RKNNRuntime.build_graph
Exception: RKNN init failed. error code: RKNN_ERR_FAIL
 
Init runtime environment failed
```
