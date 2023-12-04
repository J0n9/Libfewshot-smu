# Libfewshot-smu
## 1shot
复现：

在run_trainer.py文件下修改：

````
config = Config("./1shot/config.yaml").get_config_dict()
`````

在终端中输入：

````
python run_trainer.py
````

使用SKDModel-Gen0做为classifier并加载了预先训练效果较为理想的SKDModel的cls、emb部分，预先训练的SKDModel的cls、emb部分是由多组训练调参所得到的效果较好的模型所保存，这个过程实际上是SKDModel迭代的过程

````
cls_classifier_path: ./results/SKD-gen0-resnet12-5-1-83.756/checkpoints/cls_classifier_best.pth # 在复现训练效果时可将路径修改为: ./1shot/checkpoints/cls_classifier_best.pth
emb_func_path: ./results/SKD-gen0-resnet12-5-1-83.756/checkpoints/emb_func_best.pth # 在复现训练效果时可将路径修改为: ./1shot/checkpoints/cls_classifier_best.pth
```` 

在数据预处理阶段使用了数据增强

````
augment: true
augment_times: 5
augment_times_query: 1
````

使用了StepLR学习率调节程序每5个epoch将学习率降低0.9倍：

````
lr_scheduler:
  kwargs:
    gamma: 0.9
    step_size: 5
  name: StepLR
````


其他超参数：

````
query_num: 15
seed: 0
shot_num: 1
tag: null
tb_scale: 1.0
test_episode: 1000
test_epoch: 5
test_query: 15
test_shot: 1
test_way: 5
train_episode: 300
use_loss_yaml: true
use_memory: false
val_per_epoch: 1
warmup: 0
way_num: 5
workers: 8
````

具体训练过程详见1shot文件夹下config.yaml




## 5shot
复现：

在run_trainer.py文件下修改：

````
config = Config("./5shot/config.yaml").get_config_dict()
`````

在终端中输入：

````
python run_trainer.py
````


使用SKDModel-Gen0做为classifier并加载了预先训练效果较为理想的SKDModel的cls、emb部分。预先训练的SKDModel的cls、emb部分是由多组训练调参所得到的效果较好的模型所保存，这个过程是SKDModel迭代的过程

````
cls_classifier_path: /home/yangxinzhe/WebCaricature/SMU_V4/1shot/checkpoints/cls_classifier_best.pth # 在复现训练效果时可将路径修改为: ./5shot/checkpoints/cls_classifier_best.pth
emb_func_path: /home/yangxinzhe/WebCaricature/SMU_V4/1shot/checkpoints/emb_func_path.pth # 在复现训练效果时可将路径修改为: ./5shot/checkpoints/emb_func_best.pth
````

使用resnet12作为backbone

在数据预处理阶段使用了数据增强

````
augment: true
augment_times: 5
augment_times_query: 1
````

在训练的过程中使用CrossEntropyLoss作为损失函数、SGD作为优化函数，学习率设为0.01

由于学习率对模型训练的影响，我们发现在一些特定的epoch上模型很快过拟合，因此使用了MultissssStepLR作为学习率的调整程序，分别在epoch为30、40、50、60、70、80和100上进行学习率的衰减以减少模型过拟合效果

````
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
````

其他超参数：

````
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
````

具体训练过程详见5shot文件夹下config.yaml
