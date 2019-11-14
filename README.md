# Requirements
  
  Python2.7

  Caffe

  Pytorch >= 0.40
  
# Demo
  $ python darknet2caffe.py cfg[in] weights[in] prototxt[out] caffemodel[out]
  
  Example
```
python darknet2caffe.py cfg/yolov3.cfg weights/yolov3.weights prototxt/yolov3.prototxt caffemodel/yolov3.caffemodel
```
