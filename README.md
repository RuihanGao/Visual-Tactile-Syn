# Controllable Visual-Tactile Synthesis

### [Project Page](https://visual-tactile-synthesis.github.io/) | [Paper](https://arxiv.org/abs/2305.03051)

**Content creation beyond visual outputs**: We present an image-to-image method to synthesize the visual appearance and tactile geometry of different materials, given a handcrafted or DALL⋅E 2 sketch. We then render the outputs on a surface haptic device like TanvasTouch® where users can slide on the screen to feel the rendered textures. (Turn the audio ON to hear the sound of the rendering.)

<!-- [![Teaser video](https://img.youtube.com/vi/TdwPfwsGX3I/default.jpg)](https://youtu.be/TdwPfwsGX3I) -->
https://user-images.githubusercontent.com/33533196/236384202-c822f1fb-c7a0-4023-810e-a7995982ce35.mp4

***Controllable Visual-Tactile Synthesis*** <br>
[Ruihan Gao](https://ruihangao.com/), [Wenzhen Yuan](http://robotouch.ri.cmu.edu/yuanwz/), [Jun-Yan Zhu](https://www.cs.cmu.edu/~junyanz/)<br>
Carnegie Mellon University<br>
ICCV, 2023


### Visual-tactile synthesis
We show an example of our visual-tactile synthesis. The tactile output is shown in the 3D height map. The patches below correspond to bounding boxes shown in the sketch input. Please see our paper for more results.
<img src="assets/FlowerJeans_patches.png" alt=FlowerJeans_patches width="800">

### Colored 3D mesh for single object
We can also render the synthesized results as a colored 3D mesh. The meshes are exaggerated in z direction to show fine textures. <br>
<img src="assets/rendered_mesh_GreenTee_GreenTee.gif" alt="mesh_GreenTee" width="250">
<img src="assets/rendered_mesh_WhiteTshirt_WhiteTshirt.gif" alt="mesh_WhiteTshirt" width="250">
<img src="assets/rendered_mesh_NavyHoodie_NavyHoodie.gif" alt="mesh_NavyHoodie" width="250">
<br>
<img src="assets/rendered_mesh_FlowerJeans_FlowerJeans.gif" alt="mesh_FlowerJeans" width="250">
<img src="assets/rendered_mesh_PurplePants_PurplePants.gif" alt="mesh_PurplePants" width="250">
<img src="assets/rendered_mesh_FlowerShorts_FlowerShorts.gif" alt="mesh_FlowerShorts" width="250">
<br>

### Swapping different sketches & materials
<img src="assets/figure7_swap_sketch.png" alt=figure7_swap_sketch width="800">
<br>

### Text-guided visual-tactile synthesis
<img src="assets/figure8_DALLE_sketch.png" alt=figure8_DALLE_sketch width="800">
<br>

**Please see our website and paper for more interactive and comprehensive results**

<!-- ## Updates 
* [05/04] InitialWe release the [visual-tactile-syn arxiv](https://arxiv.org/abs/), which synthesizes synchronized visual-tactile output from a sketch/text input and renders the multi-modal output on a surface haptic device TanvasTouch®. -->

## Updates
We are plan to release our code and dataset in the following steps:
- [x] Inference and Evaluation code [05/04].
- [x] Preprocessed data of all 20 garments in our <i>TouchClothing</i> dataset [05/04].
- [x] Pretrained model (ours & baselines) on the <i>TouchClothing</i> dataset [05/04].
- [x] Training code [05/04].
- [ ] Data preprocessing code for camera and GelSight R1.5 data.
- [ ] Rendering code to generate friction maps for TanvasTouch.
- [ ] Instructions on how to create new test data.

## Getting Started
We tested our code with Python 3.8 and [Pytorch](https://pytorch.org/) 1.11.0. (We recommend installing PyTorch separately to avoid package conflicts.)
```
git clone https://github.com/RuihanGao/visual-tactile-synthesis.git
cd visual-tactile-synthesis
conda create -n VTS python=3.8
conda activate VTS
pip install torch==1.11.0+cu113 torchvision==0.12.0+cu113 torchaudio==0.11.0 --extra-index-url https://download.pytorch.org/whl/cu113
pip install -r requirements.txt
```

### Dataset
We provide the preprocessed data for our <i>TouchClothing</i> dataset, which contains 20 pieces of garments of various shapes and textures. Here are 20 objects in <i>TouchClothing</i> dataset:
<br>
<img src="assets/figure2_dataset.png" alt=“figure2_dataset” width="800">
<br>
Example of preprocessed data:
<br>
<img src="assets/figure4_sample_data.png" alt=“figure4_sample_data” width="800">
<br>

https://user-images.githubusercontent.com/33533196/236384165-7c451ac4-19f6-4001-8bd7-0273e94cf993.mp4

Use the following commands to download and unzip the dataset. <br>
(0) Install `gdown` and `unzip` as follows if you haven't done so.
```
pip install gdown
sudo apt install unzip
```
(1) Download the preprocessed data from Google Drive via the following command: 
Total size: 580M. <br>
```
bash scripts/download_TouchClothing_dataset.sh
```
<br>

(2) Put the unzipped folder `datasets` in the code repo.

Note: 
* in case there is "access denied" error, try `pip install -U --no-cache-dir gdown --pre` and run `gdown` command again. [Ref here](https://github.com/wkentaro/gdown/issues/43#issuecomment-621356443)
* use `-q` flag to `unzip` to suppress the log as it could be quite long

### Method
https://user-images.githubusercontent.com/33533196/236648641-4597ef2d-8e2f-4a47-97e9-2ce39e12f950.mp4

### Pre-trained models
We provide the pretrained models for our method and several baselines included in our paper. For each method, we provide 20 models, one for each object in our <i>TouchClothing</i> dataset.
See the Google Drive folder [here](https://drive.google.com/drive/folders/1ewamRPEyKir3jiPoeS7NBmZPZbFqxEoB?usp=sharing). To use them, <br>
(1) download the checkpoints <br>
* checkpoints for our method (124M): `gdown "https://drive.google.com/uc?export=download&id=11y2jP2vT7CtBIaEDcjROZ5hupHsYWG8D"`
* checkpoints for baselines (21.5G): `gdown "https://drive.google.com/uc?export=download&id=16NNU1GuOWWtarzEJkLSYbeSqQVaX-943"`

(2) After unzipping the files, put all pre-trained models in the folder `checkpoints` to load them properly in the testing code.

(3) See the [testing section](#test-our-model) for more examples of how to evaluate the pretrained models.

## Usage
In general, our pipeline contains two steps. We first feed the sketch input to our model to synthesize synchronized visual and tactile output. Then we convert the tactile output to a friction map required by TanvasTouch and render the multi-modal output on the surface haptic device, where you can <i>see</i> and <i>feel</i> the object simultaneously.

### Train our model
```
material=BlackJeans
CUDA_VISIBLE_DEVICES=0 python train.py  --gpu_ids 0 --name "${material}_sinskitG_baseline_ours" --model sinskitG --dataroot ./datasets/"singleskit_${material}_padded_1800_x1/"
```
where you can choose the variable `material` from our <i>TouchClothing</i> dataset or your own customized dataset.

To use our launcher scripts to run multiple experiments in tmux window, use the following command: <br>
([Ref here](https://github.com/taesungp/contrastive-unpaired-translation#training-using-our-launcher-scripts) for more examples and explanations for tmux launcher)
```
material_idx=0
python -m experiments SingleG_AllMaterials_baseline_ours launch $material_idx
```
where the material_idx set which object in the dataset to use. Choose a material_idx or use 'all' to run multiple experiments at once.
The list of the material can be found in the launcher file `experiments/SingleG_AllMaterials_baseline_ours_launcher.py` 

Note: Loading the dataset to cache before training may take up to 20-30 mins and the training takes about 16h on a single A5000 GPU. Please be patient. <br>
For a proof-of-concept training, set `data_len` in `SingleG_AllMaterials_baseline_ours_launcher` and `verbose_freq` in `models/sinskitG_model.py` to a smaller number (e.g., 3 or 10).

### Test our model

```
material=BlackJeans
CUDA_VISIBLE_DEVICES=0 python test.py  --gpu_ids 0 --name "${material}_sinskitG_baseline_ours" --model sinskitG --dataroot ./datasets/"singleskit_${material}_padded_1800_x1/" --epoch best --eval
```

Or, if you are using `tmux_launcher`, use the following command.
```
material_idx=0
python -m experiments SingleG_AllMaterials_baseline_ours test $material_idx
```
The results will be stored in the `results` directory. <br>
To compile the quantitative metrics of the tested method in a tabulated format, run `bash scripts/compile_eval_metrics_sinskitG.sh`. For each method, it retrieves the `eval_metrics.pkl` file of all materials and take the average. Modify `materials` list in `util/compile_eval_metrics_sinskitG.py` and the bash script accordingly.


## Citation
``` bibtex
@inproceedings{gao2023controllable,
  title={Controllable Visual-Tactile Synthesis},
  author={Gao, Ruihan and Yuan, Wenzhen and Zhu, Jun-Yan},
  booktitle={IEEE International Conference on Computer Vision (ICCV)},
  year={2023},
}
```

## Acknowledgment
We thank Sheng-Yu Wang, Kangle Deng, Muyang Li, Aniruddha Mahapatra, and Daohan Lu for proofreading the draft. We are also grateful to Sheng-Yu Wang, Nupur Kumari, Gaurav Parmar, George Cazenavette, and Arpit Agrawal for their helpful comments and discussion. Additionally, we thank Yichen Li, Xiaofeng Guo, and Fujun Ruan for their help with the hardware setup. Ruihan Gao is supported by A*STAR National Science Scholarship (Ph.D.).

Our code base is built upon [Contrastive Unpaired Translation (CUT)](https://github.com/taesungp/contrastive-unpaired-translation).
