# Predicting Race In Law Enforcement Killings In the US

## Background & Problem Statement

Racial discrimination in law enforcement responses in the US remains a discernible phenomenon and pressing issue.
Once racial values imputations are near true accuracy, the dataset can be used to provide better insights about the nature of racial discrimination in American policing practices.
In turn, these insights can hopefully contribute to addressing systemic racism in law enforcement responses across the US. 

Fatal Encounters is an organization that compiles and publishes data on individuals that have been killed by the police, with an observation for each person.

The current database includes details surrounding each incident, as well descriptive and identifying information for each person.
One of these features is the racial identity of the victims, and currently, of the ~32,000 observations, approximately 8,300 observations fall into the "Race Unspecified" category.Fatal Encounters has already created a model which, in two separate features, imputes the racial identity value and provides a corresponding probability of its strength of accuracy.

Goal: create a model which predicts the correct racial classification for individuals in the dataset who are categorized as "Race Unspecified" more accurately than the current model Fatal Encounters uses. 

## Work Flow

1. Data Cleaning and Merging
2. EDA
3. Baseline Scores
4. Test on Models
5. NLP on Description
6. Neural Network
7. Unsupervised Learning
8. Evaluation
9. Prediction Comparison

## Data Cleaning & Merging 
 
### Meta Data I

|     name            |     columns    |     rows      |     description                                                                      |     example                                                 |
|---------------------|----------------|---------------|--------------------------------------------------------------------------------------|-------------------------------------------------------------|
|     Incidents       |     32         |     30,413    |     Details   surrounding each incident, identifying information for each person.    |     Name,   gender, age, race, city, state, county, date    |
|     Census          |     178        |     3,168     |     Various   demographic info on county basis                                       |     Tot_pop_2010,   med_inc_2017, pct_ain_2015              |
|     Last   Names    |     151,063    |     10        |     Aggregated   last names at birth (2011) including racial composition             |     Pct_api,   pct_blck,   pct_his                          |
|     States          |     2          |     50        |     State   names in full and abbreviated formats                                    |     Name,   code                                            |

### Data Cleaning

Dropping columns and rows

Checking nulls
 Dropping columns with too many
 Imputing for numericals
 Dropping rows

Checking unique values 
 Dropping categoricals with too many
 Checking for categorical/numeric conflation 
      with smaller values

Creating features
 Last Name
 County, State
 Agency
 Date
 Dummifying categoricals
 
Formatting column names
Inner merging

### Merged DataFrame Structure

Excluded:
Counties with no incidents
Individuals with last names not in eponymous database.

| DataFrame        | Columns | Rows   |
|------------------|---------|--------|
| Complete         | 273     | 26,007 |
| Train (Labeled)  | 273     | 19,628 |
| Test (Unlabeled) | 272     | 6,739  | 


## EDA

Total number of incidents labeled ‘Race unspecified’ from 2000 to present.
Peaks in early 2000’s
Trough in mid 2010’s
Rapidly rising
Incomplete data 2021

Total number of incidents is loosely proportionate to state total population.
California has the most total number of incidents, perhaps outsize.
New York is a good exception.

Number of incidents where race is unspecified is not proportionate to state total population.
California still leads in volume.
Texas comes and Florida both come after Georgia, Pennsylvania, and Missouri.

## Baseline Scores

|     Racial   Cat.                |     Norm.   Value Counts    |
|----------------------------------|-----------------------------|
|     European-American/White      |     0.475596                |
|     African-American/Black       |     0.316334                |
|     Hispanic/Latino              |     0.176075                |
|     Asian/Pacific   Islander     |     0.018035                |
|     Native   American/Alaskan    |     0.012686                |
|     Middle   Eastern             |     0.001274                |

## Models

|     Name                        |     Train    |     Test    |
|---------------------------------|--------------|-------------|
|     Logistic   Regression       |     0.82     |     0.80    |
|     Ridge   Classifier          |     0.81     |     0.80    |
|     KNN                         |     0.74     |     0.70    |
|     Decision   Tree             |     0.99     |     0.71    |
|     Random   Forest             |     0.86     |     0.79    |
|     Bagging   Classifier        |     0.98     |     0.78    |
|     Support   Vector Machine    |     0.84     |     0.79    |
|     ADA   Boost                 |     0.67     |     0.67    |
|     Extra   Trees               |     0.99     |     0.77    |
|     Gradient   Boost            |     0.85     |     0.80    |
|     XGBoost                     |     0.97     |     0.80    |

