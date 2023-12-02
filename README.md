# Libfewshot-smu
## 1shot
复现：在run_trainer.py文件下修改：

````
config = Config("./1shot/config.yaml").get_config_dict()
`````


## 5shot
复现：在run_trainer.py文件下修改：

````
config = Config("./5shot/config.yaml").get_config_dict()
`````

使用SKDModel-Gen0做为classifier并加载了预先训练效果较为理想的SKDModel的cls、emb部分
``
cls_classifier_path: ./5shot/checkpoints/cls_classifier_best.pth

emb_func_path: ./5shot/checkpoints/emb_func_best.pth
``

使用resnet12作为backbone

在数据预处理阶段使用了数据增强

在训练的过程中使用CrossEntropyLoss作为损失函数、SGD作为优化函数，学习率设为0.01

由于学习率对模型训练的影响，我们发现在一些特定的epoch上模型很快过拟合，因此使用了MultissssStepLR作为学习率的调整程序，分别在epoch为30、40、50、60、70、80和100上进行学习率的衰减以减少模型过拟合效果

``

lr_scheduler:

  kwargs:
  
    gamma: 0.9
    
    milestones:
    
    - 30

    - 40
    
    - 50
    
    - 60
    
    - 70
    
    - 80
    
    - 100
    
  name: MultiStepLR
``

其他超参数：

``

query_num: 11

seed: 0

shot_num: 5

tag: null

tb_scale: 1.0

test_episode: 300

test_epoch: 5

test_query: 11

test_shot: 5

test_way: 5

train_episode: 300

use_loss_yaml: true

use_memory: false

val_per_epoch: 1

warmup: 0

way_num: 5

workers: 8

``

具体训练模型详见5shot文件夹下config.yaml
