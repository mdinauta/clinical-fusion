This is the repository for a course project for the course [CS 598 Deep Learning for Healthcare at UIUC](https://ws.engr.illinois.edu/sitemanager/getfile.asp?id=2191). For this project, we reproduced the results of the paper [Combining structured and unstructured data for predictive models: a deep learning approach by Zhang et al](https://www.researchgate.net/publication/346490918_Combining_structured_and_unstructured_data_for_predictive_models_a_deep_learning_approach). This repository is a fork of the [author’s original repository](https://github.com/onlyzdd/clinical-fusion), to which we’ve made a few changes in the interest of reproducing the results and running additional experiments.

To run this code, please first see the author's original repository which contains documentation on building the dataset. Below, we will highlight areas we've changed the code and therefore how to replicate findings in our project.

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
