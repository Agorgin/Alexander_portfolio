# Alexander's Data Science Portfolio
This is my data science portfolio! Here, I will be giving an overview of and showcase the academic projects I have done.

# [Project 1: Air France](https://github.com/Agorgin/Air-france-business-case)

This is my first project on the Air France business case using R!

![Title Image](https://github.com/Agorgin/Alexander_portfolio/blob/main/titleimage.jpg)


My team and I analyzed Air France's international marketing campaigns and have generated recommendations for effective search engine marketing.

The dataset is sources from the case study on Air France International Marketing by Kellog School of Business. The external libraries used are readxl, mlbench, dplyr, plotly, ggplot2, tidyverse.

The project employs visual and statistical tools in R and applies a strategy for missing values. Additionally, my team added important metrics for subsequent analysis, i.e. return on advertisement and net income.

Then, my team used descriptive analysis to identify the most effect marketing channel and generate a publiser strategy.

Lastly, we shared our findings a 6 minute presentation in form of a video presentation. Below are important code snippets from the projects:

### Logistic regression
```markdown
#
#Creating binaries assigning Profit (1) non profit (2)              

netincome <- airfrance$netincome #create new vector
binary_income <- netincome
binary_income[binary_income < 0] <- 0 #negative values are 0
binary_income[binary_income > 0] <- 1 #positive values are 1
View(binary_income)

airfrance$binary <- binary_income #assign the vector to the data frame

#Splitting into training and testing, following the 80/20 rule allocating 80% for training the model and 20% for testing
train_index <- sample(1:nrow(airfrance), size = 0.8*nrow(airfrance)) #training sample
train_data <- airfrance[train_index, ] #store into dataset
test_data <- airfrance[-train_index,] #exclude the train dataset for test data

#Running the regression of normalized variables

logistic_income <- glm(binary  ~  CC_norm +  Impressions_norm +
                         AP_norm + Amount_norm + TC_norm + TVB_norm , data = train_data, family = binomial) #training the regression

```

### Creating a GINI decision tree

```markdown

#GINI Tree
my_tree <- rpart(binary~  CC + Impressions +
                   AP + Amount + TVB , data = train_data, method = "class", cp = 0.001) #building the GINI tree


#plotting the  tree
rpart.plot(my_tree, extra = 1, type = 1) 

```
![GINI Tree](https://github.com/Agorgin/Alexander_portfolio/blob/main/Screenshot%202021-11-08%20at%2012.35.02%20AM.png)



