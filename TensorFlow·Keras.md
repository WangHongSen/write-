# Keras 

## prepare

import tensorflow as tf
from tensorflow import keras

使用tf.keras.__version__输出TensorFlow中的kerasAPI的版本，与keras并不一定一致
默认使用checkpoint.format保存模型权重， 通过save_format='h5'来使用HDF5格式保存权重

### 简单的线性模型

```
#创建
model = keras.Sequential()
#添加一个全连接层(densely-connected)
model.add(keras.layers.Dense(64,activation='relu'))
#添加一个含有是个单元的softmax输出层
model.add(keras.layers.Dense(10,activation='softmax))
#配置
layers.Dense(64,activation='sigmoid')
layers.Dense(64,activation=tf.sigmoid)
layers.Dense(64,kernel_regularizer=keras.regularizars.l1(0.01))
layers.Dense(64,bias_regularizer=keras.regularizars.l2(0.01))
layers.Dense(64,kernel_initializer='orthogonal')
layers.Dense(64,bias_initializer=keras.initializers.constant(2.0))
model.compile(optimizer=tf.train.AdamOptimizer(0.001),loss='categorical_crossentropy',metrics=['accuracy'])
data = np.random.random((1000, 32))
labels = np.random.random((1000, 10))

val_data = np.random.random((100, 32))
val_labels = np.random.random((100, 10))

model.fit(data, labels, epochs=10, batch_size=32,
          validation_data=(val_data, val_labels))
```
layers参数
activation layer的激活函数，应该是一个built-in函数的名字或者可调用对象。默认没有激活函数。
kernel_initializer(bias_initializer)权重的初始化方式，参数同activation。
kernel_regularizer(bias_regularizer)应用在权重上的正则化项。默认无正则化项。

tf.keras.Model.compile参数
optimizer:指定训练的程序，optimizer应该是tf.train模块实例，例如：AdaOptimizer, RMSPropOptimizer, GradientDescentOptimizer
loss 损失函数，应该是一个函数名或者一个tf.keras.losses模块的可调用实例
metrics 用于监督训练过程，是字符串名字或者tf.keras.metrics模块中的实例。

tf.keras.Model.fit参数
epochs, keras将训练过程结构化城epochs，一个epochs可以理解为一次对整个输入数据集（batch）的iteration。
batch_size
validation_data 一个元组，用于展示每次epochs后的loss和metrics的inference mode

### 数据集 tf.data
### evaluate and predict

## functional API
tf.keras.Sequential模型只是layers的简单stack，无法构建任意的模型。使用functional API来完成这一任务
一般步骤：
1. 定义一个callable的layer实例，且该实例返回tensor
2. 输入、输出张量来定义一个tf.keras.Model实例
3. 训练(compile,fit函数)
```
inputs = keras.Input(shape=(32,)) #定义张量
x = keras.layers.Dense(64,activation='relu')(inputs)
y = keras.layers.Dense(64,activation='relu')(x)
p = keras.layers.Dense(10,activation='softmax')(x)
model = keras.Model(inputs=inputs,outputs=p)
model.compile(optimizer=tf.train.RMSPropOptimizer(0.001),
             loss='categorical_crossentropy',
             metrics=['accuracy'])
model.fit(data,labels,batch_size=32,epochs=5)
```
也可以通过subclassing（tc.Model）实现fully-customizable,在__init__方法中定义layers，并将它们设置为类的属性（要在call方法中自定义forward pass，还要重写compute_output_shape）
```
class MyModel(keras.Model):

  def __init__(self, num_classes=10):
    super(MyModel, self).__init__(name='my_model')
    self.num_classes = num_classes
    # Define your layers here.
    self.dense_1 = keras.layers.Dense(32, activation='relu')
    self.dense_2 = keras.layers.Dense(num_classes, activation='sigmoid')

  def call(self, inputs):
    # Define your forward pass here,
    # using layers you previously defined (in `__init__`).
    x = self.dense_1(inputs)
    return self.dense_2(x)

  def compute_output_shape(self, input_shape):
    # You need to override this function if you want to use the subclassed model
    # as part of a functional-style model.
    # Otherwise, this method is optional.
    shape = tf.TensorShape(input_shape).as_list()
    shape[-1] = self.num_classes
    return tf.TensorShape(shape)


# Instantiates the subclassed model.
model = MyModel(num_classes=10)

# The compile step specifies the training configuration.
model.compile(optimizer=tf.train.RMSPropOptimizer(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Trains for 5 epochs.
model.fit(data, labels, batch_size=32, epochs=5)
```
自定义layers
需要实现以下方法：
1. build 创建layer的权重，使用add_weight方法田间weight
2. call 定义forward pass
3. compute_output_shape指定如何计算给定layer的输出shape
4. 可以通过实现get_config和from_Config实现序列化
```
class MyLayer(keras.layers.Layer):

  def __init__(self, output_dim, **kwargs):
    self.output_dim = output_dim
    super(MyLayer, self).__init__(**kwargs)

  def build(self, input_shape):
    shape = tf.TensorShape((input_shape[1], self.output_dim))
    # Create a trainable weight variable for this layer.
    self.kernel = self.add_weight(name='kernel',
                                  shape=shape,
                                  initializer='uniform',
                                  trainable=True)
    # Be sure to call this at the end
    super(MyLayer, self).build(input_shape)

  def call(self, inputs):
    return tf.matmul(inputs, self.kernel)

  def compute_output_shape(self, input_shape):
    shape = tf.TensorShape(input_shape).as_list()
    shape[-1] = self.output_dim
    return tf.TensorShape(shape)

  def get_config(self):
    base_config = super(MyLayer, self).get_config()
    base_config['output_dim'] = self.output_dim
    return base_config

  @classmethod
  def from_config(cls, config):
    return cls(**config)


# Create a model using the custom layer
model = keras.Sequential([MyLayer(10),
                          keras.layers.Activation('softmax')])

# The compile step specifies the training configuration
model.compile(optimizer=tf.train.RMSPropOptimizer(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Trains for 5 epochs.
model.fit(data, targets, batch_size=32, epochs=5)
```

### 权重
默认使用checkpoint文件格式保存，keras HDF5格式是keras multi-backend的默认实现)
save_weight(path,save_format='h5') 
相同架构才能恢复权重
load_weight(path)
模型的序列化（不包含weight）：JSON YAML
model.to_json()
keras.models.model_from_json()

### estimators API
用于在分布式环境下训练模型
```
model = keras.Sequential([layers.Dense(10,activation='softmax'),
                          layers.Dense(10,activation='softmax')])

model.compile(optimizer=tf.train.RMSPropOptimizer(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

estimator = keras.estimator.model_to_estimator(model)
```
keras通过tf.contrib.distributionStrategy实现多GPU支持，只实现了tf.contrib.distribute.MirroredStrategy
如果需要使用distributionStrategy，要通过tf.keras.estimator.model_to_estimator将tf.keras.Model转换成tf.estimatoe.Estimatoe