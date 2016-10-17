
## Load Libraries
The first task is to download all the libraries I might use during during the analysis. The caret package and the randomForest library
```
#load libraries
library(caret)
library(randomForest)
```

## Prepare Data
The data proparation includes the removal of the columnos with NotANumbers, and not numeric or errors(DIV/0). I also remove from the dataset the data which contain non relevant info (the first columns)
```{r cache = TRUE, message=FALSE}
#load data and have a look at it, remove the columns with NA
trainingData = read.csv("pml-training.csv", na.strings=c("NA","#DIV/0!","")); 
#summary(trainingData);
#remove columns without relevant info
trainingData <-trainingData[,-c(1:7)]

```
## Perform Data Cleaning
The last step in the data preparation includes the removal of those columns that have 0 values for all the measurements, as they are likely to be errors
```{r cache = TRUE, message=FALSE}
#do some cleaning
trainingData <- trainingData[, colSums(is.na(trainingData)) == 0]

```
## Processing Data
```{r cache = TRUE, message=FALSE}
#split into training and validation
trainingPartition <- createDataPartition(y=trainingData$classe, p=0.6, list=FALSE)
myTraining <- trainingData[trainingPartition, ]
myValidation <- trainingData[-trainingPartition, ]
dim(myTraining); dim(myValidation);

```
## Model : Random Forest
```{r cache = TRUE, message=FALSE}
#model 1 RF
modRF <- randomForest(classe ~. , data=myTraining)
predRF <- predict(modRF, myValidation, type = "class")
confusionMatrix(predRF , myValidation$classe)
# The model looks good, so I'm not trying any other

```
## Processing Data
```{r cache = TRUE, message=FALSE}
#model test
testData = read.csv("pml-testing.csv")
predict(modRF,testData)

```
