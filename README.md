# Pandas_HeroesOfPymoli
Heroes of Pymoli Data Analysis

Males makeup 81% of all players and 82% of revenues
Females makeup of 17% of all players and 16% of revenues
The majority of players are in the 20-24 age range.
THe most popular item is not the highes generating revenue item.
# Dependencies 
import pandas as pd
import os
import json

# load JSON
purchase_raw_data = []
with open('purchase_data.json') as data_file:
  purchase_raw_data = json.load(data_file)
purchase_raw_data

purchase_data = pd.read_json("purchase_data.json") #orient = index
#purchase_data.reset_index(level=0, inplace=True)

purchase_data

#Player Count
total_players = len(purchase_raw_data)
total_players

#Purchasing Analysis (total)
total_purchased = purchase_data["Price"].sum()
print(total_purchased)

#Gender Demographics
total_players = purchase_data["Gender"].count()

#count of players
Count_PlayersbyGen = pd.DataFrame(purchase_data.groupby('Gender')['Gender'].count())
Count_PlayersbyGen['% of Players'] = Count_PlayersbyGen['Gender']/total_players * 100
Count_PlayersbyGen.rename(columns = {'Gender':'Number of Players'},inplace=True)
Count_PlayersbyGen

#Purchasing Analysis by Gender
#Purchase count
Count_PurchbyGen = pd.DataFrame(purchase_data.groupby('Gender')['Gender'].count())
#total purchase value
Sales_PurchbyGen = pd.DataFrame(purchase_data.groupby('Gender')['Price'].sum())
PurchbyGen = pd.merge(Count_PurchbyGen, Sales_PurchbyGen, left_index = True, right_index = True)
PurchbyGen.rename(columns = {'Gender':'Number of Players','Price':'Total Purchase Value'},inplace=True)
#average purchase price
PurchbyGen['Average Pruchase Price']= PurchbyGen['Total Purchase Value']/ PurchbyGen['Number of Players']
#Avg_purchbyGen
#normalized totals
PurchbyGen

#Age Demographics into bins of 4 years (<10, 10-14, 15-19 etc)
bins = [0, 10, 14, 19, 24,29,100]
group_names = ['<10','10-14','15-19','20-24','25-29','30+']
groups = purchase_data.groupby(['Price',pd.cut(purchase_data['Age'],bins, labels=group_names)])
groups.size().unstack()                               
#Purchase count
#average purchase price
#total purchase value
#normalized totals

#Top Spenders
#SN
#Purchase count
Count_PurchbySN = pd.DataFrame(purchase_data.groupby('SN')['SN'].count())
#total purchase value
Sales_PurchbySN = pd.DataFrame(purchase_data.groupby('SN')['Price'].sum())

PurchbySN = pd.merge(Count_PurchbySN, Sales_PurchbySN, left_index = True, right_index = True)
PurchbySN.rename(columns = {'SN':'Number of Purchases','Price':'Total Purchase Value'},inplace=True)
#average purchase price
PurchbySN.sort_values('Total Purchase Value', ascending=False).head(10)

#Most Popular Items in a table by Item ID
#items_grouped = purchase_data.groupby('Item ID').agg({'SN': 'count','Price':'sum'})

#purchase count
Count_PurchbyID = pd.DataFrame(purchase_data.groupby('Item ID')['Item ID'].count())

#item Price
#total purchase value
Sales_PurchbyID = pd.DataFrame(purchase_data.groupby('Item ID')['Price'].sum())

PurchbyID = pd.merge(Count_PurchbyID, Sales_PurchbyID, left_index = True, right_index = True)
PurchbyID.rename(columns = {'Item ID':'Number of Purchases','Price':'Total Purchase Value'},inplace=True)
PurchbyID.sort_values('Number of Purchases', ascending=False).head(10)

#Most Profitable Items in a table by Item ID

#purchase count
Count_PurchbyID = pd.DataFrame(purchase_data.groupby('Item ID')['Item ID'].count())
#Total Purchase Value
Sales_PurchbyID = pd.DataFrame(purchase_data.groupby('Item ID')['Price'].sum())

PurchbyID = pd.merge(Count_PurchbyID, Sales_PurchbyID, left_index = True, right_index = True)
PurchbyID.rename(columns = {'Item ID':'Number of Purchases','Price':'Total Purchase Value'},inplace=True)
#Item Name

#Item Price

PurchbyID.sort_values('Total Purchase Value', ascending=False).head(10)
