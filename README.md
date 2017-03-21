# Speaker Change Detection using Bi-LSTM
Code for [**Speaker Change Detection in Broadcast TV using Bidirectional Long Short-Term Memory Networks**](https://ocsync.limsi.fr/index.php/s/ZAsqipfZgoZZ8E0)
## Installation

**Foreword:** The code is based on [`pyannote`](https://github.com/pyannote). You can also find a similar `Readme` file for [`TristouNet `](https://github.com/hbredin/TristouNet).

```bash
$ conda create --name change-detection python=2.7 anaconda
$ source activate change-detection
$ conda install -c yaafe yaafe=0.65
$ pip install "theano==0.8.2"
$ pip install "keras==1.1.0"
$ pip install "pyannote.metrics==0.14"
$ pip install "pyannote.db.etape==0.2.3"
$ pip install "pyannote.database==0.11.1"
$ pip install "pyannote.generators==0.10"
```
What did I just install?

- [`keras`](keras.io) (and its [`theano`](http://deeplearning.net/software/theano/) backend) is used for all things deep.
- [`yaafe`](https://github.com/Yaafe/Yaafe) is used for MFCC feature extraction in [`pyannote.audio`](http://pyannote.github.io).
- [`pyannote.audio`](http://pyannote.github.io) is the core for this project. (Model architecture, optimizer, sequence generator)
- [`pyannote.db.etape`](http://pyannote.github.io) is the ETAPE plugin for [`pyannote.database`](http://pyannote.github.io), a common API for multimedia databases and experimental protocols (*e.g.* `train`/`dev`/`test` sets definition).
- [`pyannote.metrics`](http://pyannote.github.io) provides evaluation metrics.

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

## Training and prediction
The script is in [`yinruiqing/pyannote-audio`](https://github.com/yinruiqing/pyannote-audio/blob/develop/scripts/chang_detection_with_lstm.py). It will be soon merged into [`pyannote/pyannote-audio`](https://github.com/pyannote/pyannote-audio).

