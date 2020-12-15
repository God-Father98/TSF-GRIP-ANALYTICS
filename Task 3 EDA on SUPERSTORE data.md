import pandas as pd

import seaborn as sns

import matplotlib.pyplot as plt

%matplotlib inline

import numpy as np

import plotly as py

import plotly.express as px

from plotly.offline import iplot

import cufflinks as cf

cf.go_offline()

df = pd.read_csv("SampleSuperstore.csv")

df


df.isnull().sum()

df.duplicated().value_counts()

print(df[["Ship Mode"]].value_counts(),"\n")

print(df[["Segment"]].value_counts(),"\n")

print(df[["Region"]].value_counts(),"\n")

print(df[["Category"]].value_counts(),"\n")

print(df[["Sub-Category"]].value_counts(),"\n")



s =df['Sales'].sum()

p =df['Profit'].sum()

print('Total Sales  =',s)

print('Total Profit =',p)

print('Profit Percentage = ',(p/(s-p))*100,'%')


# 1
#REGION WISE SALES AND PROFITS

df_cat = df.groupby(["Region"],as_index = False)['Profit',"Sales"].sum()

df_cat['Cost']= (df_cat.iloc[:,2]-df_cat.iloc[:,1])

df_cat['Percentage']= (df_cat.iloc[:,1]/df_cat.iloc[:,3])*100

df_cat

frame = px.bar(data_frame=df_cat,x='Region', y= ['Profit','Cost'],color_discrete_sequence =['seagreen','darkslategray'],title = 'SALES AND PROFIT (Region-Wise)',width = 400,height=500)

frame.show()




pair = df[['Region', 'Sales','Discount','Quantity','Profit']]

sns.pairplot(data=pair, hue= 'Region')


### RESULTS:
WEST REGION IS AT THE TOP IN SALES AS WELL AS PROFITS
AND 
CENTRAL IS THE WORST HIT.
____________________________________________________________________________________________________________________________

# 2
## REGION AND CATEGORY WISE

dfgroup = df.groupby(['Region','Category'],as_index = False)['Profit','Sales'].sum()

dfgroup['Cost']= (dfgroup.iloc[:,3]-dfgroup.iloc[:,2])

dfgroup['Percentage']= (dfgroup.iloc[:,2]/dfgroup.iloc[:,4])*100

dfgroup


frame1 = px.bar(data_frame=dfgroup,x='Region', y= ['Profit','Cost'],color='Category', color_discrete_sequence=['crimson','dodgerblue','forestgreen'],title = 'COST AND PROFIT(Category and Region wise)',width = 800,height=500,barmode= 'group')

frame1.show()


## RESULTS FOR 2
WEST OFFICE SUPPLIES ARE PROVIDING THE HIGHEST PROFIT PERCENTAGE

AND CENTRAL FURNITURE IS SHOWING TEH NEGATIVE PERCENTAGE.

Although, furniture in all categories is showing a good amount of sales but is not able to generate profits or is barely generating any. so the worst hit part in category wise is FURNITURE.
____________________________________________________________________________________________________________________________


# 3
## SUB CATEGORY ANALYSIS

dfsubgroup = df.groupby(['Sub-Category'],as_index = False)['Profit','Sales'].sum()

dfsubgroup['Percentage']= (dfsubgroup.iloc[:,1]/dfsubgroup.iloc[:,2])*100

colorsub= ['darkslategray','teal','midnightblue','crimson','blue','cornflowerblue','lightpink','palegreen','greenyellow','yellow','darkorange','orange','navajowhite','tomato','salmon','chocolate','maroon',]

frame3= px.bar(data_frame=dfsubgroup,x='Sub-Category', y= ['Sales'],color='Sub-Category',color_discrete_sequence =colorsub,title = 'SALES(Sub-Category-Wise)',width = 1200,height=500,barmode= 'stack')
frame3.show()

frame4= px.bar(data_frame=dfsubgroup,x='Sub-Category', y= 'Profit',color='Sub-Category',color_discrete_sequence = colorsub,title = 'PROFIT (Sub-Category-Wise)',width = 1200,height=500,barmode= 'stack')
frame4.show()


## RESULTS 3
Here we found that Tables are the negative percentage and bookcases are 2nd runner up..
___________________________________________________________________________________________________________________________


# 1-2-3
Getiing a Bird's Eye View on the Data so far.


dfcatsub = df.groupby(['Region','Category','Sub-Category'],as_index = False)['Profit','Sales'].sum()

dfcatsub['Percentage']= (dfcatsub.iloc[:,3]/dfcatsub.iloc[:,4])*100

dfcatsub['Cost']= (dfcatsub.iloc[:,4]-dfcatsub.iloc[:,3])

dfcatsub

frame2 = px.bar(data_frame=dfcatsub,x='Sub-Category', y= ['Profit','Cost'],color='Category', color_discrete_sequence=['crimson','dodgerblue','forestgreen'],title = 'COST AND PROFIT (SUB-CATEGORY & REGION-WISE) ',width = 1200,height=500,barmode= 'group',facet_col = 'Region',facet_col_spacing =0)

frame2.show()

# Results 1-2-3
so the EAST_FURNITURES_TABLES are the worst hit..
furnitures are being hit bad because of the tables. 

and then

Central furnitures FURNISHINGS
As Furniture is being affected by Furnishings as well so it is taking the Central down...
Although in Central almost others are negative too..
so we need to put an effort in  all subcategories Central have except chairs..
____________________________________________________________________________________________________________________________

#calculating state wise and determining the Sales and the Profits...

statewise = df.groupby(['State'],as_index=False)['Profit','Sales'].sum()

