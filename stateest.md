---
title: State Estimation
layout: template
filename: stateest
--- 
### [Kalman and Bayesian Filters in Python](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python)
Reading to acquire a basic understanding of kalman filters.
#### Install and Preface
Installed anancoda and read through the Preface chapter.  This is my first experience with Anaconda and Jupyter Notebook.  Jupyter Notebook is a web application the author uses to share his text, with live, executable (in the browwser) code.  Anaconda comes with a package and environment manager (Conda) along with Python/R libraries for machine learning (scikit-learn), scientific computing (sciPy), plotting (Matplotlib) and more.  Notes on setup:
*   Install [Anaconda Individual Edition](https://www.anaconda.com/products/individual)
*   clone repo (`git clone --depth=1 https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python.git`)
*   Install filterPy library (`pip install filterpy`)
*   Launch jupyter notebook (jupyter notebook)
*   To deactivate conda environment: `coda deactivate/activate`
#### Chapter 1
Notes from chapter:
*   In Jupyter Notebook you can open the source code by typing the function name followed by two question marks (e.g. plot_errorbars??); if followed by one question mark, it will open the funciton documentation.
*   Ctrl+Enter will Run the current cell.
*   Keypoint of book: BLEND PREDICTION AND MEASUREMENT
*   Another point of emphasis:  don't throw out data, even extremely noisy data; usually can still provide benefit
*   Used a weight scale example to show benefit of g-h (a.k.a. alpha-beta) filter, fusion of measurement with prediction, and using both to update the rate of change gain (used by prediction)
*   "Any estimation problem consists of forming an estimate of a hidden state via observable measurements."