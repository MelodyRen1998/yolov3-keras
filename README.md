# yolov3-keras

Train YOLOv3 with custom images (to finish bachelor thesis XD).

I use the model from [this repo](https://github.com/experiencor/keras-yolo3), so pls fork that first.

## Install required modules

```python
pip install -r requirements.txt
```

It has been tested to work with Python 2.7.13 and 3.5.3. I used Python 3.7.0, and it works well too.

`tensorflow.__version__`: 1.14.0

`keras.__version__`: 2.3.1

## Training

### 1. Data Preparation

For my thesis topic, I collected around 117 badminton court images from campus, all of which contain ball elements. I used [labelImg](https://github.com/tzutalin/labelImg) to create bounding box **annotations** as _PASCAL VOC_ format. Data augmentation was inspired by [Augment Bounding Boxes](https://nbviewer.jupyter.org/github/aleju/imgaug-doc/blob/master/notebooks/B02%20-%20Augment%20Bounding%20Boxes.ipynb), which could support for bounding boxes and their augmentation. There are totally 1177 images after augmentation, which could be found in `./data/train_set` and `./data/test_set`.

If you are going to train YOLOv3 on your own data set, pls follow the instruction in [it](https://github.com/experiencor/keras-yolo3).

### 2. Edit the configuration file

When editting the `config.json` file, I changed the `labels`, `train_image_folder`, `train_annot_folder` and `saved_weights_name` corresponding to my own data. Besides, I reset the `gpus` and `batch_size` during my training.

Before training, it is **required** to download the pretrained weight for backend at [here](https://bit.ly/39rLNoE). This weight was trained on COCO dataset. As mentioned in the original repo, **the code does not work without this weights.**

### 3. Generate anchors for your dataset

Despite it is an optional step, I still run `python gen_anchors.py -c config.json` for expecting better performance.

### 4. Start training process

`python train.py -c config.json`

By the end of this process, the code will write the weights of the best model to the file specified in the setting "saved_weights_name" in the `config.json` file.

During the training process, you may go to TensorBoard and visualize the training loss.

`tensorboard --logdir ./your/log/path`

Visit http://localhost:8080 to find it.

### 5. Perform detection using trained weights on image, set of images, video, or webcam

I only detect image sets and it works well.

![](https://user-images.githubusercontent.com/35906792/77249469-258d8e80-6c7c-11ea-8992-635177009f74.jpg)

## Evaluation

`python evaluate.py -c config.json`

It would compute mAP of the model.
