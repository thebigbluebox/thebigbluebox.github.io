---
title:  "Standford Machine Learning"
date:   2016-6-13 9:00:00
description: Starting with Coursera machine learning course

technologies:
 - name: "Machine Learning"
 - name: "Octave"
---

# Machine Learning
# Supervised Learning
* We gave the algorithm a data set of the correct answer, and kept it in an environment where all correct answers are given.
* It is also a similar to a regression problemwhere we predict a continous valued output (in reference to the example)
* Classification problem means if you have discrete valued output (category A B or C)
* Looking at the breast cancer example we started off with a simple binary classification benign or malignant on a 2 axis graph, but then we can simplify it down to a linear line showing the results according to the tumor size
    * by remapping this, we allow for the adding of another factor such as AGE to replace our former Binary classification
* How to deal with an infinity number of features?
* ### Regression problem -> We want to perdict a continous valued output
* ### Classification problem -> We want to classify an input as one or many different categories

# Unsupervised Learning
* We go from given labels from the dataset in supervised learning and now we are given a dataset and we must decide for ourselves if the data has some sort of structure
* Clustering is a problem that invloves figuring out different clusters from a given data set such as market segmentation, computing clusters, and your social network, it is analgous to the classification problem in supervised learning however you are not given the labels to the data
* Cocktail party problem, 2 people 2 microphone in different locations, so we have different pitch and tone for the different microphones
    * And we can feed it into an algorithm and find structure, which can output a separation of the voice that overlapped

## Tools
* We will be using Octave or Matlab to work on these algorithms
* People use Octave or Matlab as a quick prototyping tool

# Linear Regression
Math!!!
#### Regression means we will be predicting a real-valued output
* Our data set for the regression -> training set
* Notation
    * m = Number of training examples (47 rows of training examples means 47 traning examples)
    * x's = "input" variable / features
    * y's = "output" variable / "target" variable
    * (x,y) = one training example
    * (x^i,x^i) = the ith training example 
* Training Set
    * We feed training set -> Learning Algorithm -> Function H (Hypothesis which is a function that takes in the features and output the estimated output)
    * hypothesis is a function that maps X -> Y
    * How do we represent h? he(x) = thetaO + theta1x
        * We are going to predict that Y is a linear function of X 
        * So we can see that y is a straight line function
    * This is linear regression with one variable -> Univariate linear regression
## Cost function
Learning to fit the best straight line to our data
* Thetas on the hypothesis functions are parameter values
* Different choicers of the parameter we get different hypothesis functions
* How do we get the best fit line?
    * Choose theta 0 and theta 1 so that h(x) is close to y for our training examples (x,y)
    * We want to solve a minimization problem
    * minimize theta 0 and theta 1 (h(x) - y)^2 and minimize the square difference
    * Look at note 1
* Hypothesis is a function of x while the cost function is a function of the value for theta

# Gradient Descent
Have a function J(O1,O2) and want to minimize the theta and J(O1,O2)
1. Outline start with theta 0 and 1
2. keep changing theta 0 and theta 1 to reduce J(01,02) until we hopefully we get a minimize
Gradient dessent will not give you a exact answer all the time as we keep on changing it differently

Batch gradient desent uses all data value from the training set, while we can also selectively pick a smaller group of data to make computing easier

# Multivariate  Linear Regression
What if use multiple features for the hypothesis increasing our factors to train with
We still use X for our features and our predicted output as our Y
Now we let N be the number of features we have

## Hypothesis representation 
Since before we had our hypothesis represented by a standard Y=MX+B now we will have it represented like h(x) = theta0 + theta1*x1 + theta2*x2 ... etc
So instead we now will create our features and fit it into an  m+1 -dimensonal vector
So also we will write our training set data and fit it into an n+1 dimensonal vector

With the two vectors now we can just do an inner product to fi

## Feature Scaling
When our contour graph is actually very different scales, we should be scaling it so it could be closer and similar to each other so we can get to the local optimum quicker
And we can scale the ranges so that the graph looks better, such as reducing the features to between -1 < 0 < 1 range

## Mean Normalization
Replace xi with xi-mui to make features have approximately zero mean (Do not apply to x0 = 1)
    * With Mu(i) being the average of xi in the training set
    * And the division below is the (max - min) from the trainning set
