# Median-knn_imputation
We use the dataset BreastCancer in order to put into practice meidan and Knn imputation. This dataset is interesting because many of the predictors contain missing values  and most rows of the dataset have at least one missing value

#setwd("")
load("BreastCancer.RData")


#This dataset is interesting because many of the predictors contain missing values 
#and most rows of the dataset have at least one missing value



library(caret)
library(RANN)
myControl<- trainControl(method = "cv", number = 10, verboseIter = TRUE)
####################################################################
#Median Uputation
####################################################################
# Apply median imputation: model
model <- train(
  x = breast_cancer_x, y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = "medianImpute"
)

# Print model to console
model
####################################################################
#KNN imputatin
####################################################################
# Apply KNN imputation: model2
model2 <- train(
  x = breast_cancer_x, y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = "knnImpute"
)

# Print model to console
model2
####################################################################
#Compare KNN and median imputation
####################################################################
median_model <- model
knn_model <- model2
resamples <- resamples(x = list(median_model = median_model, knn_model = knn_model))
#Plot to see
dotplot(resamples, metric = "Accuracy")
#knn model is slightly better.




####################################################################
#Combining preprocessing methods
####################################################################
# Fit glm with median imputation: model1
model1 <- train(
  x = breast_cancer_x, y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = "medianImpute"
)

# Print model1
model1

# Fit glm with median imputation and standardization: model2
model2 <- train(
  x = breast_cancer_x, y = breast_cancer_y,
  method = "glm",
  trControl = myControl,
  preProcess = c("medianImpute", "center", "scale")
)

# Print model2
model2

