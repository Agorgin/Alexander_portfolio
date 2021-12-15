# Alexander's Data Science Portfolio
This is my data science portfolio! Here, I will be giving an overview of and showcase the academic projects I have done.

# [Project 1: Air France](https://github.com/Agorgin/Air-france-business-case)

This is my first project on the Air France business case using R!

![Title Image](titleimage.jpg)


My team and I analyzed Air France's international marketing campaigns and have generated recommendations for effective search engine marketing.

The dataset is sources from the case study on Air France International Marketing by Kellog School of Business. The external libraries used are readxl, mlbench, dplyr, plotly, ggplot2, tidyverse.

The project employs visual and statistical tools in R and applies a strategy for missing values. Additionally, my team added important metrics for subsequent analysis, i.e. return on advertisement and net income.

Then, my team used descriptive analysis to identify the most effect marketing channel and generate a publiser strategy.

Lastly, we shared our findings a 6 minute presentation in form of a video presentation. Below are important code snippets from the projects:

# Match type and Publisher Name
The two variables used to measure business success are Match Type and Publisher Name.

### Match type
Match Type fall under four categories:
* Advanced
* Broad
* Exact
* Standard 

These can tell us how accurate was the match based on the Keyword, and is the one of the two metrics to define the future strategy. 

### Publishers
* Yahoo US and Global
* MSN US and Global
* Google US and global
* Overture US and Global
* Kayak

These can tell us which publisher perform the best, to narrow down the strategy and increase efficiency. 

# Insights from plots



# Logistic regression
```markdown

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
### Insight
Moving to a more predictive part of our analysis, we built a model we can then use to test possible future strategies to see whether we can expect them to be successful in meeting our objective of positive net income. So we defined a positive net income as 1 (business success) and negative net income as 0 (business failure)

First we cleared any highly correlated variables among them to avoid collinearity,  we normalized the dataset since we were dealing with different units and ranges, we ran the regression a number of times and we kept working with it to see what was the best combination to explain the behaviour of net income. Bleow is a snippet of using a User Defined Function to normaliz our data:

```markdown
#Normalizing data with a UDF
normal <- function(x){(x-min(x)) / (max(x)-min(x)) }

#Normalizing our data
train_data$CC_norm <- normal(train_data$CC) 
train_data$Impressions_norm <- normal(train_data$Impressions) 
train_data$AP_norm <- normal(train_data$AP) 
train_data$Amount_norm <- normal(train_data$Amount) 
train_data$TC_norm <- normal(train_data$TC) 
train_data$TVB_norm <- normal(train_data$TVB) 
```
Overall, very briefly going through our conclusion is that the normalised model we reached signals our variables to be significant at a 90% confidence level and the most influential coefficients in terms of their marginal effect on success were Average positioning in a (–) way and Volume of bookings in a (+)




# GINI decision tree

```markdown

#GINI Tree
my_tree <- rpart(binary~  CC + Impressions +
                   AP + Amount + TVB , data = train_data, method = "class", cp = 0.001) #building the GINI tree


#plotting the  tree
rpart.plot(my_tree, extra = 1, type = 1) 

```
![GINI Tree](https://github.com/Agorgin/Alexander_portfolio/blob/main/Screenshot%202021-11-16%20at%2011.26.12%20PM.png)
### Insight
Amount being the most relevant variable, it’s the first one on the tree. If the amount of the transaction is less than 100, then there is a 100% probability that the net income is going to be negative. 

The second most relevant variable is Click Charges (CC). If Amount is bigger than 100 and Click Charges is less than 1357, there is 98%  probability of having a positive net income.

From the Gini tree we can determine that Amount and Click Charges are the variables that mainly influence the outcome.




# Conclusion
Prioritize the Publisher and Match type that give us the highest ROA and Net income: 
* Yahoo-US
* Google-Global
* MSN Global
* Match type of exact

Furthermore, by analyzing the GINI tree, all of the searches that give less that $100 in revenues should be dropped, as well as those with particularly high Click Charges. 




# [Project 2: Business Insight Report - NLP ](https://github.com/Agorgin/Air-france-business-case)
UPCOMING: This project will analyze text data in R using at least 2 frameworks: N-grams, sentiment analysis, tf-idf, correlograms, classification with Bayes, LDA, .etc).



```markdown

