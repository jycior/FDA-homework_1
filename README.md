# FDA-homework_1
# 1. Top 10 Reviewer

import pandas as pd

data = pd.read_csv('Reviews.csv')

data = data[:10000]

top10 = data.groupby('UserId').size().sort_values(ascending=False).head(10)

#get top 10 reviewers

top10

score_mean=[]

profile_name=[]

userid=[]

score_count=[]

for i in range(0,10,1):

    sm = sum(data.loc[data['UserId']==top10.index[i]]['Score'])/top10[i]
		
    pn = data.ix[data.loc[data['UserId']==top10.index[i]]['ProfileName'].index[0]]['ProfileName']
		
    ui = top10.index[i]
		
    sc = int(top10[i])
		
    score_mean.append(sm)
		
    profile_name.append(pn)
		
    userid.append(ui)
		
    score_count.append(sc)
		
DesiredOutput = {'UserId':userid,'ProfileName':profile_name,'Score count':score_count,'Score mean':score_mean}

DO = pd.DataFrame(DesiredOutput)

#print out top 10 revewers' information

DO

# 2.Plot score distribution for the user with the most number of reviews

import numpy as np

import matplotlib.pyplot as mpl

fre_user = data.loc[data['ProfileName'] == 'c2']

x = [list(fre_user['Score']).count(i) for i in range(1,6)]

mpl.bar(range(1,6), x, color=['lightblue', 'orange', 'lightgreen', 'pink', 'purple'])

mpl.show()

# 3. Plot pandas Series DataFrame (Time->Date)
import time

dates = []

for i in data['Time']:

    dates.append(time.strftime('%Y', time.localtime(i)))
    
dates = pd.DataFrame(dates)

group = dates.groupby(0)

mpl.bar(group.size().index, group.size())

mpl.grid()

mpl.show()

# 4.Plot HeatMap using seaborn

import seaborn as sb

ident = data['Id']

HFD = data['HelpfulnessDenominator']

HFN = data['HelpfulnessNumerator']

Time = data['Time']

Score = data['Score']

df = pd.concat([ident,HFD,HFN,Time,Score], axis=1)

correlation = np.corrcoef([df['Id'], df['HelpfulnessNumerator'], df['HelpfulnessDenominator'], df['Score'], df['Time']])

df = pd.DataFrame(correlation)

df.columns = ['Id', 'HelpfulnessNumerator', 'HelpfulnessDenominator', 'Score', 'Time']

df.index = ['Id', 'HelpfulnessNumerator', 'HelpfulnessDenominator', 'Score', 'Time']

sb.heatmap(df, annot=True, vmin=-0.2, vmax=0.6)

# 5.Helpful percent

percent = []

for numerator, denominator in zip(data['HelpfulnessNumerator'], data['HelpfulnessDenominator']):

    if denominator == 0 or numerator == 0: 
		
        percent.append(-1)
				
    elif numerator > denominator: 
		
        pass
				
    else:
		
        percent.append(numerator/denominator)
mpl.hist(percent)

mpl.grid()

mpl.show()
