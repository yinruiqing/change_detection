# Speaker Change Detection using Bi-LSTM
Code for [**Speaker Change Detection in Broadcast TV using Bidirectional Long Short-Term Memory Networks**](https://ocsync.limsi.fr/index.php/s/ZAsqipfZgoZZ8E0)

## Citation

```
@techreport{Yin2017,
  author = {Ruiqing Yin and Herv\'e Bredin and Claude Barras},
  title = {{Speaker Change Detection in Broadcast TV using Bidirectional Long Short-Term Memory Networks}},
  howpublished = {{Submitted to Interspeech 2017}},
  url = {https://github.com/yinruiqing/change_detection},
}
```

## Installation

**Foreword:** The code is based on [`pyannote`](https://github.com/pyannote). You can also find a similar `Readme` file for [`TristouNet `](https://github.com/hbredin/TristouNet).

```bash
$ conda create --name change-detection python=2.7 anaconda
$ source activate change-detection
$ conda install gcc
$ conda install -c yaafe yaafe=0.65
$ pip install "pyannote.audio==0.2.1"
$ pip install pyannote.db.etape
```
What did I just install?

- [`keras`](keras.io) (and its [`theano`](http://deeplearning.net/software/theano/) backend) is used for all things deep. If you want to use `GPU`, please downgrade `Keras` version to `1.2.0`
- [`yaafe`](https://github.com/Yaafe/Yaafe) is used for MFCC feature extraction in [`pyannote.audio`](http://pyannote.github.io).
- [`pyannote.audio`](http://pyannote.github.io) is the core for this project. (Model architecture, optimizer, sequence generator)
- [`pyannote.db.etape`](http://pyannote.github.io) is the ETAPE plugin for [`pyannote.database`](http://pyannote.github.io), a common API for multimedia databases and experimental protocols (*e.g.* `train`/`dev`/`test` sets definition).

Then, edit `~/.keras/keras.json` to configure `keras` with `theano` backend.

```json
$ cat ~/.keras/keras.json
{
    "image_dim_ordering": "th",
    "epsilon": 1e-07,
    "floatx": "float32",
    "backend": "theano"
}
```

#### About the ETAPE database

To reproduce the experiment, you obviously need to have access to the ETAPE corpus.  
It can be obtained from [ELRA catalogue](http://islrn.org/resources/425-777-374-455-4/).

However, if you own another corpus with *"who speaks when"* annotations, you can fork [`pyannote.db.etape`](http://github.com/pyannote/pyannote-db-etape) and adapt the code to your own database. 

## Training and evaluation
You can use:

```bash
$ pyannote-change-detection -h
```
to find the usage information.

```
change detection

Usage:
    pyannote-change-detection train [--database=<db.yml> --subset=<subset>] <experiment_dir> <database.task.protocol>
    pyannote-change-detection evaluate [--database=<db.yml> --subset=<subset> --epoch=<epoch> --min_duration=<min_duration>] <train_dir> <database.task.protocol>
    pyannote-change-detection apply  [--database=<db.yml> --subset=<subset> --threshold=<threshold> --epoch=<epoch>  --min_duration=<min_duration>] <train_dir> <database.task.protocol>
    pyannote-change-detection -h | --help
    pyannote-change-detection --version
...
```
Example of config file can be found in `change_detection/config/`. Before doing the training and evaluation, you can clone this project to local directory. 

```bash
git clone https://github.com/yinruiqing/change_detection.git
```

### Training 

```bash
$ pyannote-change-detection train --database change_detection/config/db.yml --subset train change_detection/config Etape.SpeakerDiarization.TV
```
This is the expected output:

```
Epoch 1/100
62464/62464 [==============================] - 171s - loss: 0.1543 - acc: 0.9669   
Epoch 2/100
62464/62464 [==============================] - 117s - loss: 0.1375 - acc: 0.9692     
Epoch 3/100
62464/62464 [==============================] - 115s - loss: 0.1376 - acc: 0.9691     
...
Epoch 50/100
62464/62464 [==============================] - 112s - loss: 0.0903 - acc: 0.9724  
...
Epoch 98/100
62464/62464 [==============================] - 115s - loss: 0.0837 - acc: 0.9732     
Epoch 99/100
62464/62464 [==============================] - 112s - loss: 0.0839 - acc: 0.9732     
Epoch 100/100
62464/62464 [==============================] - 112s - loss: 0.0840 - acc: 0.9731

```
### Evaluation
```bash
$ pyannote-change-detection evaluate --database change_detection/config/db.yml --subset development change_detection/config/train/Etape.SpeakerDiarization.TV Etape.SpeakerDiarization.TV 
```
This is the expected output:

```
0         95.720% 36.603%
0.0526316 95.526% 49.018%
0.105263  95.213% 57.660%
...
0.526316  92.396% 83.756%
...
0.894737  88.155% 91.139%
0.947368  87.468% 92.046%
1         86.929% 92.672%

```
The first column is threshold, the second column is purity and the third column is coverage. 


