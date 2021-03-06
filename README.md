MuseGAN
=======

[MuseGAN](https://salu133445.github.io/musegan/) is a project on music generation.
In essence, we aim to generate polyphonic music of multiple tracks (instruments) with harmonic and rhythmic structure, multi-track interdependency and temporal structure.
To our knowledge, our work represents the first approach that deal with these issues altogether.

The models are trained with [Lakh Pianoroll Dataset](https://salu133445.github.io/musegan/dataset) (LPD), a new multi-track piano-roll dataset, in an unsupervised approach.
The proposed models are able to generate music either from scratch, or by accompanying a track given by user.
Specifically, we use the model to generate pop song phrases consisting of bass, drums, guitar, piano and strings tracks.
Sample results are available [here](https://salu133445.github.io/musegan/results).

Paper
-----

Hao-Wen Dong\*, Wen-Yi Hsiao\*, Li-Chia Yang and Yi-Hsuan Yang, "**MuseGAN: Multi-track Sequential Generative Adversarial Networks for Symbolic Music Generation and Accompaniment**," in *AAAI Conference on Artificial Intelligence (AAAI)*, 2018.
[[arxiv](http://arxiv.org/abs/1709.06298)]
[[slides](https://github.com/salu133445/musegan/blob/master/docs/pdf/musegan-aaai2018-slides.pdf)]

\**These authors contributed equally to this work.*

Usage
-----

```python
import tensorflow as tf
from musegan.core import MuseGAN
from musegan.components import NowbarHybrid
from config import *

# initialize a tensorflow session
with tf.Session() as sess:

    ### prerequisites ###
    # step 1. initialize the training configuration
    t_config = TrainingConfig
    
    # step 2. select the desired model
    model = NowbarHybrid(NowBarHybridConfig)
    
    # step 3. initialize the input data object
    input_data = InputDataNowBarHybrid(model)
    
    # step 4. load training data
    path_train = 'train.npy'
    input_data.add_data(path_train, key='train')
    
    # step 5. initialize the museGAN object
    musegan = MuseGAN(sess, t_config, model)

    ### training ###
    musegan.train(input_data)

    ### load and generate samples ###
    # load pretrained model
    musegan.load(musegan.dir_ckpt)
    
    # add testing data
    path_test = 'train.npy'
    input_data.add_data(path_test, key='test')
    
    # generate samples
    musegan.gen_test(input_data, is_eval=True)
```

Training Data
-------------
- [tra_phr.npy](https://drive.google.com/file/d/1-bQCO6ZxpIgdMM7zXhNJViovHjtBKXde/view) (7.54 G):
A numpy array that contains the training data for phrases. Its shape is (50266, 384, 84, 5). 
 
- [tra_bar.npy](https://drive.google.com/file/d/1Xxj6WU82fcgY9UtBpXJGOspoUkMu58xC/view?usp=sharing) (4.79 G):
A numpy array that contains the training data for bars. Its shape is (127734, 96, 84, 5). 
