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
*   Launch jupyter notebook (`jupyter notebook`)
*   To deactivate conda environment: `coda deactivate/activate`

#### Chapter 1
Notes from chapter:
*   In Jupyter Notebook you can open the source code by typing the function name followed by two question marks (e.g. plot_errorbars??); if followed by one question mark, it will open the funciton documentation.
*   Ctrl+Enter will Run the current cell.
*   Keypoints
	*   These filters blend predictions / models and measurements
	*   An estimation problem consists of forming an estimate of a hidden state via observable measurements
	*   Don't throw out data, even extremely noisy data; usually can still provide benefit
*   Used a weight scale example to show benefit of g-h (a.k.a. alpha-beta) filter, fusion of measurement with prediction, and using both to update the rate of change gain (used by prediction)
	*   prediction - previous estimate plus the estimated rate of change per unit time
	*   residual - difference between measurement and predicted value
    *   'g' gain - state gain; scales the amount of residual included in the estimate (i.e. est = predition + g * residual)
    *   'h' gain - state derivative gain; the amount of residual per unit time included in the derivative estimate
    *   gh are like proportional-integral (PI) gains
*   This makes me wonder what is being used for the model prediction for our navigation filter, in not a dynamics model?

#### Chapter 2
Discrete Bayes Filter.  Walks through how to predict a dogs position in a hallway, using a sonar sensor that returns 1 if in front of a door and 0 otherwise.  This would have been helpful to go through before implementing BATAN!!!
*   update_belief computes the likelihood score by applying a scale/weight to the postive (match) and negative readings.  The weight is a function of the accuracy of the the measurement.  In most cases, the measurement accuracy is expressed as a gaussian function (mean & standard deviation).  
*   BATAN uses a trapezoidal cost function.  Should turn measurement diff cost function into a probability using cell mean and standard deviation, or make some assumption about measurement accuracy using gradient
*   The likelihood score is converted to a probability density function by normalizing cell values using the sum of all cells (i.e. sum of all elements equals one after normalization).
*   this chapter has some great animations!
*   we are convolving the current probabilistic position estimate with a probabilistic estimate of how much we think the position moved.
	*   the example accounts for movement error by convolving with a 1x3 kernel ([0.1 0.8 0.1]); which means the filter believes there's a 10% probability the movement overshoot, 10% it undershoot and 80% its exact.
	*   this is where position uncertainty is applied.  In the precenses of drift, the uncertainty should grow with time... unless somehow we can estimate the position drift (i.e. velocity error).
 
*   Terminology
 	*   prior
 	*   posterior