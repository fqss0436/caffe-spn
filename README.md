# Installation
The codes for [Learning Affinity via Spatial Propagation Networks](https://papers.nips.cc/paper/6750-learning-affinity-via-spatial-propagation-networks.pdf) is based on [CAFFE](http://caffe.berkeleyvision.org/). To install, 

$cd caffe-dev 

and then follow the [Installation](http://caffe.berkeleyvision.org/installation.html) to install all neccesary dependencies, then 

$make all -j

$make pycaffe


# SPN layer integration
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