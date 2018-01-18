
# SPN: Learning Affinity via Spatial Propagation Network

## License

Copyright (C) 2018 NVIDIA Corporation.  All rights reserved.
Licensed under the CC BY-NC-SA 4.0 license (https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode). 


## Installation

The codes for [Learning Affinity via Spatial Propagation Networks](https://papers.nips.cc/paper/6750-learning-affinity-via-spatial-propagation-networks.pdf) is based on [CAFFE](http://caffe.berkeleyvision.org/). To install, 

$cd caffe-dev 

and then follow the [Installation](http://caffe.berkeleyvision.org/installation.html) to install all neccesary dependencies, then 

$make all -j

$make pycaffe


## SPN layer and integration

If you need to integrate the SPN layer into your own caffe, do the following steps and rebuild:

1. copying "caffe-dev/src/caffe/layers/gaterecurrent2dnoind_layer.cpp", "caffe-dev/src/caffe/layers/gaterecurrent2dnoind_layer.cu" into "<your caffe root>/src/caffe/layers".

2. copying "caffe-dev/include/caffe/layers/gaterecurrent2dnoind_layer.hpp" into "<your caffe root>/include/caffe/layers".

3. open "<your caffe root>/src/caffe/proto/caffe.proto", add:

	a. "optional GateRecurrent2dnoindParameter gaterecurrent2dnoind_param = \<next avaliable id\>;" under message LayerParameter;
	
    b. add the following to your caffe.proto:
    
    	message GateRecurrent2dnoindParameter {
            optional uint32 num_output = 1 [default = 16]; 
            optional bool horizontal = 16 [default = true];
            optional bool reverse = 17 [default = false]; //recurrent direction

            enum Active {
            LINEAR = 0; 
            SIGMOID = 1; 
            RELU = 2; 
            TANH = 3; 
            }    
            optional Active active = 18 [default = LINEAR];
        }
    or searching "caffe-dev/src/caffe/proto/caffe.proto" for details.

## Pytorch ext for spn layer

We develop the spn layer ext for pytorch [HERE](https://github.com/Liusifei/pytorch_spn.git). Note that we did not re-implement the vision tasks in the paper on pytorch yet. However, you are encouraged to apply this module to any vision tasks you are working on.

## Training

The example of datalayer and trainig scripts are in [pylayers/vocspn_datalayer.py](https://github.com/Liusifei/caffe-spn/blob/master/pylayers/vocspn_datalayer.py) and [myscripts/train.py](https://github.com/Liusifei/caffe-spn/blob/master/myscripts/train.py). To train the spn segmentation refinement model:

1. adding your dataset path to the datalayer (line 10 in [models/voc_rnn_vgg_v3.prototxt](https://github.com/Liusifei/caffe-spn/blob/master/models/voc_rnn_vgg_v3.prototxt));

2. You could download the segmentation restuls (probabilistic outputs) of FCN-8s on VOC training set from [HERE](https://www.dropbox.com/sh/x2i1wgplvau0a7t/AADu_3YOSLZVL4g91H109bz7a?dl=0) as an example to train your refinement moded. Please put it on the corresponding VOC folders. You could centainly design new algorithms by applying other coarse maps for you own tasks.

3. run:

	{$ python myscripts/train.py}


## Testing

## Coarse segmentation mask example
The example of coarse segmentation results (probability output), generated by deeplabv2 Resnet-101 can be found [HERE](https://www.dropbox.com/sh/x2i1wgplvau0a7t/AADu_3YOSLZVL4g91H109bz7a?dl=0). Nevertheless, the provided refinement model can be generalized to many segmentation results.
