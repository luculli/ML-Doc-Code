# Info-RandomForest
Some useful links and info on random forest methods

* [[RF+Clustering+MDS]](https://stats.stackexchange.com/questions/134095/how-to-get-class-probabilities-for-unsupervised-random-forest/183116) - Nice example of how to attach a probability to predictions based on RF. Key idea: 1) build a cluster based on RF proximity metric; 2) then you can use your clusters as classes to train a supervised model; 3) adopt your supervised model to make prediction on new data.
```
library(mlr)

n = nrow(iris)
iris.train = iris[seq(1, n, by = 2), -5]
iris.test = iris[seq(2, n, by = 2), -5]
task = makeClusterTask(data = iris.train)
mod = train("cluster.kmeans", task)

newdata.pred = predict(mod, newdata = iris.test)

## get a posteriori probabilities create a Learner
lrn = makeLearner("classif.rpart", predict.type = "prob")
mod = train(lrn, iris.task)

pred = predict(mod, newdata = iris)

head(as.data.frame(pred))
head(getPredictionProbabilities(pred))
calculateConfusionMatrix(pred)
conf.matrix = calculateConfusionMatrix(pred, relative = TRUE)

## Classification: Adjusting the decision threshold
lrn = makeLearner("classif.rpart", predict.type = "prob")
mod = train(lrn, iris.task)
pred = predict(mod, newdata = iris)
pred$threshold

## Visualizing the prediction
lrn = makeLearner("classif.rpart", id = "CART")
plotLearnerPrediction(lrn, task = iris.task) 
```