# Import the libraries
library(tidytext)
library(tidyverse)
library(tidyr)
library(tidytuesdayR)
library(stringr)
library(textreadr)
library(pdftools)
library(textshape)
library(twitteR)
library(tm)
library(ggplot2)
library(scales)
library(magrittr)
library(dplyr)
library(gutenbergr)
library(Matrix)
library(textdata)
library(igraph)
library(ggraph)
library(widyr)
library(topicmodels)
library(gutenbergr)
library(quanteda)
library(quanteda.textmodels)
library(RColorBrewer)
library(readr)

```

#Data Wrangling
```markdown
df_comment <- read_csv("/Users/johndinh/Documents/GitHub/AirBnB/Assets/reviewer_comments.csv")

df_names <- read_csv("/Users/johndinh/Documents/GitHub/AirBnB/Assets/stop_name.csv")

df_names$word <- tolower(df_names$word)

custom_stop_words <- tribble(
  ~word, ~lexicon,
  "http", "CUSTOM",
  "https", "CUSTOM",
  "t.co", "CUSTOM",
  "apartment", "CUSTOM",
  "de", "CUSTOM",
  "à", "CUSTOM",
  "的", "CUSTOM",
  "la", "CUSTOM",
  "très", "CUSTOM",
  "muy", "CUSTOM",
  "stay", "CUSTOM",
  "location", "CUSTOM",
  "el", "CUSTOM",
  "es", "CUSTOM",
  "le", "CUSTOM",
  "é", "CUSTOM",
  "很", "CUSTOM",
  "nos", "CUSTOM",
  "se", "CUSTOM",
  "les", "CUSTOM",
  "wir", "CUSTOM",
  "haben", "CUSTOM",
  "die", "CUSTOM"
  
)

Binding custom stop words 

stop_words_air <- stop_words %>% 
  bind_rows(custom_stop_words)

stop_words_air <- stop_words_air %>% 
  bind_rows(df_names)


tidy_comment <- df_comment %>% 
  group_by(property_type) %>% 
  mutate(linenumber = row_number()) %>% 
  ungroup() %>% 
  unnest_tokens(word, comments) %>%
  anti_join(stop_words_air)


tidy_comment_no_stop <- tidy_comment %>% 
  anti_join(stop_words_air)

most_comment_words <- tidy_comment_no_stop %>% 
  count(property_type, word, sort = TRUE )

```

# Correllgram between Property Types

```markdown

#Correlgram

frequency <- tidy_comment%>%
  anti_join(stop_words_air) %>% 
  mutate(word=str_extract(word, "[a-z']+")) %>%
  count(property_type, word) %>%
  group_by(property_type) %>%
  mutate(proportion = n/sum(n))%>%
  select(-n) %>%
  spread(property_type, proportion) %>%
  gather(property_type, proportion, `Condominium`, `House`)

#let's plot the correlograms:

library(scales)
ggplot(frequency, aes(x=proportion, y=`Apartment`, 
                      color = abs(`Apartment`- proportion)))+
  geom_abline(color="grey40", lty=2)+
  geom_jitter(alpha=.1, size=2.5, width=0.3, height=0.3)+
  geom_text(aes(label=word), check_overlap = TRUE, vjust=1.5) +
  scale_x_log10(labels = percent_format())+
  scale_y_log10(labels= percent_format())+
  scale_color_gradient(limits = c(0,0.001), low = "darkslategray4", high = "gray75")+
  facet_wrap(~property_type, ncol=2)+
  theme(legend.position = "none")+
  labs(y= "Apartment", x=NULL)

```

![Title Image](titleimage.jpg)



