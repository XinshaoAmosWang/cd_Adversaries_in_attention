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
        - path_label = '#dataset/label_gt.lua', -- label file (in order*)
        - path_img = '#dataset/image_gt.lua', -- image file (in order*)
        - list_labels = '#dataset/label_names.lua',
    - 'preproc' for receiving normalized images available in t7 format
        - path_img = 'data.t7',

- path_model 
    - pass a single-entry table containing the path to model wts. for standard architectures (VGG/Overfeat): <br/>
    '{"#overfeat-torch/model.net"}'
    - pass a 2-entry table containing the path to the prototxt and wt. file for caffe models :<br/>
      '{"#models/imagenet-vgg19/VGG_ILSVRC_19_layers_deploy.prototxt","#models/imagenet-vgg19/VGG_ILSVRC_19_layers.caffemodel"}'
    - pass a multi-entry table containing the path to sub-model wts. for [attention-mounted archi.]():<br/>
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
- image_size int: should match with the acceptable input size for the incoming models
- norm_range int (default=1): for variance normalization
- noise_intensity int (deafult=1): L-inf norm for gradient sign
- path_save: folder names where the adversaries should be saved
- mean (default='{118.380948/255}'): global mean used to train overfeat
- std  (default='{61.896913/255}'): global std used to train overfeat
- platformtype (default='cuda')
- gpumode (0 for cpu/ 1 for gpu)
- gpusetdevice (default=1)


### Dependency

- Model and data files need to be made available*


### Example

- Make available a sample script to give a demo of the code-run* (similar to the .sh files)
