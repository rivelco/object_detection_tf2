# Object detection - Tensorflow 2

## Setting up the environment

- Python 3.10.2
- Pip 22.0.4
- Microsoft Visual Studio 2022
  - Visual C++ 2015-2022 Redistributable x64 14.31.31103.0

### Modules

Install the following modules using `pip` make sure to use administrator level

```python
pip install tensorflow
pip install tensorflow-gpu
pip install opencv-python
pip install Cython
pip install contextlib2
pip install pillow
pip install lxml
pip install jupyter
pip install matplotlib
pip install object_detection
```

## Usage

- Once all the modules has been installed, download protobuf from the official repo. Here I used version 3.19.4 from [https://github.com/protocolbuffers/protobuf/releases/tag/v3.19.4]
  - Unzip the file and add the `protoc.exe` file from `bin/` to the Path environment variable. Make sure to add it to the Path variable.
- Clone or download the models repository from Tensorflow Github repository at [https://github.com/tensorflow/models]
  - You can use the command `git clone https://github.com/tensorflow/models` just make sure to have git installed first
- Go to `models/research` on the console and run the command `protoc object_detection/protos/*.proto --python_out=.` the idea is to compile all the files in `protos/` using protobuf
  - Note that all the files ended with `.proto` must have another matching file ended with `.py`
  - If your command fail because it does not identify the command `protoc` try placing the file `protoc.exe` from protobuf inside the folder `models/research`
- Note that in the same folder where you place the main code `live_detection.py` must be another folder named `data` that contains the labels for the data sheet that we are going to use from COCO data sheets.
- Copy `setup.py` from `models/research/object_detection/packages/tf2/` to `models/research`
- Run the command `python -m pip install --use-feature=2020-resolver .`
- WARNING! This command may generate the desired files in the folder `models/research/build/lib/object_detection/protos` but we need them at the root of our Python packages. A simple solution is to copy all the files from the former to `AppData/Local/Programs/Python/Python310/Lib/site-packages/object_detection/protos`. If we don't do this, when executing `python live_detection.py` we may encounter with error like `cannot import name 'string_int_label_map_pb2' from 'object_detection.protos'`.

The following steps modifies source files from the packages installed through `pip` but it is the only way I found to make this work. This is caused by the incompatibilities between the Object Detection API and Tensorflow2. For example, note that we are running `live_detection.py` using a compatibility mode with Tensorflow1 as set in `import tensorflow.compat.v1 as tf`.

Next, we are going to be running the command `python live_detection.py` and finding the incompatible source files among our `pip` packages. Mostly changing just two lines on every source file. I will be listing each filename and location and the changes needed as well.

1. `AppData/Local/Programs/Python/Python310/lib/site-packages/object_detection/utils/label_map_util.py`
  - Change line 29 to `import tensorflow.compat.v1 as tf`

## Considerations

This procedure does not guaranty that all the needed and used modules work when tested independently, in many cases there are things to change for them to run with Tensorflow2, it is needed to use some things from the latest version of the Slim module. The "how to" for all of this, has been answered in stackoverflow.
