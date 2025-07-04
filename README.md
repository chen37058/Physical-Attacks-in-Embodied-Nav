# Towards Physically Realizable Adversarial Attacks in Embodied Vision Navigation(IROS 2025)

[arXiv Paper](https://arxiv.org/abs/2409.10071) 

## Abstract
The significant advancements in embodied vision navigation have raised concerns about its susceptibility to adversarial attacks exploiting deep neural networks. Investigating the adversarial robustness of embodied vision navigation is crucial, especially given the threat of 3D physical attacks that could pose risks to human safety. However, existing attack methods for embodied vision navigation often lack physical feasibility due to challenges in transferring digital perturbations into the physical world. Moreover, current physical attacks for object detection struggle to achieve both multi-view effectiveness and visual naturalness in navigation scenarios. To address this, we propose a practical attack method for embodied navigation by attaching adversarial patches to objects, where both opacity and textures are learnable. Specifically, to ensure effectiveness across varying viewpoints, we employ a multi-view optimization strategy based on object-aware sampling, which optimizes the patch's texture based on feedback from the vision-based perception model used in navigation. To make the patch inconspicuous to human observers, we introduce a two-stage opacity optimization mechanism, in which opacity is fine-tuned after texture optimization. Experimental results demonstrate that our adversarial patches decrease the navigation success rate by an average of 22.39%, outperforming previous methods in practicality, effectiveness, and naturalness.

## Example Video
![example-video-1](docs/naturalness.png)

![example-video-2](docs/example-video-2.gif)

## Experiment
### Attack performance against multiple targets in HM3D for navigation
| Target       | Attack Method  | SR↓(%)  | SPL↓(%)  | DTS↑(m)  |
|-------------|---------------|---------|---------|---------|
| **tv monitor** | No Attack     | 100.00  | 63.03   | 0.04    |
|             | Camouflage     | 100.00 *(0.00↓)* | 56.37 *(6.66↓)* | 0.04 *(0.00↑)* |
|             | *Ours* Patch   | **60.00** *(40.00↓)* | **14.81** *(48.22↓)* | **1.68** *(1.64↑)* |
| **plant**   | No Attack     | 93.75   | 26.60   | 0.24    |
|             | Camouflage     | **37.50** *(56.25↓)* | 11.87 *(14.73↓)* | **1.06** *(0.82↑)* |
|             | *Ours* Patch   | 62.50 *(31.25↓)* | **7.98** *(18.62↓)* | 0.73 *(0.49↑)* |
| **bed**     | No Attack     | 94.11   | 55.83   | 0.16    |
|             | Camouflage     | 94.11 *(0.00↓)* | 55.95 *(0.12↓)* | 0.16 *(0.00↑)* |
|             | *Ours* Patch   | **94.11** *(0.00↓)* | **46.44** *(9.39↓)* | **0.16** *(0.00↑)* |
| **chair**   | No Attack     | 100.00  | 65.18   | 0.22    |
|             | Camouflage     | 100.00 *(0.00↓)* | 58.97 *(6.21↓)* | 0.22 *(0.00↑)* |
|             | *Ours* Patch   | **83.33** *(16.67↓)* | **47.12** *(18.06↓)* | **0.34** *(0.12↑)* |
| **sofa**    | No Attack     | 100.00  | 57.28   | 0.36    |
|             | Camouflage     | 100.00 *(0.00↓)* | 50.45 *(6.83↓)* | 0.36 *(0.00↑)* |
|             | *Ours* Patch   | **78.57** *(21.43↓)* | **42.19** *(15.09↓)* | **0.52** *(0.16↑)* |
| **toilet**  | No Attack     | 81.25   | 43.32   | 1.32    |
|             | Camouflage     | 75.00 *(6.25↓)* | 36.85 *(6.47↓)* | 1.49 *(0.17↑)* |
|             | *Ours* Patch   | **56.25** *(25.00↓)* | **27.42** *(15.90↓)* | **1.93** *(0.61↑)* |
| **Average** | No Attack     | 94.85   | 51.87   | 0.39    |
|             | Camouflage     | 84.44 *(10.41↓)* | 45.08 *(6.79↓)* | 0.56 *(0.17↑)* |
|             | *Ours* Patch   | **72.46** *(22.39↓)* | **30.99** *(20.88↓)* | **0.89** *(0.50↑)* |


## Data Preparation 
Download the attack scenarios and model weights and place them in the corresponding directories: [https://drive.google.com/file/d/1dhUcv7MvavmHwG4E11L12KK6zkjP1Ui5/view?usp=drive_link](https://drive.google.com/file/d/1dhUcv7MvavmHwG4E11L12KK6zkjP1Ui5/view?usp=drive_link)

## Setting up the Environment for Testing Navigation Agents
Refer to [PEANUT](https://github.com/ajzhai/PEANUT) for environment setup
```python
# Download navigation dataset and model weights, place them in the corresponding directories
Physical-Attacks-in-Embodied-Navigation
├── PEANUT/
│   ├── habitat-challenge-data/
│   │   ├── objectgoal_hm3d/
│   │   │   ├── train/
│   │   │   ├── val/
│   │   │   └── val_mini/
│   │   └── data/
│   │       └── scene_datasets/
│   │           └── hm3d/
│   │               ├── train/
│   │               └── val/
│   └── test_patch/
│       └── models/
│           ├── pred_model_wts.pth
│           └── mask_rcnn_R_101_cat9.pth

# Build Docker (requires sudo)
cd PEANUT
./build_and_run.sh

# After building, enter docker to test navigation agents
conda activate habitat
export CUDA_VISIBLE_DEVICES=0
./nav_exp.sh

# If you need to save the current running Docker container as an image
docker commit <container ID or container name> <image name>
```

## Setting up the Environment for Optimizing Adversarial Patches
Refer to [REVAMP](https://github.com/poloclub/revamp) for environment setup
```python
# Set up conda environment
conda env create -f environment.yml 

# torch and cuda versions
pip install torch==1.10.0+cu111 torchvision==0.11.0+cu111 torchaudio==0.10.0 -f https://download.pytorch.org/whl/torch_stable.html  

# Install pre-built Detectron2 (https://detectron2.readthedocs.io/en/latest/tutorials/install.html): 
python -m pip install detectron2==0.6 -f https://dl.fbaipublicfiles.com/detectron2/wheels/cu111/torch1.10/index.html 
```

Set up the victim model
  - Victim model configuration directory: ```configs/model/mask_rcnn_R_101_cat9.yaml```
  - Victim model's configuration and weights directory: ```pretrained-models/mask_rcnn_R_101_cat9```

Set up the attack scenario
- Download example scenes in obj and xml formats and place them in the corresponding locations.
- Use the ```test-patch/test_generate_random.py``` script to generate an initial RGB texture and an initial transparency mask, install the Blender (3.6.0 version) plugin: import images as planes, import the initial adversarial patch (the image after merging the initial RGB texture and the initial transparency mask), and adjust the position: ![import-images-as-planes.png](docs/import-images-as-planes.png)
- Convert the obj format scene to xml format. First, install the plugin [Mitsuba-Blender](https://github.com/mitsuba-renderer/mitsuba-blender), follow the Mitsuba-Blender Installation tutorial to install. After installation, File -> Export -> Mitsuba(.xml) -> choose "Selection Only" and export.
- Place the xml exported from blender into scenes/00813-svBbv1Pavdk/00813-svBbv1Pavdk.xml, and the corresponding ply and png files also need to be placed in the relative directories meshes and textures.

## Generate adversarial patch
- Set the id for untargeted attack in the global configuration ```configs/config.yaml```, for example, TV, untarget_idx: 5

- Set the parameters to be optimized in the scene configuration, taking the scene config configs/scene/00813-svBbv1Pavdk.yaml as an example,
  - If you want to optimize camouflage, target_param_keys: ["elm__50.bsdf.reflectance.data"]
  - If you want to optimize the texture of the adversarial patch, target_param_keys: ["adversarial_patch.bsdf.nested_bsdf.reflectance.data"]
  - If you want to optimize the opacity of the adversarial patch, target_param_keys: ["adversarial_patch.bsdf.opacity.data"]
  

- Run the optimization script. If the memory is not enough, in configs/config.yaml, reduce samples_per_pixel, increase multi_pass_spp_divisor
```
export CUDA_VISIBLE_DEVICES=0
python revamp.py
```

## Test the attack effect of the adversarial patch on the navigation agent
- After optimization, export the attack scene's obj: ![export-obj.png](docs/export-obj.png)
- Convert the scene from obj format to glb format, ignore the obj file, the material format of the adversarial patch in the mtl file is as follows (d is opacity, map_Kd is texture, map_d is the transparency mask)
```python
newmtl random_gaussian_noise
d 0.20000
illum 1
map_Kd adversarial_patch.png
map_d adversarial_patch_mask.png
```

```python
# Install obj2gltf
conda install -c conda-forge nodejs
npm install -g obj2gltf
conda activate obj
# Use obj2gltf to convert
obj2gltf -i attack-scene-example.obj -o svBbv1Pavdk.basis.glb
```

- Copy to the navigation scene dataset directory, replace the original glb, for example:
```
cp -f svBbv1Pavdk.basis.glb /home/disk1/cm/Projects/PEANUT/habitat-challenge-data/data/scene_datasets/hm3d/val/00813-svBbv1Pavdk/
```

- In the PEANUT docker environment, run the navigation agent to test the attack effect (you can filter the task dataset to only include the target episode for testing)

## Object-aware Sampling
Use the blender script to generate camera positions, export xml, use test-patch/test_delete.py, delete id and name
![script](docs/script.png)
Script content:
```python
import bpy
import math
from mathutils import Euler

# Center position
# center = (8.86993, -0.88, 3.99213) # bed
center = (7.84387, -0.88, -3.82026) # tv

num = 40

# List of radii to create
radius_list = [2.50, 2.25, 2.00, 1.75, 1.50, 1.25, 1, 0.75]

# Create cameras
for radius in radius_list:
    for i in range(5, 20):  # Only create cameras with IDs 6 to 20
        # Calculate camera position
        angle = (2 * math.pi / num) * i
        pos_x = center[0] + radius * math.cos(angle)
        pos_z = center[2] + radius * math.sin(angle)
        
        # Create camera object
        bpy.ops.object.camera_add(location=(pos_x, -0.88, pos_z))
        camera = bpy.context.object
        
        # Set camera parameters
        camera.data.angle = math.radians(90)  # Field of view is 90 degrees
        camera.rotation_euler = Euler((0, math.radians(i * 360 / num - 90), math.radians(180)), 'XYZ')  # Set Euler angles
        
        # Set camera name
        camera.name = f"Camera_r{radius}_num{i+1}"
```

## Citation
Please cite our paper if you find this repo useful!
```bibtex
@article{chen2024towards,
  title={Towards Physically-Realizable Adversarial Attacks in Embodied Vision Navigation},
  author={Chen, Meng and Tu, Jiawei and Qi, Chao and Dang, Yonghao and Zhou, Feng and Wei, Wei and Yin, Jianqin},
  journal={arXiv preprint arXiv:2409.10071},
  year={2024}
}
```

## Acknowledgments
This project builds upon code from [REVAMP](https://github.com/poloclub/revamp),  [PEANUT](https://github.com/ajzhai/PEANUT).  We thank the authors of these projects for their amazing work!
