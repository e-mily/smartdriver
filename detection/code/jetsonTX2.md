## Training the Model on Jetson TX2

Image classification training data samples are simply images that have been labeled by class (generally a tiny image or patch containing a single object) 
(typically integer class ID or a string class name). Object detection, on the other hand, necessitates a greater amount of data for training. The training 
data samples for DetectNet are larger photos with several items. The training label for each object in the image must capture not only the object's class 
but also the coordinates of its bounding box's corners. A simplistic choice of label format with changing length and dimensionality would make designing 
a loss function problematic because the number of objects can vary across training photos.

```
$ cd jetson-inference/python/training/detection/ssd
$ python3 train_ssd.py --dataset-type=voc --data=data/<YOUR-DATASET> --model-dir=models/<YOUR-MODEL>
```
PyTorch model to ONNX:
```
$ python3 onnx_export.py --model-dir=models/<YOUR-MODEL>
```

The converted model will then be saved under <YOUR-MODEL>/ssd-mobilenet.onnx, which can then load with the detectnet programs:
```
NET=models/<YOUR-MODEL>

detectnet --model=$NET/ssd-mobilenet.onnx --labels=$NET/labels.txt \
          --input-blob=input_0 --output-cvg=scores --output-bbox=boxes \
            csi://0
```
