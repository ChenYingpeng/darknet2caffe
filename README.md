# Requirements
  
  Python2.7

  Caffe

  Pytorch >= 0.40
# Add Caffe Layers
1. Copy `caffe_layers/mish_layer/mish_layer.hpp,caffe_layers/upsample_layer/upsample_layer.hpp` into `include/caffe/layers/`.
2. Copy `caffe_layers/mish_layer/mish_layer.cpp mish_layer.cu,caffe_layers/upsample_layer/upsample_layer.cpp upsample_layer.cu` into `src/caffe/layers/`.
3. Copy `caffe_layers/pooling_layer/pooling_layer.cpp` into `src/caffe/layers/`.Note:only work for yolov3-tiny,use with caution.
4. Add below code into `src/caffe/proto/caffe.proto`.

```
// LayerParameter next available layer-specific ID: 147 (last added: recurrent_param)
message LayerParameter {
  optional TileParameter tile_param = 138;
  optional VideoDataParameter video_data_param = 207;
  optional WindowDataParameter window_data_param = 129;
++optional UpsampleParameter upsample_param = 149; //added by chen for Yolov3, make sure this id 149 not the same as before.
++optional MishParameter mish_param = 150; //added by chen for yolov4,make sure this id 150 not the same as before.
}

// added by chen for YoloV3
++message UpsampleParameter{
++  optional int32 scale = 1 [default = 1];
++}

// Message that stores parameters used by MishLayer
++message MishParameter {
++  enum Engine {
++    DEFAULT = 0;
++    CAFFE = 1;
++    CUDNN = 2;
++  }
++  optional Engine engine = 2 [default = DEFAULT];
++}
```
5.remake caffe.

# Demo
  $ python cfg[in] weights[in] prototxt[out] caffemodel[out]
  
  Example
```
python cfg/yolov4.cfg weights/yolov4.weights prototxt/yolov4.prototxt caffemodel/yolov4.caffemodel
```
  partial log as below.
```
I0522 10:19:19.015708 25251 net.cpp:228] layer1-act does not need backward computation.
I0522 10:19:19.015712 25251 net.cpp:228] layer1-scale does not need backward computation.
I0522 10:19:19.015714 25251 net.cpp:228] layer1-bn does not need backward computation.
I0522 10:19:19.015718 25251 net.cpp:228] layer1-conv does not need backward computation.
I0522 10:19:19.015722 25251 net.cpp:228] input does not need backward computation.
I0522 10:19:19.015725 25251 net.cpp:270] This network produces output layer139-conv
I0522 10:19:19.015731 25251 net.cpp:270] This network produces output layer150-conv
I0522 10:19:19.015736 25251 net.cpp:270] This network produces output layer161-conv
I0522 10:19:19.015911 25251 net.cpp:283] Network initialization done.
unknow layer type yolo 
unknow layer type yolo 
save prototxt to prototxt/yolov4.prototxt
save caffemodel to caffemodel/yolov4.caffemodel

```
