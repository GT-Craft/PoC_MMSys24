# PoC_MMSys24

This PoC is based on Ubuntu 22.04, The machine is supposed to have x86-based CPU and NVIDIA GPU (optional).

------------------------------------------------------------------------------------------------------------
## Setup Prerequisite
#### 1. Python3 and Virtualenv
If you don't have Python3, install it. If you want to setup in a virtual environment for python, install virtualenv.
```bash
$ sudo apt install python virtualenv
```
#### 2. Unity3D and Unity USD SDK (project wise)
- Firstly, visit [the official Unity website](https://docs.unity3d.com/hub/manual/InstallHub.html#install-hub-linux) and install Unity Hub. Or, run the following commands from that website.
```bash
$ wget -qO - https://hub.unity3d.com/linux/keys/public | gpg --dearmor | sudo tee /usr/share/keyrings/Unity_Technologies_ApS.gpg > /dev/null
$ sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/Unity_Technologies_ApS.gpg] https://hub.unity3d.com/linux/repos/deb stable main" > /etc/apt/sources.list.d/unityhub.list'
$ sudo apt update
$ sudo apt-get install unityhub
```
- Then, visit [the Unity archive](https://unity.com/releases/editor/archive) and install Unity 2020.03.

<img src="https://github.com/GT-Craft/PoC_MMSys24/blob/main/Figure/unity.jpg" style="width:400px">

------------------------------------------------------------------------------------------------------------

## Map Streaming and Semantic Segmentation
#### 1. Clone the repo & setup virtualenv
```bash
$ git clone git@github.com:GT-Craft/Map_Streaming_SemanticExtraction.git
$ cd Map_Streaming_SemanticExtraction
$ virtualenv ./venv --python=python3
$ . ./venv/bin/activate
(venv) $ ./setup_cpu.sh # for non-GPU user
(venv) $ ./setup_cu118.sh # for Nvidia-GPU user with CUDA 11
(venv) $ pip install -r requirements.txt
```
#### 2A. Download dataset & Train a model (Optional)
Since it takes long to train models, we provide pretrained models. You can skip this, or please refer [README in the streaming and segmentation repo](https://github.com/GT-Craft/Map_Streaming_SemanticExtraction).

#### 2B. Get pretrained models
```
$ mkdir pretrained_models # under Map_Streaming_semanticExtraction
```

Download `road` and `building` from [our shared drive](https://gtvault-my.sharepoint.com/:f:/g/personal/jheo33_gatech_edu/Ei14o8j-lTpIvsZNtQegs6cBPfoK2uv9Zltnxiy4SgIL9A?e=ks9jNj).
Then, put them under the created directory `pretrained_models`.

#### 3. Run
You need to change the project/dataset directory in the `scripts/config.json`.
```
{
"PROJECT_DIR" : "YOUR_PROJECT_PATH/Map_Streaming_SemanticExtraction/",
"DATASET_DIR" : "YOUR_DATASET_PATH/Massachusett_Dataset/"
}
```

1. **Streaming Map and Elevation (can skip)**: This requires the MS map account and key for APIs. We describe how to set it up in [README of the streaming and segmentation repo](https://github.com/GT-Craft/Map_Streaming_SemanticExtraction).
2. **Elevation Matrix Visualization**: As we prepared the sample data, you can run `cd scripts && python elevation_visualizer.py`
3. **Road and Building Segmentation**: `cd scripts && python road_buidling_segmentation.py`
4. **Training models (can skip)**: after setting the dataset directory, `cd scripts && python train_building.py` for building segmentation model. `cd scripts && python train_road.py` for road segmentation model.
5. **Validate trained models**: you can validate the trained models on your target area with ground truth data. You can directly run `cd scripts && python validate_road_building.py` with sample data.

## Virtual Scene Generation
#### 1. Clone the repo
```bash
$ git clone git@github.com:GT-Craft/VirtualSceneGeneration.git
```
