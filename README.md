# Analyst-Meets-Soccer_Data
Comparing Simple Regression, Simple Regression with CV & Lasso Regression

## Choosing a sample
One way to choose a sample out of a significantly heterogeneous dataset for model training is to
use “Leave One-Out Cross Validation”. An example of this method is to randomly order the
dataset, and then divide the data set into 5 parts. Then the model is trained on the data from 4
parts and is tested on the 5th part. Then this process is repeated with a different combination of 4
training parts and 5th testing part.

The benefit of this method specifically regarding heterogenous data is that some of the
“heterogeneous” observations (AKA, “extreme outliers”) will at one point be left out of the
training data. This will avoid overfitting a model to all the outlier observations. Other benefits
are that each observation will be used for both training and validation, and the sampling variance
of cross validation model selection will be lower.

## Fitting a simple regression model
In the first exercise, we fit a simple linear regression model to predict the overall score of players
within the FIFA Dataset. This was done after randomly splitting the data into test and training
sets with only 10% of the records in the training set. One way to measure the accuracy of the
model is by measuring how close the data is to the fitted regression line through the R-squared
statistic measure.

To perform this simple linear regression model, we first cleaned our data by removing null
values. We then created indicator variables for categorical data after determining if there was a
correlation between them and the dependent variable using the Spearman correlation test. We
eliminated the features that were highly correlated with one another to reduce multicollinearity
effect. Although multicollinearity is not an issue when the model is used for prediction, it helped
us in improving the model stability and a higher R2 value with high adjusted R2 as well. We had
a total of 33 features. After fitting our linear regression model, we obtained an R2 value of
approximately 0.921.

## Fitting a simple regression model with 5-fold cross validation
Next, we tried tweaking our model by applying a different technique (cross validation using kfold). We trained the model using 10% of the data. Then by choosing 5-fold cross validation, the
test set was split into 5 parts with the same ratio of training / test sets which alternated until all
possible combinations were made.

After fitting our simple regression model with a 5-fold cross validation, we obtained a prediction
with an R2 of approximately 0.922 which is the average of the accuracy of each fold. In both
cases (Part 1 and 2), the R2 was quite high, but the accuracy (measured using R2) was slightly
higher using a 5-fold cross validation. This may be due to the cross-validation’s better
performance on unseen data. The higher R2 values explains that the model is good at predicting
the data that it hasn’t seen already. 

## Fitting a Lasso Regression
For this part, we used Lasso regression to predict the overall scores of players in the test set
using the default value of alpha = 1. At this value of alpha, the number of features used in the
model was 20 with an R2 of 0.915. While the value is still relatively high, it is smaller than the
R2 obtained from the simple linear regression model fitted in part 1.

Essentially, Lasso regression operates using a shrinkage technique which focuses on reducing
high levels of multicollinearity rather than trying to maximize the prediction accuracy. Hence, in
the case of using alpha = 1, the number of features used in the model was reduced to 20. If the
focus is to improve prediction error, then perhaps using a simple linear regression model is more
effective; however, Lasso regression may be a better tool for interpretability. Since we had a
high value of 1 for the alpha, the lasso optimized by reducing the number of features in the
model which explains the slight reduction in the R2 value over the one obtained from the linear
regression model.

## Comparing Ridge & Lasso
Ridge and Lasso Regressions operate similarly in that they try to decrease the model complexity
by minimizing the number of predictors. Instead of removing them using backward or forward
selection, we can examine the variable’s effect on the response by penalizing those predictors
that have coefficients far away from zero. The theoretical difference between Ridge and Lasso
regression is that Ridge’s penalty term uses a squared magnitude of coefficient whereas, Lasso’s
penalty term uses an absolute magnitude of the coefficient.

As a result, Lasso shrinks the less important features’ coefficients to 0 and eliminates them from
the model. Accordingly, Lasso will have fewer features. While this may be better for feature
selection, it would lead to yielding a less accurate prediction. This was re-validated when testing
our model where we observed a higher R2 value (0.921) with a higher number of features in the
model.

## Fitting a Lasso Regression with Optimal Alpha
According to our code, the ideal value of alpha is 0.001 where 32 features are utilized. Using
these 32 features, an R2 value of 0.924 was deduced which is higher than the value obtained
from Part 1 (0.921). The main issue with Lasso is that when the model has correlated variables, it
will keep one variable while setting other variables to zero thus sacrificing the accuracy of the
model to avoid multicollinearity. Nonetheless, by looking for an ideal value for alpha, we are
targeting the best estimator which would yield the highest model accuracy rather than focus on
minimizing the number of features that may contribute to multicollinearity.

## Assessing the best with Information Criterion
According to the results obtained, we notice that the values of AIC / BIC are lower in the case of
our Lasso regression model which we built in Part 5. Having a lower AIC or BIC value indicates a 
better fit; hence, the Lasso model built in Part 5 was a better model. The only difference between
AIC and BIC is that BIC considers the number of observations as shown in the formula below:

AIC = -2*ln(L) + 2*k
BIC = -2*ln(L) + 2*ln(N)*k

Accordingly, at any number of observations above 2, the value of BIC would always be
higher. After deducing the corrected AIC for both models, we notice that the values are
extremely close (21849.14 vs. 21848.99) for the simple linear regression and (21838.44 vs.
21838.30) for the Lasso regression. This is usually the case when the number of observations is
large, so the corrected AIC converges to AIC.

## ICs vs. CVs
Information Criteria (IC) are alternatives to Cross Validation. ICs should not be trusted entirely
the same as Cross Validation, especially depending on the situation. In general, an Information
Criterion such as AIC only estimates a model’s out-of-sample deviance. By contrast, k-fold
Cross Validation, for example, uses k-1 parts of the data to train a model, and then uses the kth
part to test the model. So this method actually tests the model against out-of-sample data; it does
not just estimate the model’s deviation on out-of-sample data like AIC.

In addition, in the specific situation where the data is very large such that the number of
parameters (which is also the number of degrees of freedom, “df”) equals or nearly equals the
number of observations (n), then AIC will give a bad approximation and will overfit the data. (In
this case the corrected AIC (AICc) should be used.) But when comparing the AIC to the Cross
Validation in this case, AIC does not perform as well. 
