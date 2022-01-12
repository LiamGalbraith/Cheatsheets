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
