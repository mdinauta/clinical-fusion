This is the repository for a course project for the course [CS 598 Deep Learning for Healthcare at UIUC](https://ws.engr.illinois.edu/sitemanager/getfile.asp?id=2191). For this project, we reproduced the results of the paper [Combining structured and unstructured data for predictive models: a deep learning approach by Zhang et al](https://www.researchgate.net/publication/346490918_Combining_structured_and_unstructured_data_for_predictive_models_a_deep_learning_approach). This repository is a fork of the [author’s original repository](https://github.com/onlyzdd/clinical-fusion), to which we’ve made a few changes in the interest of reproducing the results and running additional experiments.

Below are instructions on how to run the code. Partially taken from the author's original repository:

# Requirements 

## Dataset
MIMIC-III database analyzed in the study is available on [PhysioNet](https://mimic.physionet.org/about/mimic) repository. Here are some steps to prepare for the dataset:

- To request access to MIMIC-III, please follow https://mimic.physionet.org/gettingstarted/access/. Make sure to place `.csv` files under `data/mimic`.
- With access to MIMIC-III, to build the MIMIC-III dataset locally using Postgres, follow the instructions at https://github.com/MIT-LCP/mimic-code/tree/master/buildmimic/postgres.
- Run SQL queries to generate necessary views, please follow https://github.com/onlyzdd/clinical-fusion/tree/master/query.

## Software

- Python 3.6.10
- Gensim 3.8.0
- NLTK: 3.4.5
- Numpy: 1.14.2
- Pandas: 0.25.3
- Scikit-learn: 0.20.1
- Tqdm: 4.42.1
- PyTorch: 1.4.0

# Preprocessing

```sh
$ python 00_define_cohort.py # define patient cohort and collect labels
$ python 01_get_signals.py # extract temporal signals (vital signs and laboratory tests)
$ python 02_extract_notes.py --firstday # extract first day clinical notes
$ python 03_merge_ids.py # merge admission IDs
$ python 04_statistics.py # run statistics
$ python 05_preprocess.py # run preprocessing
$ python 06_doc2vec.py --phase train # train doc2vec model
$ python 06_doc2vec.py --phase infer # infer doc2vec vectors
```

# Run

* Changes were made to the file cnn.py in order to replicate the Fusion-CNN results. The Fusion-CNN model can be trained by running

```sh
$ python main.py --model cnn --inputs 7
```

* To run the additional *random forest* baseline experiments,

```sh
python baselines5.py --model rf --inputs 7
````

* To run the additional *gradient boosting* baseline experiments,

```sh
python baselines5.py --model gbm --inputs 7
```

# Replication Study Results Summary

| Model        | Purpose           | Mean AUCROC (replication) |  Mean AUCROC (original paper)
| ------------- |:-------------:| -----:|------:|
| Baseline (LR)      | Replication | 0.864 | 0.862 |
| Baseline (RF)      | Replication | 0.724 | 0.735 |
| Baseline (RF) - New Params | Experiment | 0.862 | N/A |
| Baseline (GB) - New Model | Experiment | 0.833 | N/A |
| Fusion-CNN | Replication | 0.868 | 0.871 |

The above AUCRUC values were obtained by fitting the models with all of the training data (Model Inputs = U + T + S, if comparing to the original report).