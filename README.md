# YOLACT++_road-markings
  ## 1. Converts the format of VGG Image Annotator (VIA) to the COCO format.
  Classes:straight arrow, left arrow, right arrow, straight left arrow, straight right arrow,  pedestrian crossing, special lane
  ## 2. YOLACT++ environment
  * Operating System: Ubuntu 20.04.4
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
  * project name: My Dataset
  * modify train/ val images and annotations path
  * modify class names
  * modify label map to match the coco class ids

  ```python
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
  ```

  * modify num_classes (classes number + background)
  * modify training parameters
  * num_epoch: max_iter // [(len(dataset) // batch_size)]
  * iterations per epoch: len(dataset) // batch_size
  ```python 
  yolact_base_config = coco_base_config.copy({
  'name': 'yolact_base',

  # Dataset stuff
  'dataset': dataset_base,
  'num_classes': 7 + 1,

  # Image Size
  'max_size': 700, 
    
  # Training params
  'lr_steps': (2800, 6000, 7000, 7500),
  'max_iter': 35000,  ## num_epoch:max_iter// [(len(dataset) // batch_size)]  []:iterations
  ```
  
  ## 4. Training
  batch size=8 (default)
  
      python train.py --config=yolact_plus_base_config
      
  ## 5. Evaluation
      python eval.py --trained_model=weights/yolact_plus_base_100_35000.pth
      
  ## 6. Visulization
      python eval.py --trained_model=weights/yolact_plus_base_100_35000.pth --score_threshold=0.15 --top_k=15 --images=/home/user/Datasets/data/my_dataset       /TW_homo/JPEGImages:/mnt/disk/Chun/yolact/output/TW_homo
      
  ## 7. Result
  Train with the bird's eye view image model 
  
  Front View
  ![image](https://github.com/yichun-hub/YOLACT_road-markings/blob/main/result/TW_125.png)
  
  Bird's eye view
  ![image](https://github.com/yichun-hub/YOLACT_road-markings/blob/main/result/homo_TW_125.png)