x1 = (size - 1000) / 2000
x2 = (#bedrooms -2) / 5
This makes the range of x1 and x2 all between -0.5 < x1,x2 < 0.5

## Choosing learning rate Alpha
### Making sure gradient descent is working correctly

we want to ensure that the plot of minimizing of theta of J(theta) and as the iterations -> infinity it should be reducing at a reasonable rate
If the the plot of iteration to J(theta) is getting higher then reduce and use a smaller alpha (learning rate)
If the learning rate alpha is small eno ugh all J(theta) will always move downward

## Features and Polynomial Regression
Is scaling up your x values so it fits your data set better such as x^2 or sqrt(x)

## Normal Equations
Instead of iteratively solving for the optimal point using gradient descent we can solve it directly using normal euqation

## Using Octave
* && logical and
* % logical or
* PS('>>'); changes the octave prompt
* assign variable can be done using a = x;
* or strig assignment a = 'hi';
* disp(a) display the variable
* disp(sprintf('2 decimals: %0.2f', a)) this will print using a traditional C syntax
* format long will show the long variable, and format short resets the display

### vector
* A = [ 1 2; 34; 5 6] -> will create a matrix that is 3x2 matrix
* V = [1 2 3] will give a 1x3 vector
* V = [ 1; 2; 3] will give a 3x1 vector
* V = 1:0.1:2 this means start at 1, increment by 0.1, end at 2, or 2:1:5 start at 2 increment by 1 and end by 5
* ones(2,3) will generate a all one matrix of 2 by 3
* rand(1,3) will create a 1x3 vector of random numbers similarly rand(3,3) will generate a random matrix of 3x3
* randn will generate a gussian random number
* hist(w) will plot the histogram of the function w
* eye(4) identity matrix of 4x4 or .... 3x3 2x2 1x1
* help ___ similar to man but for exaplanation of a function in Octave

### Moving Data around
* size(A) returns a 1x2 matrix on the size ofthe matrix row x collu mns
* size (A,1) gives the length of the matrix of the first dimension
* length(A) will give the longest dimension of the matrix

Data
* pwd will give the current path of octave
* ls will list the directories and files
* cd will change directory
* load something.dat will load the data from the current path to Octave
* who will show the current variables that are available 
* whos gives a detail view of all variables that are currently being used
* clear will delete a variable 
* v = priceY(1:10) will store the data from priceY from rows 1 to 10
* save hello.mat will save the current file to the mat file in path
* save hello.mat v ... the v flag will save in a binary data
* save hello.txt v -ascii will save this file as a text encoded in ASCII
* A = [1 2; 3 4; 5 6] then we do A(3,2) will fetch the value from third row 2nd element
* A(2,:) will give everything from the second row... A(:,2) will give everything in the second column
* A[1 3],:) get everything from first and 3rd row and everything from their columns
* A(:,2) = [ 10;11;12] we can also do assignments
* A = [A,[100 ; 101; 102]] will append another column to the matrix
* A(:) puts all elements of A into a single vector
* A = [1 2; 3 4; 5 6] B = [7 8; 9 10; 11 12] C = [ A B ] this will concatenate the matrices
* C = [A ; B] will concanenate the matrices from top to bottom

### Computation
* A*C to compute the product of the matrices
* A .* B will do a element wide multiplication
* A .^ 2 will do the element wide square
* 1 ./ V will do element wide inverse
* abs absolute value
* multiply the negative of the vector by just adding -
* v + ones(length(v) , 1) which gives us a ones matrix that is just the length of the rows in v and with column of 1) giving us length(v) x 1
* v + 1 also adds it all to the vector
* A' gives A transpose
* max(a) will give the max value of the matrix 
* we can do a < 3 will give us the element wise comparison returning a 1x...length of matrix giving us a true or false
* find(a < 3) will give us all the numbers greater than 3
* [r,c] = find(A >= 7) this will create a row and colum r c vectors of the values greater than 7
* sum sums the elements
* prod multiples all the elements
* floor(a) round down
* ceil(a) round up
* max(A, [], 1) takes the maximum of the rows
* max(A, [], 2) takes the maximum of the columns
* sum(A,1) does the column wise sum 
* sum(A,2) does the row wise sum

