title: macOS安装tensor flow 基于 Anaconda 的安装

date: 2019-12-09

------

## 1.安装Anaconda
[下载地址](https://www.anaconda.com/distribution/#macos)

下载完后命令行无法识别conda

原因是环境变量没有配置好，终端输入

```
vi ~/.bash_profile
```

然后如果没有的话就在环境变量里加一句：

```
export PATH="/Users/Mac/anaconda3/bin:$PATH"
```

source ~/.bash_profile### ，刷新环境变量

## 2.建立一个叫tensorflow的conda计算环境

```
conda create -n tensorflow python=3.6
```

## 3.激活tensorflow环境

```
source activate tensorflow
```

## 4.在该环境中安装tensorflow

```
conda install tensorflow
```

## 5.关闭conda环境
```
anado deactivate
```
### 要使用tensorflow的时候激活环境即可
```
source activate tensorflow
```
## 6.测试代码
忽略警告
```
import os 
os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'
```

```
import tensorflow as tf
/#查看tensorflow版本/
print(tf.__version__)
```

```
import tensorflow as tf

tf.compat.v1.disable_eager_execution()

hello = tf.constant('Hello, TensorFlow!')

sess = tf.compat.v1.Session()

print(sess.run(hello))
```