statewise['Cost']= (statewise.iloc[:,2]-statewise.iloc[:,1])

statewise['Percentage']= (statewise.iloc[:,1]/statewise.iloc[:,3])*100

qq = statewise.sort_values(by= 'Profit', ascending = 0)

qq

qq.iplot(kind= 'bar', x = ["State"], y =['Sales'] ,title = "SALES STATE WISE", xTitle = "STATES", yTitle= "Sales", bargap =0.2, color = ["forestgreen", "steelblue"],dimensions = (1200,500))

qq.iplot(kind= 'bar', x = ["State"], y =['Profit'] ,title = "PROFIT STATE WISE", xTitle = "STATES", yTitle= "Profit", bargap =0.2, color = ["steelblue"],dimensions = (1200,500))

qq.iplot(kind= 'bar', x = ["State"], y =['Percentage'] ,title = "PROFIT PERCENTAGE STATE WISE", xTitle = "STATES", yTitle= "Profit Percentage on Sales", bargap =0.2, color = ["gold", "steelblue"],dimensions = (1200,500))




statewise1 = df.groupby(['State','Sub-Category'],as_index=False)['Profit','Sales'].sum()

statewise1['Cost']= (statewise1.iloc[:,3]-statewise1.iloc[:,2])

statewise1['Percentage']= (statewise1.iloc[:,2]/statewise1.iloc[:,4])*100


statewise1.iplot(kind= 'bar', x = ['State','Sub-Category'], y =['Profit','Sales'] ,title = "STATE WISE DISTRIBUTION OF Profit & Sales", xTitle = "STATES", bargap =0.2, color = ['darkorange', "steelblue"], dimensions = (1200,500))



statewise2 = df.groupby(['Sub-Category','State'],as_index=False)['Profit','Sales'].sum()

statewise2['Cost']= (statewise2.iloc[:,3]-statewise2.iloc[:,2])

statewise2['Percentage']= (statewise2.iloc[:,2]/statewise2.iloc[:,4])*100

statewise2.iplot(kind= 'bar', x = ['Sub-Category','State'], y =['Profit','Sales'] ,title = "SUB CATEGORY WISE distribution of PROFIT AND SALES", xTitle = "STATES", bargap =0.2, color = ['darkorange', "steelblue"], dimensions = (1200,500))



statewise1.iplot(kind= 'bar', x = ['State','Sub-Category'], y =['Percentage'] ,title = "PROFITABILITY OF STATES WITH RESPECT TO SUB CATEGORIES", xTitle = "STATES", yTitle= "Profit Percentage on Sales", bargap =0.2, color = ['darkorange', "steelblue"], dimensions = (1200,500))


statewise2.iplot(kind= 'bar', x = ['Sub-Category','State'], y =['Percentage'] ,title = "PROFITABILITY OF SUBCATEGORIES WITH RESPECT TO STATES", xTitle = "STATES", yTitle= "Profit Percentage on Sales", bargap =0.2, color = ["steelblue",'red'], dimensions = (1200,500))


# Analyzing the Discounts:
d = df[['Discount']]

d.value_counts()

##trying to find a solid proof
dp = df[["Discount", "Profit"]]

print(dp.corr())

# not good
sns.lmplot(x = 'Discount', y = "Profit", data = df);


ds =df[["Discount", "Sales"]]

print(ds.corr())

#not good either

sns.lmplot(x='Discount', y ="Sales" ,data =df);

dq = df[['Discount', "Quantity"]]


print(dq.corr())

sns.lmplot(x = 'Discount', y = "Quantity", data = df);

# lets see who has got the most discounts that is more than 30%

highdiscount = df[(df['Discount'] >=0.30)]

sns.countplot(x = highdiscount['Region']);

RESULTS of the Discount Analysis

 As we found no Correlation of discount with any of the Profit, sales or Quantity over all,,
 but we can see that most discounts were given in the  Central region
 
 
centralonly= df[(df['Region']=='Central')]

centralonly[['Discount']].describe()

central_ds = centralonly[['Discount','Sales']]

print(central_ds.corr())

sns.lmplot(x = 'Discount', y = "Sales", data = central_ds);

central_dp = centralonly[['Discount', "Profit"]]

print(central_dp.corr())

sns.lmplot(x = 'Discount', y = "Profit", data = central_dp);


# Checkingm which Category is giving the most loss.

cent= centralonly.groupby(['Category'],as_index=False)['Profit','Sales'].sum()

cent


## Digging Deep in to Category "Furniture" of CENTRAL Specifically


central_fur = df[(df['Region']=="Central") & (df["Category"] == "Furniture")]

central_fur_ds = central_fur[['Discount','Quantity']]

print(central_fur_ds.corr())

sns.lmplot(x = 'Discount', y = "Quantity", data = central_fur_ds);



central_fur_ds = central_fur[['Discount','Sales']]

print(central_fur_ds.corr())

sns.lmplot(x = 'Discount', y = "Sales", data = central_fur_ds);



central_fur_dp = central_fur[['Discount',"Profit"]]


print(central_fur_dp.corr())

sns.lmplot(x = 'Discount', y = "Profit", data = central_fur_dp);




# so we can conclude that ..
in Central Region , Furniture Category, 
discounts given had dragged the profits down and 
also if discounts were given on a purpose that sales will boost up, 
it also didnt work.
even by giving the discounts, sales were not fetched also , profits had to pay the price.


Also we have checked many sub categories that have been giving losses in a particular state but giving profit in other states so, 
these subcategories are needed to be analyzed deeply , whether they are being sold with wrong technique or the place is just not right to sell those particular sub category.
the solution can be Dynamic as per the many other factors of marketing of various products.


