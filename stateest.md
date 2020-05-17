---
title: State Estimation
layout: template
filename: stateest
--- 
### [Kalman and Bayesian Filters in Python](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python)
Reading to acquire a basic understanding of kalman filters.
#### Install and Preface
Installed anancoda and read through the Preface chapter.  This is my first experience with Anaconda and Jupyter Notebook.  Jupyter Notebook is a web application the author uses to share his text, with live, executable (in the browser) code.  Anaconda comes with a package and environment manager (Conda) along with Python/R libraries for machine learning (scikit-learn), scientific computing (sciPy), plotting (Matplotlib) and more.  Notes on setup:
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
*   Used a body weight-scale example to show benefit of g-h (a.k.a. alpha-beta) filter, fusion of measurement with prediction, and using both to update the rate of change gain (used by prediction)
   *   prediction - previous estimate plus the estimated rate of change per unit time
   *   residual - difference between measurement and predicted value
   *   'g' gain - state gain; scales the amount of residual included in the estimate (i.e. est = predition + g * residual)
   *   'h' gain - state derivative gain; the amount of residual per unit time included in the derivative estimate
   *   gh are similar to a controller's proportional-integral (PI) gains
*   This makes me wonder what is being used for the model prediction for our navigation filter, if not a dynamics model?

#### Chapter 2
Discrete Bayes Filter.  This chapter demonstrates how to use a Bayes filter to predict a dogs position in a hallway, with a sonar sensor that returns 1 if in front of a door and 0 otherwise.  The output of the sensor has a categorical distribution because it can only have one of N outcomes.  Like the g-h filter, the Bayes filter process has two steps: predict and update.
*   Process
   *   Predict
       *   Formulate prediction by moving the prior.
       *   Add uncertainty to this movement by convolving with the movement error model; this smears the prior based on uncertainty in movement between steps.  In this process step, certainty degrades because we lose confidence of prior knowledge with each prediction (i.e. old measurement data decays because it is stale).
   *   Update
       *   Now update by calculating the likelihood score using the measurement and measurement error model.  Certainty of true positive(s) should grow because new information was added.
       *   Multiply likelihood and prior
       *   Convert product to probability (normalize using cumulative sum); result is the posterior (probability, sum of all is one(1)).
   *   we are convolving the current probabilistic position estimate with a probabilistic estimate of how much we think the position moved.
*	Notes on the example   
    *   update_belief computes the likelihood score by applying a scale/weight to the postive (matches door) ~~and negative~~ readings only.  The weight is a function of the accuracy of the the measurement.  In the example, the scale is 0.75 (measurement is correct 3 out of 4 times).  In many cases, the measurement error can be expressed as a gaussian function (standard deviation).    
    *   the example accounts for movement error by convolving with a 1x3 kernel ([0.1 0.8 0.1]); which means the filter believes there's a 10% probability the movement overshoot, 10% it undershoot and 80% its exact.  This is where position uncertainty is applied.  In the precenses of drift, should the uncertainty grow with time?  How would this filter/implementation behave in the presence of drift?  Could the drift be estimated somehow?
*   Terminology
    *   prior (probability distribution) - probabiilty prior to incorporating measurement / information
    *   posterior (probability distribution) - probability after incorporating measurement / information
    *   likelihood - how likely each array / matrix element is the measurement (i.e. door)
*   Frequentist vs Bayesian statistics.
*   Notes for BATAN
    *   don't intialize all elements to one(1), give all cells equal (1/(row x col)) starting probability
    *   change current ordering; move prior and then blur using gaussian, then perform update step (use new terrain measurement)
    *   update terrain measurement error model; use vehicle attitude and gradient in cost function.  Maybe turn convert to a gaussian function using cell mean and standard deviation from DTED?  Bottom-line, need to make the measurement more probabilistic.
    *   the true positives don't recover in current implementation because we're not performing the update then normalizing to create a probability; during this sequence the true positives should increase significantly wrt to false positives and after normalizing the matrix, the true positive probability should increase while all other fall.
    *   the graph behavior will essentially flip with the changes.  Instead of waiting for false positives to fall off, we'll wait for the true postive to grow.  This will help the match logic because it will make it easier to reject false positives.  With the previous implementation, time steps with little new information (i.e. pitch/roll angles were large so weight was 0 or large cell gradients would result in wide span of terrain being accepted as a 'match') would hold false positives above the score threshold.  With inversion, calculation should better at rejecting false positives because steps with little information will hold score/certainty down (this makes more sense!).
    *   also, since we're not normalizing the likeness scores to turn them into a probability, determining when a match has a occurred doesn't have to be concerned with checking for multiple peaks greater than our threshold (0.7) (i.e. 1.0 - 0.7 < threshold).
    *   with this new approach, additional parameter tuning will certainly be required: finding the lowest acceptable threshold to lower time to match but ensure no false positives, etc.
    *   should we use the drift / velocity error to calculate the match timeout?  if t * v > then size of one element and no match found reset?
    *   actually, if drift causes the true position to move to a new grid cell, the old true positive (now a false positive) should decay and the new true positive cell should increase ... maybe there's no longer a need to reset?
    *   does it make sense the kernel should be calculated using the estimated drift?  How would the filter behave if the kernel expands over time (due to drift) as the system is propogated?
*   Random thought: the decaying behavior of old information made me think about my engineering knowledge.  Each time step (e.g. year) my prior knowledge about an event (e.g. undergraduate prob & stas course) degrades and will continue to degrade until without new information.
