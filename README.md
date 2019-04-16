# VisualizingWeights

Full Notebook: 

In [Lecture 10](https://forums.fast.ai/t/lesson-10-discussion-wiki-2019/42781/306) we looked at a few approaches to using hooks and plotting information about means and standard deviations of our network's activations.

This seems like it might be useful as a debugging strategy or sanity check on real-world models, so I wanted to try to instrument my own network. For simplicity's sake I chose to ResNet-18 against the MNIST dataset. 

The structure of ResNet-18 looks like: 

![image|443x400](https://i.imgur.com/1g29NJ9.png) 

I've chosen to instrument `conv1`, `conv2_x`, `conv3_x`, `conv4_x`, and `conv5_x`.

I ran `.fit()` for `3` epochs using a learning rate of `1e-2`. I used a validation size of 50% because the graphs start to get too wide if there are too many items in the training set.

### Pretrained ResNet-18 Activations

Trains to an error rate of `0.033000`

![image|690x452](https://i.imgur.com/gOgkdMX.png) 

### Untrained ResNet-18 Activations

Trains to an error rate of `0.034486`
![image|690x449](https://i.imgur.com/U9Jr8wQ.png) 

### Some thoughts

 - Both train to comparable error rates, despite having what appear to be wildly different activations
 - All of the layers of the untrained model change considerably from where they started
 - The pretrained model's earlier layers don't appear to change much. The first convolutional layer appears to be almost static. Part of me wonders what would happen if we tried to change this with something like "reverse-discriminitive learning rates". What if we used a higher learning rate for earlier layers than later layers. Presumably when using transfer learning this seems like it wouldn't work, after all we're counting on those earlier pre-trained layers to be in a good state. That said, I always read about vanishing gradients and part of me wonders what would happen if we just made the learning rate larger to force the gradients backwards through the network?



### Let's Break the Pretrained ResNet-18 Model

Out of curiosity, what happens if we use learning rates that are too large.

Trained with a learning rate of `1` to an error rate of `0.808114`

![image|690x457](https://i.imgur.com/G5AAWFO.png) 

### Let's Break the Untrained ResNet-18 Model

Out of curiosity, what happens if we use learning rates that are too large.

Trained with a learning rate of `1` to an error rate of `0.898943`

![image|690x457](https://i.imgur.com/O69SBKE.png) 





Surprisingly the pretrained model's first layer activations are left almost unchanged, even with larger learning rates. 

The untrained model's weights descend into some kind of pattern. In general it looks like most of the activations are collapsing to values closer to zero.
