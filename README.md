# YOLACT++_road-markings
  ## 1. Converts the format of VGG Image Annotator (VIA) to the COCO format.
  classes:straight arrow, left arrow, right arrow, straight left arrow, straight right arrow,  pedestrian crossing, special lane
  ## 2. YOLACT++ environment
  * Operating System: Ubuntu
  * GPU: NVIDIA GeForce RTX3090
  * CUDA 11.0
  * pytorch 1.7.1
  * torchvision 0.8.2
  * python 3.7
  YOLACT++ please compile DCNv2
      cd external/DCNv2
      python setup.py build develop
  !! for the version of pytorch 1.7.0 ↑ , please download https://github.com/MatthewHowe/DCNv2
  ## 3. Modify Congig.py
      dataset_base = Config({
    'name': 'My Dataset',

    # Training images and annotations
    'train_images': '/home/user/Datasets/data/my_dataset/train_front_2data/JPEGImages',
    'train_info':   '/home/user/Datasets/data/my_dataset/train_front_2data/annotations.json',

    # Validation images and annotations.
    'valid_images': '/home/user/Datasets/data/my_dataset/val_front_2data/JPEGImages',
    'valid_info':   '/home/user/Datasets/data/my_dataset/val_front_2data/annotations.json',

    # Whether or not to load GT. If this is False, eval.py quantitative evaluation won't work.
    'has_gt': True,

    # A list of names for each of you classes.
    'class_names': ('straight arrow', 'left arrow', 
    'right arrow', 'straight left arrow', 'straight right arrow', 
    'pedestrian crossing', 'special lane'),

    # COCO class ids aren't sequential, so this is a bandage fix. If your ids aren't sequential,
    # provide a map from category_id -> index in class_names + 1 (the +1 is there because it's 1-indexed).
    # If not specified, this just assumes category ids start at 1 and increase sequentially.
    'label_map': {1:1, 2:2, 3:3, 4:4, 5:5, 6:6, 7:7}
    })
