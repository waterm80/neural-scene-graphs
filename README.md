# Implementation

## Dataset

#### KITTI
1. Get the [KITTI MOT dataset](http://www.cvlibs.net/datasets/kitti/eval_tracking.php) we need:
   1. [Left color images](http://www.cvlibs.net/download.php?file=data_tracking_image_2.zip)
   2. [Right color images](http://www.cvlibs.net/download.php?file=data_tracking_image_3.zip)
   3. [GPS/IMU data](http://www.cvlibs.net/download.php?file=data_tracking_oxts.zip)
   4. [Camera Calibration Files](http://www.cvlibs.net/download.php?file=data_tracking_calib.zip)
   5. [Training labels](http://www.cvlibs.net/download.php?file=data_tracking_label_2.zip)
2. Extract everything to ```./data/kitti``` (Have to create this directory manually)

#### Virtual KITTI 2

```
bash ./download_virtual_kitti.sh
```

---

## Setting Environment

```
conda create -n neural_scene_graphs --file requirements.txt -c conda-forge -c menpo
conda activate neural_scene_graphs
cd neural-scene-graphs
```

## Training

The original iteration time is 1000000, we change it to 10000 due to the lack of time.  

vkitti2:
```
python main.py --config example_configs/config_vkitti2_Scene06.txt

```

KITTI:
```
python main.py --config example_configs/config_kitti_0006_example_train.txt
```

To train on normal GPU and CPU:

1. Use chunk and netchunk in the config files to limit parallel computed rays and sampling points.
   
2. Use training factor 
```
--training_factor = 'desired factor'
```

---

## Rendering a Sequence

#### Render a pretrained KITTI sequence

Download the pretrained weight
```
bash download_weights_kitti.sh
```

To make a full render pass over all selected images : 'render_only=True'
- Render only the outputs of the static background node : 'bckg_only=True'
- Render all dynamic parts : 'obj_only=True' & 'white_bkgd=True'
```
python main.py --config example_configs/config_kitti_0006_example_render.txt
```

## Result
```
tensorboard --logdir=example_weights/summaries --port=6006
```