All models’ accuracy scores capped around 0.80
Tree based models seemed to very well on training data, but lost some testing accuracy
Gradient Boost had the highest scores, and was slightly overfit
ADA Boost had the worst scores, but was the least overfit


## Logistic Regression with TFIDF



 ### TFIDF Coefficients

Most predictive value: names common among black-Americans, latino-Americans and white-Americans
Least predictive value: odd verbs, oxycontin, ‘28’. 

## Neural Networks

5 Dense Layers
Softmax activation function
Early Stopping
11 Epochs
Loss: 0.40
Val loss: 0.54
Training: 0.85
Testing: 0.79

## Unsupervised Learning

|     Racial Cat.    |          0        |          1        |          2        |          3        |          4        |          5        |
|:------------------:|:-----------------:|:-----------------:|:-----------------:|:-----------------:|:-----------------:|:-----------------:|
|         A&B        |     0.09411765    |     0.14414414    |     0.22047244    |     0.33619702    |     0.58906752    |     0.34685864    |
|         A&PI       |     0.00392157    |     0.01201201    |     0.08661417    |     0.01546392    |     0.00192926    |      0.0104712    |
|          EW        |     0.85441176    |     0.40840841    |     0.20472441    |     0.52806415    |     0.34662379    |     0.34816754    |
|         HIS        |     0.04362745    |     0.42342342    |     0.48818898    |      0.1185567    |     0.06173633    |     0.29057592    |
|         MID        |          -        |          -        |          -        |     0.00057274    |          -        |      0.0039267    |
|         NAT        |      0.003921     |      0.0120120    |          -        |     0.00114548    |      0.0006430    |          -        |                                           |

## Evaluation

| Metric       | Score |
|--------------|-------|
| Accuracy     | 0.82  |
| Recall Score | 0.80  |
| Precision    | 0.80  |
| F1           | 0.80  |

## Interpreting coefficients

|          |     Feature Name                        |     Coeff. Val.    |
|----------|-----------------------------------------|--------------------|
|     0    |     name_pct_black                      |     2.381574       |
|     1    |     unemployed_2017                     |     2.023155       |
|     2    |     sales_2007                          |     1.925037       |
|     3    |     private_nonfarm_estblhments_2009    |     1.845359       |
|     4    |     fed_spending_2009                   |     1.818185       |
|     5    |     pop_black_2010                      |     1.791341       |
|     6    |     persons_per_household_2017          |     1.673634       |
|     7    |     two_plus_races_2010                 |     1.655088       |
|     8    |     State_FL                            |     1.531944       |
|     9    |     State_GA                            |     1.526401       |

Portions of black American having a particular last name or living in a particular county were both important features when predicting race.
Demographic features such as portion of Americans identifying as two or more races, and household density.
County based economic data such as unemployment, total sales, count non-farm businesses, and federal spending. 
State location: Florida and Georgia


## Model Performance Comparison

|     Racial Cat.                  |     Trevor    |     F.E.            |     Baseline    |
|----------------------------------|---------------|---------------------|-----------------|
|     European-American/White      |     0.55      |     0.63            |     0.47        |
|     African-American/Black       |     0.31      |     0.22            |     0.31        |
|     Hispanic/Latino              |     0.12      |     0.13            |     0.17        |
|     Asian/Pacific   Islander     |     0.01      |     0.01            |     0.01        |
|     Native   American/Alaskan    |     0.001     |     N/A             |     0.01        |
|     Middle   Eastern             |     0.001     |     N/A             |     0.001       |

## Conclusions

The racial distribution of a last name is the probably the most powerful predictor of race.
On average our model predicted race with slightly less confidence than the Fatal Encounters Model. 
Taking the most confident prediction from both datasets provides the best dataset.

### Project Future

More data:
More levels of geographic census info 
ZIP
State
City
More/all years
More names (or workaround)
Create more features
Hyperparameter tuning & regularization
RNN with TFIDF
Time Series
Parallel computing
Reach Out to Fatal Encounters

