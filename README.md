# posecnn-pytorch

PyTorch implementation of the PoseCNN framework.

### Introduction

We introduce PoseCNN, a new Convolutional Neural Network for 6D object pose estimation. PoseCNN estimates the 3D translation of an object by localizing its center in the image and predicting its distance from the camera. The 3D rotation of the object is estimated by regressing to a quaternion representation. [arXiv](https://arxiv.org/abs/1711.00199), [Project](https://rse-lab.cs.washington.edu/projects/posecnn/)

[![PoseCNN](http://yuxng.github.io/PoseCNN.png)](https://youtu.be/ih0cCTxO96Y)

### License

PoseCNN is released under the MIT License (refer to the LICENSE file for details).

### Citation

If you find the package is useful in your research, please consider citing:

    @inproceedings{xiang2018posecnn,
        Author = {Xiang, Yu and Schmidt, Tanner and Narayanan, Venkatraman and Fox, Dieter},
        Title = {PoseCNN: A Convolutional Neural Network for 6D Object Pose Estimation in Cluttered Scenes},
        booktitle   = {Robotics: Science and Systems (RSS)},
        Year = {2018}
    }

### Installation

Use python3 by default. If ROS is needed, compile with python2.

1. Install [PyTorch](https://pytorch.org/).

2. Install Eigen from the Github source code [here](https://github.com/eigenteam/eigen-git-mirror)

3. Install Sophus from the Github source code [here](https://github.com/yuxng/Sophus)

4. Compile the new layers under $ROOT/lib/layers we introduce in PoseCNN.
    ```Shell
    cd $ROOT/lib/layers
    sudo python setup.py install
    ```

5. Compile the ycb_render in $ROOT/ycb_render
    ```Shell
    cd ycb_render
    python setup.py develop
    ```

6. Compile cython components
    ```Shell
    cd $ROOT/lib
    python setup.py build_ext --inplace
    ```

### Download
- 3D models [here]

### Data needed for training
- Pretrained VGG16 weights: [here](https://drive.google.com/file/d/1tTd64s1zNnjONlXvTFDZAf4E68Pupc_S/view?usp=sharing) (528M). Put the weight file to $ROOT/data/checkpoints.
- Background dataset: Our own images [here](https://drive.google.com/open?id=1YDnGV4poelk9iezxLxYK_zexXugc4Ih1)
- Background dataset: COCO 2014 [here](https://cocodataset.org/#download)

### Required environment
- Ubuntu 16.04 or above
- PyTorch 0.4.1 or above
- CUDA 9.1 or above

### Running the demo
1. Download our trained model on five YCB Objects from [here](https://drive.google.com/open?id=1fxfBBCOPqSMYARiJQBc8ZjcWq5LiLHDq), and save it to $ROOT/data/checkpoints.

2. Download the 3D models of the YCB Objects from [here](https://drive.google.com/file/d/1gmcDD-5bkJfcMKLZb3zGgH_HUFbulQWu/view?usp=sharing), and save it to $ROOT/data/YCB_Object.

3. run the following script
    ```Shell
    ./experiments/scripts/demo.sh $GPU_ID
    ```

### Running on the YCB-Video dataset
1. Download the YCB-Video dataset from [here](https://rse-lab.cs.washington.edu/projects/posecnn/).

2. Create a symlink for the YCB-Video dataset
    ```Shell
    cd $ROOT/data/YCB_Video
    ln -s $ycb_data data
    ln -s $ycb_models models
    ```

3. Training and testing on the YCB-Video dataset
    ```Shell
    cd $ROOT

    # multi-gpu training
    ./experiments/scripts/ycb_video_train.sh

    # testing
    ./experiments/scripts/ycb_video_test.sh $GPU_ID

    ```

### Running with ROS
```Shell
# start realsense
roslaunch realsense2_camera rs_aligned_depth.launch tf_prefix:=measured/camera

# start rviz
rosrun rviz rviz -d ./ros/posecnn.rviz

# run posecnn for detection only (20 objects)
./experiments/scripts/ros_ycb_object_test_detection.sh $GPU_ID

# run full posecnn (20 objects)
./experiments/scripts/ros_ycb_object_test.sh $GPU_ID
```
