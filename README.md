
## Load Libraries
#load libraries
library(caret)
library(randomForest)

## Prepare Data
#load data and have a look at it, remove the columns with NA
trainingData = read.csv("pml-training.csv", na.strings=c("NA","#DIV/0!","")); 
#summary(trainingData);
#remove columns without relevant info
trainingData <-trainingData[,-c(1:7)]

## Perform Data Cleaning
#do some cleaning
trainingData <- trainingData[, colSums(is.na(trainingData)) == 0]

## Processing Data
#split into training and validation
trainingPartition <- createDataPartition(y=trainingData$classe, p=0.6, list=FALSE)
myTraining <- trainingData[trainingPartition, ]
myValidation <- trainingData[-trainingPartition, ]
dim(myTraining); dim(myValidation);

## Model : Random Forest
#model 1 RF
modRF <- randomForest(classe ~. , data=myTraining)
predRF <- predict(modRF, myValidation, type = "class")
confusionMatrix(predRF , myValidation$classe)
# The model looks good, so I'm not trying any other

## Processing Data
#model test
testData = read.csv("pml-testing.csv")
predict(modRF,testData)
