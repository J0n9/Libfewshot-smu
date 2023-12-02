# Libfewshot-smu
## 1shot
## 5shot
复现：在run_trainer.py文件下修改：

````
config = Config("5shot/config.yaml").get_config_dict()
`````

使用SKDModel-Gen0做为classifier并加载了预先训练效果较为理想的SKDModel的cls、emb部分
![image](https://github.com/J0n9/Libfewshot-smu/assets/77601178/e4cffa8b-3346-4f70-9fae-6e7acb83c5d8)

使用resnet12作为backbone

在数据预处理阶段使用了数据增强

在训练的过程中使用CrossEntropyLoss作为损失函数、SGD作为优化函数，学习率设为0.01

由于学习率对模型训练的影响，我们发现在一些特定的epoch上模型很快过拟合，因此使用了MultissssStepLR作为学习率的调整程序，分别在epoch为30、40、50、60、70、80和100上进行学习率的衰减以减少模型过拟合效果

![image](https://github.com/J0n9/Libfewshot-smu/assets/77601178/a4e607b2-0f39-431b-9418-2ae67344fd13)

具体训练模型详见5shot文件夹下config.yaml
