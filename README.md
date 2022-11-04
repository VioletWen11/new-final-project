import imp
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
plt.style.use('seaborn')


st.title('Roller Coasters Around the World')
df1 = pd.read_csv('roller_coasters.csv')
df2 = pd.read_csv('Golden_Ticket_Award_Winners_Steel.csv')
df3 = pd.read_csv('Golden_Ticket_Award_Winners_Steel.csv')


st.subheader('Steel Roller Coasters who won the Golden Ticket Award')
# points filter
points_filter = st.slider('The minimal points:', 0, 1400, 59)
df2 = df2[df2.Points >= points_filter]

st.subheader('Roller Collars and Their Points')

# create a input form
form = st.sidebar.form("park_form")
park_filter = form.text_input('Park Name (enter ALL to reset)', 'ALL')
form.form_submit_button("Apply")
if park_filter!='ALL':
    df2 = df2[df2.Park == park_filter]


genre = st.sidebar.radio(
    "Choose point level",
    ('Low 0~200', 'Medium 200~550', 'High 550~', 'All'))

if genre == 'Low 0~200':
    df2 = df2[df2.Points <= 200]
elif genre == 'Medium 200~550':
    df2 = df2[df2.Points > 200]
    df2 = df2[df2.Points < 550]
elif genre == 'High 550~':
    df2 = df2[df2.Points > 550]
elif genre == 'All':
    df2 == df2

df4 = df2[['Name', 'Park', 'Supplier', 'Points']]
fig, ax = plt.subplots(figsize=(20, 10))
x = df4.groupby('Name')['Points'].mean()
x.plot.bar(ax = ax)
st.pyplot(fig)

# park 与 评分
st.subheader('Parks and their Points')
fig, ax=plt.subplots()
df_park = df2[['Park', 'Points']].groupby('Park').mean()
df_park.sort_values('Points', ascending=False).plot.bar(ax=ax)
st.pyplot(fig)

# 生产年份与评分关系
st.subheader('The Built Year of the Roller Coaster and the Points They Got')
fig, ax=plt.subplots()
x = df2[['Year Built', 'Points']].groupby('Year Built').mean()
x.plot(ax=ax)
st.pyplot(fig)

# supplier 与 评分
st.subheader('Suppliers and Their Points')
fig, ax=plt.subplots()
df_park = df2[['Supplier', 'Points']].groupby('Supplier').mean()
df_park.sort_values('Points', ascending=False).plot.bar(ax=ax)
st.pyplot(fig)

# supplier 所拥有过山车的数量
st.subheader('The Percentage of Roller Coasters Owned by the Suppliers in the Market')
fig, ax=plt.subplots(figsize=(8, 5))
x = np.array(df3.Supplier.value_counts()/len(df3.Supplier))#用一维数组存入各个饼块的尺寸。
plt.pie(x, labels= ['B&M', 'Intamin', 'RMC', 'Schwarzkopf', 'Rocky Mountain', 'Mack', 'Arrow', 'Morgan', 'Lagan', 'Vekoma', 'Schwarz', 'Chance', 'Premier', 'Morgan/Arrow', 'Zierer'])#绘制饼状图，默认是从x轴正方向逆时针开始绘图
plt.show()#显示饼状图
st.pyplot(fig)

