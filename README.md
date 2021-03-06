# Spectral_Normalization-Tensorflow
 Simple Tensorflow Implementation of [Spectral Normalization for Generative Adversarial Networks](https://openreview.net/forum?id=B1QRgziT-&noteId=BkxnM1TrM) (ICLR 2018)
 
 ## Usage
 ```bash
 > python main.py --dataset mnist --sn True
 ```
 
 ## Summary
 ![sn](./assests/sn.png)
 
 ## Simple Code
 ```python
def spectral_norm(w, iteration=1):
    w_shape = w.shape.as_list()
    w = tf.reshape(w, [-1, w_shape[-1]])

    u_hat, v_hat = None, None

    u = tf.get_variable("u", [1, w_shape[-1]], initializer=tf.truncated_normal_initializer(), trainable=False)

    for i in range(iteration):
        v_ = tf.matmul(u, tf.transpose(w))
        v_hat = l2_norm(v_)

        u_ = tf.matmul(v_hat, w)
        u_hat = l2_norm(u_)
        u = u_hat

    sigma = tf.matmul(tf.matmul(v_hat, w), tf.transpose(u_hat))
    w_norm = w / sigma

    w_norm = tf.reshape(w_norm, w_shape)

    return w_norm
 ```
 
 ## How to use
 ```python
    w = tf.get_variable("kernel", shape=[kernel, kernel, x.get_shape()[-1], channels])
    b = tf.get_variable("bias", [channels], initializer=tf.constant_initializer(0.0))

    x = tf.nn.conv2d(input=x, filter=spectral_norm(w), strides=[1, stride, stride, 1]) + b
 ```
 
 ## Related works
 * [Group Normalization-Tensorflow](https://github.com/taki0112/Group_Normalization-Tensorflow)
 
 ## Author
 Junho Kim
