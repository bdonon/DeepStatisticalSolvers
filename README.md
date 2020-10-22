# Deep Statistical Solver

This repository is the official implementation of [Deep Statistical Solvers](https://hal.inria.fr/hal-02974541) (accepted at NeurIPS 2020).

The core idea is to address optimization problems with deep learning tools. 

## Virtual environment

It is recommended to use a [virtual environment](https://python-guide-pt-br.readthedocs.io/fr/latest/dev/virtualenvs.html), as it helps manage in a clean way the code dependencies. 
```virtualenv
pip install virtualenv
```
Go to the folder /DeepStatisticalSolver, and create a new virtualenv, with python 3
```virtualenv
virtualenv ENV -p python3
```
The command above should have created a folder `ENV/`. Now you need to activate your virtual environment.
```virtualenv
source ENV/bin/activate
```
You should now see (ENV) before your username.
If you want to deactivate your virtualenv (for instance to work on another project), use the following command line
```virtualenv
deactivate
```
But for now, keep you virtual environment activated!

## Requirements

If you have a GPU and want to use it, install the following requirements:

```setup
pip install -r requirements-gpu.txt
```

Otherwise, if you do not have a GPU, or do not want to use it, install the following requirements:

```setup
pip install -r requirements.txt
```

## Datasets

This work relies on datasets of optimization problems instances. We built these from scratch, and more informations are given in the notebooks. 
For each dataset, download all the files that are available at the links below, and place them in the corresponding folder:

- [Dataset Linear systems](http://doi.org/10.5281/zenodo.4024811) --> place them in `datasets/linear_systems`
- [Dataset AC power flow 14 nodes](http://doi.org/10.5281/zenodo.4024866) --> place them in `datasets/acpf_14`
- [Dataset AC power flow 118 nodes](http://doi.org/10.5281/zenodo.4024875) --> place them in `datasets/acpf_118`

In the linear systems dataset, you also have to unzip the file `varying_size.zip`.

Disclaimer : In the submitted version of the paper Deep Statistical Solver, there are links to past versions of those datasets (with slight differences in file names, as well as in the problem.py files). Please use the most recent versions of the datasets by using the links above.

## Training

To train the models used in the paper, here are the exact commands that were used:

- Linear systems experiments

```
python main.py --data_dir=datasets/linear_systems --max_iter=1000000 --minibatch_size=100 --track_validation=1000 --learning_rate=1e-3 --discount=0.9 --latent_dimension=10 --hidden_layers=2 --correction_updates=30 --alpha=0.001
```

- AC power flow 14 nodes experiments

```
python main.py --data_dir=datasets/acpf_14 --learning_rate=3e-3 --minibatch_size=1000 --alpha=1e-2 --hidden_layers=2 --latent_dimension=10 --correction_updates=10 --track_validation=1000
```

- AC power flow 118 nodes experiments

```
python main.py --data_dir=datasets/acpf_118 --learning_rate=3e-3 --minibatch_size=500 --alpha=3e-4 --hidden_layers=2 --latent_dimension=10 --correction_updates=30 --track_validation=1000 --discount=0.9
```

## Evaluation

To evaluate the models, we recommend to take a look at the jupyter notebooks. They provide some code to reload trained model, perform inference on the test set, and also some visualizations!
See details below for more about the notebooks.

## Pre-trained Models

You can download pretrained models [here](http://doi.org/10.5281/zenodo.4108352).
Unzip the file and place all of the 9 model directories inside the `DeepStatisticalSolver/results` directory.

## Results

Here are the metrics for the three different experiments, and for each of the three corresponding models.
The main metric is indeed the correlation with the best existing optimization method.
Check the notebooks to obtain these results.

### Linear systems experiments

The best existing method is the LU decomposition.

| Model name         | Model 0 | Model 1 | Model 2 |
| ------------------ |---------|---------|---------|
| Correlation w/ LU  | 99.99%  | 99.99%  | 99.99%  |
| Loss 10th perc.    | 3.0 E-4 | 3.8 E-4 | 2.7 E-4 |
| Loss median        | 6.0 E-4 | 1.3 E-3 | 6.9 E-4 |
| Loss 90th perc.    | 1.5 E-3 | 4.0 E-3 | 2.3 E-3 |

### AC power flow - 14 nodes

The best existing method is the Newton-Raphson method (NR). The correlation is in term of active power flows through 
power lines.

| Model name         | Model 0 | Model 1 | Model 2 |
| ------------------ |---------|---------|---------|
| Correlation w/ NR  | 99.99%  | 99.99%  | 99.99%  |
| Loss 10th perc.    | 2.5 E-5 | 3.4 E-5 | 4.1 E-5 |
| Loss median        | 4.0 E-5 | 6.3 E-5 | 1.0 E-4 |
| Loss 90th perc.    | 1.0 E-4 | 1.5 E-4 | 4.4 E-4 |

### AC power flow - 118 nodes

The best existing method is the Newton-Raphson method (NR). The correlation is in term of active power flows through 
power lines.

| Model name         | Model 0 | Model 1 | Model 2 |
| ------------------ |---------|---------|---------|
| Correlation w/ NR  | 99.99%  | 99.99%  | 99.99%  |
| Loss 10th perc.    | 5.3 E-7 | 1.3 E-6 | 1.7 E-7 |
| Loss median        | 1.3 E-6 | 1.7 E-6 | 2.8 E-7 |
| Loss 90th perc.    | 3.4 E-6 | 2.5 E-6 | 4.8 E-7 |


## Notebooks

In order to help people understand and visualize our approach, we built a series of notebooks that provides visualizations and additional explanations about the problems and modelling choices. Feel free to take a look.

To do so, you need to open a jupyter notebook session:
```notebooks
ENV/bin/jupyter notebook
```
This should have opened a web page on your browser which allows you to naviguate through the directory. Click on the `notebook/` folder, and then on the notebook you are interested in!