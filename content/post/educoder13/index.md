---
title : 'Educoder13'
date : 2023-12-19T03:11:31Z
tags : ['Educoder']
draft : false
---
# 头歌-DeepDream

## Part 1

```python
from keras import backend as K
import matplotlib
matplotlib.use('Agg')
from matplotlib import pyplot as plt
import numpy as np


def feature_map_vis(model, image):
    '''
    将model的第6层所生成最后4个特征图可视化，并保存到./step1/result/feature_map.png中
    :param model: VGG16模型
    :param image: 待输入的图像，类型为ndarray，shape为(224, 224, 3)
    :return: 无
    '''

    #********* Begin *********#
    getf = K.function([model.layers[0].input], [model.layers[6].output])
    fimgs = getf([np.expand_dims(image, axis=0)])[0]
    for i in range(4):
        show_img = fimgs[:, :, :, i]
        show_img.shape = [fimgs.shape[1], fimgs.shape[2]]
        plt.subplot(1, 4, i+1)
        plt.imshow(show_img, cmap='gray')
        plt.axis('off')
    plt.savefig('./step1/result/feature_map.png')
    #********* End *********#
```

## Part 2

```python
from keras import backend as K
import numpy as np


def update_img(epochs, input_layer, feature_map, learning_rate):
    '''
    随机生成1张宽224，高224的RGB图像，并更行图像
    :param epochs: 梯度上升的迭代次数
    :param input_layer: CNN模型的输入层
    :param feature_map: CNN模型的指定的特征图
    :param learning_rate: 梯度上升的学习率
    :return: 更新后的图像和，其中图像的维度为(图像的高，图像的宽，通道数)
    '''

    #********* Begin *********#
    # 创建一个随机初始化的图像
    img = np.random.randint(low=0, high=255, size=(1, 224, 224, 3))
    img = img / 255.0  # 归一化处理

    # 定义损失函数为特征图的均值
    loss = K.mean(feature_map)

    # 计算损失函数对图像的梯度并进行归一化
    grads = K.gradients(loss, input_layer)[0]
    grads = grads / (K.sqrt(K.mean(K.square(grads))) + K.epsilon())

    # 定义前向传播操作
    iterate = K.function([input_layer], [loss, grads])

    # 迭代epochs次梯度上升
    for _ in range(epochs):
        # 得到当前的损失值与梯度
        loss_value, grads_value = iterate([img])
        # 根据梯度上升的公式更新图像
        img += grads_value * learning_rate
        # 如果更新不动了就提前返回
        if loss_value <= K.epsilon():
            break

    # 返回更新后的图像
    return img[0]
    #********* End *********#
```

## Part 3
```python
from keras import backend as K
import numpy as np
def deep_dream(img, model, epochs, learning_rate, max_threshold):
    '''
    将VGG16的block4_conv1与block5_conv2用来做deep dream，其中block4_conv1的权重为0.05，block5_conv2的权重为0.2，并将图像返回
    :param img: 预处理后的图像，类型为ndarray，shape为(样本个数，图像的宽，图像的高，通道数)
    :param model: VGG16
    :param epochs: 梯度上升的迭代次数
    :param learning_rate: 梯度上升的学习率
    :param max_threshold: 损失值的最大阈值
    :return: deep dream后的图像，其中图像的维度为(图像的高，图像的宽，通道数)
    '''
    #********* Begin *********#
    layer_contributions = {
        'block4_conv1': 0.05,
        'block5_conv2': 0.2
    }
    # 定义一个包含损失的张量，即上面列出的激活的L2范数的加权和。
    layer_dict = dict([(layer.name, layer) for layer in model.layers])
    # 定义损失
    loss = K.variable(0.)
    for layer_name in layer_contributions:
        # 将丢失的图层特征的L2范数添加到损失中。
        coeff = layer_contributions[layer_name]
        activation = layer_dict[layer_name].output
        # 通过仅涉及损失中的非边界像素来避免边界伪影。
        scaling = K.prod(K.cast(K.shape(activation), 'float32'))
        loss += coeff * K.sum(K.square(activation[:, :, :, :])) / scaling
    
    dream = model.input
    # 计算损失的梯度的变化
    grads = K.gradients(loss, dream)[0]
    # 标准化梯度
    grads /= K.maximum(K.mean(K.abs(grads)), 1e-7)
    # 设置函数以检索给定输入图像的损失和梯度值。
    outputs = [loss, grads]
    fetch_loss_and_grads = K.function([dream], outputs)
    def eval_loss_and_grads(x):
        outs = fetch_loss_and_grads([x])
        loss_value = outs[0]
        grad_values = outs[1]
        return loss_value, grad_values
    for i in range(epochs):
        loss_value, grad_values = eval_loss_and_grads(img)
        if max_threshold is not None and loss_value > max_threshold:
            break
        img = img + learning_rate * grad_values
    return img
    #********* End *********#
```