## Adversarial example generator

This script generates adversarial examples for convolutional neural networks using fast gradient sign method (FGSM) 
presented in [Explaining and harnessing adversarial examples](https://arxiv.org/abs/1412.6572) (Goodfellow et al. 2015).

It is a more general version of the code-base provided [here]() which at the time of initiation of this project only 
worked for [OverFeat](https://github.com/sermanet/OverFeat) network architecture and for a single image input.

## Functionalities incorporated

- Supports any model architecture that can be loaded into torch
    - Has been tested for VGG, Overfeat, [Attention-mounted-VGG] models()
- Can be used to generate adversaries for any number of images (single or in bulk) using any supported model
- Can be used to evaluate the robustness of models to adversarial perturbations for any number of images (single or in bulk)

## Modes of operation

The following command-line parameters can be used to customize the operation of the program:

- action
    - 'generate' to generate adversarial versions of input images
    - 'evaluate' to evaluate the robustness of given model to adversarial perturbations to input images
- mode
    - 'unproc' for receiving unnormalized (not normalized for mean and std-dev) images available in raw format in a given folder
        - path_img = 'data.t7',
    - 'preproc' for receiving normalized images available in t7 format
        - path_label = '#dataset/label_gt.lua', -- label file (in order*)
        - path_img = '#dataset/image_gt.lua', -- image file (in order*)
        - list_labels = '#dataset/overfeat_label.lua',

- path_model 
    - pass a single-entry table containing the path to model wts. for standard architectures (VGG/Overfeat): '{"#overfeat-torch/model.net"}'
    - pass a 2-entry table containing the path to the prototxt and wt. file for caffe models :
      '{"#models/imagenet-vgg19/VGG_ILSVRC_19_layers_deploy.prototxt","#models/imagenet-vgg19/VGG_ILSVRC_19_layers.caffemodel"}'
    - pass a multi-entry table containing the path to sub-model wts. for [attention-mounted archi.]():
      '{"#models/cubs-2level-1global/mlocal_1.net","#models/cubs-2level-1global/mlocal_2.net",
       "#models/cubs-2level-1global/mglobal_2.net","#models/cubs-2level-1global/matten_1.net",
       "#models/cubs-2level-1global/matten_2.net","#models/cubs-2level-1global/mmatch.net"}'
- type_model
    - 'torch' for receiving torch models
    - 'caffe' for receiving caffe models
- atten
    - 0 for models with no inbuilt attention
    - 1/2/3 for models with attention at 1/2/3 levels
- batch_size int
- image_size int
    - should match with the acceptable input size for the incoming models
- norm_range int (default=1)
    - for variance normalization
- noise_intensity int (deafult=1)
    - L-inf norm for gradient sign
- path_save
    - folder names where the adversaries should be saved
- mean = '{118.380948/255}',   -- global mean used to train overfeat
- std = '{61.896913/255}',     -- global std used to train overfeat
- platformtype = 'cuda',
- gpumode = 1,
- gpusetdevice = 1,


The script can be provided the following inputs:
- mode: preproc: Path to the image folder which contains images for generating adversarial examples on. Things to check inside the code for this:
	-- Size of the input image which needs to match the input layer of the architecture
	-- Mean (for subtraction)
	-- Standard deviation (for division)
	-- Path to the ground truth labels (if not available with the images)
- mode: unproc: Path to the t7 file containing images and labels (both)
- Path of the trained model

###Dependency

This script requires trained .
Running the `example.lua` or the snippet will automatically create the model
and download other files.

```bash
git clone https://github.com/jhjin/overfeat-torch
cd overfeat-torch
. install.sh
th run.lua
mv model.t7 bee.jpg overfeat_label.lua ..
cd ..
```


### Example

The example script predicts the output category of original and its adversarial examples.

```bash
th example.lua
```

![](example.png)