### Plotting
* we set the scale using a tuple [beginning: increment: end]
* we then assign the function to a variable and use the function multiply it to the tuple
* we then use the function plot(scale,func)
* xlabel
* ylabel
* title
* print -dpng 'myPlot.png' will save the plot to a file
* plot(scale,func,color)
* by plotting the second time over we actually will add on to our previous plot
* use close to remove the plot window
* figure(1); plot(1,y1) figure creates two window ... separate window for different plots 
* figure(2); plot(t,y2)
* subplot(1,2,1) divides plot a 1x2 grid access the first element
* plot(t,y1) sets the first plot subplot will create the first plot
* clf to clear the subplots
* imagesc(A) plots a matrix in color in response to value
* imagesc(A), colorbar, colormap grap

### Control statements
* for i = 1:10 ...function... end;
* while i <= 5... increment .... end;
* end all blocks with end;
* create a function by creating a file with the function name with .m extension
    * function y = squareThisNumber(x) % this x means one input and squareThisNumber is the function name
    * you must load the file first before running the function
    * addpath('PWD') to add a path to search

### Vectorization
*  Do things faster using matrices and do the operation all at once instead of using an iteration or for loop

## Classification
Again to reiterate
* Email:Spam not Spam
* Online Transactions: Fraudulent (yes/no)
* Tumor: Malignant / Benign 
The variables we want to predict are y which are part of {0,1} negative and positive classes
We can apply linear regression to fit a straight line to the data of the classification
Or we can threshold our classifier output h(x) at 0.5
* if h(x) >= 0.5 predict y = 1 (positive)
* if h(x) < 0.5 predict y = 0 (negative)

Linear regression might work, for a 0,1 problem however as we add more data, linear regression could be heavily skewed depending on the slope
Linear regression isn't good at giving us clearly defined classes, and it can give you classifications that are bigger than 0 or 1 even if the classes are defined {0,1}

