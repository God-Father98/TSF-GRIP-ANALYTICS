# TSF-GRIP-ANALYTICS
Code for the regression analysis 

### Importing libraries
import pandas as pd
import matplotlib.pyplot as plt
from sklearn import linear_model

### Getting data
url = "http://bit.ly/w-data"
df = pd.read_csv(url)

print("Data imported successfully")
df.head(3)

### Data view for the First time
df.plot(x = "Hours", y = "Scores", kind = "scatter")
plt.show()

### First step to check the Correlation
df.corr()

### Defining variables as DataFrames
score= df[['Scores']]
hour = df[["Hours"]]


## Building linear Regression Model
reg = linear_model.LinearRegression()
### independent first and dependent Second in model
model = reg.fit(hour,score)

### Describing the Constant and variable part.
variable = model.coef_
constant = model.intercept_

### Creating Regression line by the basic formulae y = m +bx
line = variable*hour + constant
plt.scatter(hour, score)
plt.xlabel("REGRESSION LINE ")
plt.plot(hour, line);
plt.show()

## finding R square (evaluating Model)
rs = model.score(hour , score)

## Adding PREDICATED values into the Data frame
df[["Predicted"]] = hour*variable + constant
# we can predict Now
# y = m + bx
hours = 9.25
prediction = constant + (variable)*hours
print(prediction)
