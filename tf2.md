# How Tensorflow works
## Graphs and Sessions
Tensorflow creates a graph of defined computations - this is like writing down an equation without performing any math e.g. `varaible z = variable x + varaible y`, without knowing the actualy value of x, y, or z. Different computations are linked together, so a certain computation may need to be completed before another can be run, so these are linked together in the graph.

Sessions are way to execute part of, or the entire, graph.

## Tensors
Tensors are vectors generalised to higher dimensions. Tensorflow represents tensors as n-dimensiona arrays of base datatypes.

Each tensor has a data type (float32, int32, string etc.) and a shape (the dimensions).

### Defining a tensor
#### Scalar
Scalars are simple tensors, with shape 1 (1 value). You can create a scalar variable with `tf.Variable`:
```
string_var = tf.Variable("This is a string", tf.string)
int_var = tf.Variable(27, tf.int16)
float_var = tf.Variable(2.7, tf.float64)
```

#### Rank
The rank of a tensor is the deepest level of a nested list. You can determine the rank using `tf.rank(tensor)`


Scalars are rank 0 tensors, this is because they are a single, defined value with no dimensionality - it has no axes.
```
scalar_value = tf.Variable(4, tf.int32)
tf.rank(scalar_value)
>> <tf.Tensor: shape=(), dtype=int32, numpy=0>
```

If you add an axis, this becomes a rank-1 tensor:
```
rank1_tensor = tf.Variable([4, 5, 6], tf.int32)
tf.rank(rank1_tensor)
>> <tf.Tensor: shape=(), dtype=int32, numpy=1>
```

By adding a second axis, this becomes a rank-2 tensor:
```
rank2_tensor = tf.Variable([[4, 5], [5, 6]], tf.int32)
tf.rank(rank2_tensor)
>> <tf.Tensor: shape=(), dtype=int32, numpy=2>
```

#### Shape
Shape is the number of elements that exist in each dimension. You can view this using `tensor.shape`

```
scalar_value = tf.Variable(4, tf.int32)
scalar_value.shape
>> TensorShape([

rank1_tensor = tf.Variable([4, 5, 6], tf.int32)
rank1_tensor.shape
>> TensorShape([3])

rank2_tensor = tf.Variable([[4, 5], [5, 6]], tf.int32)
rank2_tensor.shape
>> TensorShape([2, 2])


rank2_tensor = tf.Variable([[4, 5], [5, 6], [6, 7]], tf.int32)
rank2_tensor.shape
>> TensorShape([3, 2])


rank2_tensor = tf.Variable([[4, 5, 6], [5, 6, 7], [6, 7, 8]], tf.int32)
rank2_tensor.shape
>> TensorShape([3, 3])
```

#### Changing Shape
```
tensor1 = tf.ones([1, 2, 3])  # Creates a Tensor of ones, with shape [1, 2, 3]
>> tf.Tensor(
    [
        [
            [1. 1. 1.]
            [1. 1. 1.]
        ]
    ],
    shape=(1, 2, 3),
    dtype=float32
)

tensor2 = tf.reshape(tensor1, [2, 3, 1])
>> tf.Tensor(
    [
        [
            [1.]
            [1.]
            [1.]
        ]
        [
            [1.]
            [1.]
            [1.]
        ]
    ],
    shape=(2, 3, 1),
    dtype=float32
)

tensor3 = tf.reshape(tensor2, [3, -1])  # -1 infers the number, by figuring out how many elements are left. In this case, 2, as 6 elements and 3 * 2 = 6
>> tf.Tensor(
    [
        [1. 1.]
        [1. 1.]
        [1. 1.]
    ],
    shape=(3, 2),
    dtype=float32
)

tf.reshape(tensor2, [6])
>> tf.Tensor(
    [1. 1. 1. 1. 1. 1.],
    shape=(6),
    dtype=float32
)
```

#### Types
* Constant
* Variable
* Placeholder
* SpareTensor

With the exception of `Variable`, these are all immutable.

#### Evaluation
To evaluate a tensor, we must create a session:
```
with tf.Session() as sess:
    tensor.eval()
```
Now, we can use that tensor in future calculations.

# Keras

*Load built-in datasets*
```
mnist = tf.keras.datasets.mnist
(x_train, y_train), (x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0  # Convert the sample from int to float (0 to 1)
```

*Build a model*
```
model = tf.keras.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dropout(0.2),
    tf.keras.layers.Dense(10)
])
```

*Define a loss function*
**SparseCategoricalCrossentropy** takes a vector of logits and a True index and returns a scalar loss for each example.
The loss if equal to the negative log probability of the true class - the loss is zero if the model is sure the of the correct class.
```
loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
```

*Compile a model*
Before you start training, you need to configure and complie the model
```
model.compile(optimizer='adam',
              loss=loss_fn,
              metrics=['accuracy'])
```

*Train and Evaluate a model*
```
model.fit(x_train, y_train, epochs=5)
model.evaluate(x_test, y_test, verbose=2)
```

*Return a probability*
Wrap the model with a softmax layer
```
probability_model = tf.keras.Sequential([
    model,
    tf.keras.layers.Softmax()
])
```

### Classification Models
Create a final layer with multiple node, and the model returns a vector of logit (or log-odds) scores for each class.
  * The log offs of an event is the logit of the probability of the event.
  * Log-odds = ln(p/(1-p))

The softmax function converts these logits to probabilities for each class:
```
tf.nn.softmax(predictions).numpy()
```

To return a probability, you can wrap the trained model and attach the softamx to it:
```
model = tf.keras.Sequential([
    tf.keras.layers.Flatten(input_shape=(28, 28)),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dropout(0.2),
    tf.keras.layers.Dense(10)
])

loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)

model.compile(optimizer='adam',
              loss=loss_fn,
              metrics=['accuracy'])
              

model.fit(x_train, y_train, epochs=5)

probability_model = tf.keras.Sequentals([
    model,
    tf.keras.layers.Softmax()
])
```