### Logistic Regression
Will always give an output of y between 0<=H(x)<=1
When using linear regression we had theta'x now we will redfine this
* h(x) = g(theta'x)
* g(z) = 1/(1+e^-z) 
* h(x) = 1/(1+e^(-tehta'x))
* The Sigmoid or logistics function will cross (0.0.5) and asymptotes at 0 and 1 giving us an upper and lower bound

Interpretation of Hypothesis h(x)
* h(x) = estimated probability that y = 1 on input x
* If x = [x0 x1] = [1 tumorSize]
* h(x) = 0.7 this means for a patient with feature x has a 70 percent change of y = 1 or the tumor being malignant
* h(x) = P(y = 1 | x; theta) probability  that y =1 given x and paramterized by theta
* counting on the hypothesis to give us the likelihood of an output to be y = 1 or 0
    * P(y = 0 | x; theta) + P(y = 1 | x; theta) = 1
    * P(y = 0 | x; theta) = 1-  P(y = 1 | x; theta)

#### Decision Boundry
* Suppose we predict y = 1 if h(x) >= 0.5 then we predict y = 0 if h(x) < 0.5
* g(z) >= 0.5 when z >= 0 looking at the logistic graph therefore h(x) = g(theta'x) >= 0.5 whenever theta'x >= 0
    * Therefore our hypothesis is going to predict y =1 if theta'x >= 0
E.g if our h(x) had 3 features h(x) = g(theta0 + theta1x1 + theta2'x2)
    * theta0 = -3, theta1 = 1, theta2 = 1 => theta = [-3 ; 1 ; 1]
    * from our previous derived formulas, we see y = (1 if -3 + x1 + x2) :=theta'x >= 0
    * x1 + x2 >= 3 by moving the equality, x1 + x2 = 3 is our line that separates the classification called the decision boundry
    * x1 + x2 >= 3 we have y = 1
    * x1 + x2 < 3 we have y = 0
    * x1 + x2 = 3 -> h(x) = 0.5
Non linear decision boundaries so like circles and shit
E.g if our h(x) = g(theta0 + theta1x1 + theta2x2 + theta3x3^2 + theta4x4^2) 
    * theta0 = -1, theta1 = 0, theta2 = 0, theta3 = 1, theta4 = 1 [-1 ; 0 ; 0 ; 1 ; 1]
    * we will predict y = 1 if -1 + x1^2 + x2^2 >= 0 

Decision boundry is not defined by the trainning set, but is set by the parameters and values of the theta

## Cost function for logistics regression model
What we have
* Training set of m examples that is n+1 dimensional with our x0 = 1 and y being within the set of {0,1} 
* hypothesis, the parameter is theta h(x) = 1/(1 + e^ (-theta'x))
* So how do we choose the parameter theta given our data set? 

Cost function
* J(theta) = 1/m sum(cost(h(x),y)
* cost(h(xi),yi) = 1/2(h(xi)-yi)^2
* cost(h(x),y) = 1/2 of the square error between h(x) and y (between predicted value X and actual value Y)
* This is a non convex function and we have many local minimum, and we would like it if it was convex
    * so we want to rewrite and create a better function that is infact convex so we can find the global optimum

Logistic regression cost function
So below is how we describe the new cost function, which depending on what the actual correct output Y is, we have a different function to evaluate the cost of the prediction h(x)
    * cost(h(x),y) = -log(h(x)) if y = 1 else -log(1-h(x)) if y = 0
    * cost = 0 if y = 1, h(x) = 1 which our x0 should be
    * But as h(x) -> 0 cost -> infinity
    * Captures intuition that if h(x) = 0 predict P(y =1|x;theta) = 0, but y =1 we'll penalize learning algorithm by a very large cost
    * Cost = 0 if we predicted correctly
    * However if incorrectly predict and if the output of the hypothesis approaches zero the coost goes towards infinity

If y = 0 then we have -log(1-h(x))
* so we see that the curve is increasing to inifinity at 1
* and if predicted correctly we land at zero perfect certainty, while we get penalized for getting it wrong

## Gradient Descent with Logistics Regression
lets re write our cost function to be in one line so its simplified
cost(h(x),y) = -ylog(h(x) - (1-y)log(1-h(x)) just believe this is simpliefied 
so gradient descent is still repeat { thetaJ := thetaJ - alpha*(partial derievative of J(theta))
    * that partial deraivative = 1/m sum(i to m) of (h(x)-y)*xj this xj is the derviative term like x^2 -> 2x...
    * So theta starts off with a parameter theta of 0 - n vector and you'll update the whole vector
    * Vectorized implementation -> theta:= theta - alpha/m  * sum(i to m) of (h(x)-y) * xi

## Advanced Optimization
There lies more complex algorithms that allows us to compute more to assist us in cacluating our parameters theta
* Gradient descent
* Conjugate gradient
* BFGS
* L-BFGS
However we get significant advantages to these such as
* No need to manually pick alpha
* often faster than gradient descent
But disadvantages are that it is much more complex

 ## Multiclassification
 Learning algoprith mto put email in to different folders, work, friends, family, hobby
 with the appropriate assigning of class y =1 , y =2 , y =3
 Or medical diagrams: Not ill, Cold, Flu
 y = 1 2 3..... 

 All of these examples Y can take on many different values

 ### One vs all classification algorithm (One vs rest)
 If we have 3 classes then we are going to take the training set and turn it into 3 binary training sets, where its training set showing one vs the rest of the classes
 h theta 1 of class 1, and we repeat for the rest of the classes to draw up the decision boundraries
 So we fit three classifers into the probability of y = i given x and parameter theta hi(x) = P( y = i| x; theta) given i = 1 , 2, 3

 One vs all
 Train a logistic regression classifier hi(x) for each class i to predict the probability that y = i
 On a new input x, to make a prediction pick the class i that mkaximizes the hi(x) giving the highest probability

 ## Regularization and overfitting
 What is over fitting?
 We can fit data by adding more variables and dimensions to fit the curve

 Underfit / High Bias - When the linear regression cannot fit a non linear data set, high bias because the fit has a high tendency already feel like it has a bias to go up
 Overfitting / High Variance - the hypothesis almost fits it too accuratly and does not describe the data correctly

 Overfitting: If we have too many features the learned hypothesis may fit the trainning set very well, but fail to generalize to new examples (predict prices on new examples).

 Not only does this apply to linear regression, we can fit it to logistic regression

 ### Addressing overfitting
 We can plot hypothesis to decide which degree polynomials to use, however as features increase its much harder to plot
 1. Reduce number of features
    * Manually select which features to keep.
    * Model selection algorithm, algorithms that automatically decide which features to keep and which ones to kick out
        * By throwing away some of the features we are throwing away informations that contributes to the prediction
2. Regularization
    * keep all the features, but reduce magnitude/values of parameters thetaj
    * Works well when we have a lot of features each of which contributes a bit to predicting y.

### Cost function behind regularization
Suppose we penalize and make theta 3 and theta 4 really small
e.g min thetat 1/2m sum(i-mn)(h(x)-y)^2 + 1000theta 3 squared + 1000theta4 squared if those thetas are huge its impossible to avoid however if we make it close to 0 then its closer
Which means that theta 3 and theta 4 becomes irrelevant and close to zero meaning we essentially come up with an quadratic function

Regularization
Small values for parameters theta 1 ... n
    * simpler hypothesis
    * Less prone to overfitting
E.g Housing:
    * Features x1, x2 ... x100
    * Parameters theta1, theta2 ... theta100

We will take our cost function and modify it so it shrinks all of the parameters, and add a lamybda term at the end of it
J(theta) = 1/2m sum(h(x)-y)^2 + lambda sum(1 to n) of thetaj squared

Lamda is the regularization parameter, it controls the trade off between two different goals
    * We would like to fit the training data well
    * We want to keep the parameters small and therefore keeping the hypothesis simple to prevent overfitting

The bigger the lamdbda the more likely it is for us to underfit the data for extremely large values
We end up penalizing theta1 to theta n very heavily, then we will end up with all the parameters close to zero, which means then we get hypothesis of a straight line which doesn't help us

Regularization of linear regression
Gradient descent is modified if you remenber we don't add lambdas to theta 0 so we omit theta 0 from the one that uses lambdas and also the 

thetaj = thetaj(1 - a) 1/m sum(1 - m) (h(x) - y)dervative x + lambda/m theta j then we can rewrite it like so
thetaj = thetaj(1 - alpha lambda/m) - alpha(1/m)sum(1-m) (h(x) - yi)derivative x we see now its again the same as the old before

### Normal equation
x = [ x1'..] mx(n+1) dimension matrix and y (y1...m) vector size m
minimization of J(theta) -> theta = (X'X)'X'y
now we do regualrization we change it to theta = (X'X+ lambda[zero for 1,1, but identity afterwards)'X'y, so n = 2 
## If you have less data than there are features then the matrix is non invertiable or a degeneret matrix
But as long as lambda is greater than zero than that means the matrix is invertible

## Regularization for Logistics Regression
So from our old cost function using log we just add a new term of lambda/2m sum(1-n) of thetaj^2 | theta1.....thetan

Gradient descent
Again we separate out the theta zero since lambda of regularization does not apply to it, and we re modify the theta1-n bgy adding lambda/m*thetaj

Using Octave to implement the regularization

# Neural Networks
A previously known machine learning algorithm and have became the standard for ML now.
This can be applied when we have hundreds of features inside a classification algorithm and we need to narrow down our features to create the classification.
5000 features and multiple feature degree, and to actually classify these features we can't just use regular logistics regression to compute this

Why we need non linear hypothesis
Computer vision will be using feature vectors of a small photo would require an astounding amount of pixel values
n = features m = datasets 

The goal of neural networks have the purpose to model how the brain works
The "one learning algorith" hypothesis, brain learns through one way which is through snynapsis and then we can learn as these snynapsis can reroute and then remake new connectionst that makes sense

Neural Network Representation
Developed to simulate neurons in the brain. So our brains are packed with neurons that has a cell body and has input wires (Dendrite) and output wire (Axon) which sends messages to ther neurons
Neurons communicate with each other using spikes (pulses of electricity) And depnding on the inputs and outputs we communicate the to other neurons till we do all sorts of things

Neuron model: Logistic unit
We have 3 feature input and then a yellow circle will be the body of the neuron and does a computation and outputs to a functionX
X0 is the bias unit which we will include sometimes on the inputs
We describe neurons will activation function ... Sigmoid(Logistic) activiation function which is what function is used to determine if the inputs register to fire

Neural Network
A group of logistic unit strung together to communicate with each other.
First layer is called the input layer
Intermidiant layer are the hidden layer -> we don't get to see the values observed in the training set, values generated by the learning algorithm
Last layer is the output layer which is final value computed by the hypothesis

a{j}[i] activation of unit i in layer j {} -> super script [] -> subscript
theta{j} matrix of weights controlling function mapping from layer j to layer j + 1
If networks has s[j] units in layer j, s[j+1] units in layer j + 1, then theta{j} will be of dimension s[j+1]x (s[j] + 1)

Forward propagation is what is computing the activation from the input to the hidden to the output layer. 
It is a feedforward system whereby our inputs are computed through the  linking of different nodes to compute the next inputs

** switched to paper notes because of more math based text **