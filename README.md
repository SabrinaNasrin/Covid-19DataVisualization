# Data Visualization Project on Covid-19 Canada

### MM802- Visualization Mini-project
#### Date: March 19, 2022

This project is done as a part of MM-802 course.

##### Files:
* [app.py](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/app.py)
* [data](https://github.com/SabrinaNasrin/Covid-19DataVisualization/tree/main/Data)
* [giphy-downsized.gif](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/giphy-downsized.gif)


##### IDE:
* Visual Studio Code 1.65.2.0

##### Language:
* Python 3.9

##### Dependencies:
Install the following libraries in the system:
* Datetime
* Matplotlib.pyplot
* Streamlit
* Pandas
* Plotly
* Plotly Express
* Streamlit_option_menu
* Numpy


##### How to run the code:
* Download the repository
* Open Visual Studio Code
* Keep all the files in the repository in the same folder
* Install all the dependencies
* Open "Terminal" of VSCode
* In the Terminal, write `streamlit run app.py`, press Enter
* It will open the website in the browser
* For Mac users change the following code in app.py in the specified line as mentioned:
```
# On line 16 replace the code with this line: 
df = pd.read_csv("data/cases_timeseries_canada.csv")

# On line 20 replace the code with this line:
province_data = pd.read_csv("data/cases_timeseries_prov.csv")

# On line 253, replace the code with this line:
ageData = pd.read_csv("data/datasetage.csv")
```

##### Description of Code:
###### Importing Libraries
```
import datetime
from email.policy import default
from tkinter import Label
from turtle import position, width
from matplotlib import container, pyplot
import streamlit as st
import pandas as pd
import plotly
import matplotlib.pyplot as plt
from streamlit_option_menu import option_menu
from numpy import isin
import plotly.express as px
```
###### Radio Button Styling
This line is to make the orientation of the radio buttons in horizontal direction
```
st.write(
    '<style>div.row-widget.stRadio > div{flex-direction:row;justify-content: space-evenly;}</style>', unsafe_allow_html=True)
```
###### Reading Datasets
We have used two datasets for cases, mortality, recovered and testing. They are- [Canada_Timeseries](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Data/cases_timeseries_canada.csv) and [Provincewise_Data](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Data/cases_timeseries_prov.csv)
`df.columns` and `province_data.columns` are for assigning the columns of the dataset.
`pd.to_datetime` is used to convert the given argument to datetime and `dt.strftime` convert to index using specified date_format '%Y-%m-%d'.
```
df = pd.read_csv("Data\cases_timeseries_canada.csv")
df.columns = ["Country", "Date", "DailyCasesCountry", "CumulativeDailyCases_country", "DailyDeathsCountry",
              "CumulativeDailyDeaths", "DailyRecoveredCountry", "CumulativeDailyRecoveredCountry", "DailyTestingCountry", "CumulativeDailyTestingCountry"]
df['Date'] = pd.to_datetime(df['Date']).dt.strftime('%Y-%m-%d')
province_data = pd.read_csv("Data\cases_timeseries_prov.csv")
province_data.columns = ["Country", "prov_code", "Province", "Date", "Daily_Cases", "Cumulative_Cases", "Daily_Deaths",
                         "Cumulative_Deaths", "Daily_Recovered", "Cumulative_Recovered", "Daily_Testing", "Cumulative_Testing"]
province_data['Date'] = pd.to_datetime(
    province_data['Date']).dt.strftime('%Y-%m-%d')
```
###### Sidebar
This is the code for creating and styling the sidebar.
```
with st.sidebar:
    selected = option_menu(
        menu_title=None,
        options=["Home", "Cases", "Mortality",
                 "Recovered", "Testing", "Age wise Data"],
        icons=['card-list', 'clipboard-plus', "heart pulse",
               'file-earmark-plus', "eyedropper", 'people'],
        default_index=0,
        styles={"container": {"padding": "0!important", "width": "100%", "background-color": "#eee", "margin": "0"},
                "icon": {"color": "purple", "font-size": "20px"},
                "nav-link": {"font-size": "15px", "text-align": "left-align", "margin": "0px", "--hover-color": "white"},
                "nav-link-selected": {"background-color": "black", "text-color": "white"}, }
    )
  ```
##### Homepage
If "Home" is selected then user will go to the homepage.
```
if selected == "Home":
    st.markdown("""
    # Covid-19 in Canada
    Coronavirus disease 2019 (COVID-19) is a highly contagious viral illness caused by severe acute respiratory syndrome SARS-CoV-2. It has had a devastating effect on the world’s demographics resulting in more than 5.3 million deaths worldwide. It has emerged as the most consequential global health crisis since the era of the influenza pandemic of 1918. This dashboard provides summary of COVID-19 cases across Canada and over time. It Contains detailed data about the spread of the virus over time and in different regions of the country.  It also includes breakdowns by age and sex or gender and provides an overview of deaths, testing and recoveries.
    """)
    st.image("giphy-downsized.gif", width=700)
```
##### Cases
This is the code for Cases page. `st.radio` is for radio buttons. Here, `st.date_input` is used to select date picker. In the cumulative report option, an `animation frame` and `animation group` is used to show the animation. `px.bar` and `px.pie` is for bar chart and pie chart.
```
# Cases
if selected == "Cases":
    st.markdown("""
    # Daily Reported Cases in Canada
    """)

    daily = st.radio("", options=["Daily Report",
                     "Cumulative Report", "Pie Chart"])

    if daily == "Daily Report":
        st.markdown("""
    ##### Daily Report of Covid Cases
    ###### Date Range
    """)
        a = st.date_input("From", datetime.date(2020, 1, 25), min_value=(
            datetime.date(2020, 1, 25)), max_value=(datetime.date.today()))
        b = st.date_input("to", min_value=(datetime.date(
            2020, 1, 25)), max_value=(datetime.date.today()))
        fig = px.bar(df, x='Date', y="DailyCasesCountry", range_y=[
            0, 75000], range_x=[a, b], labels={'DailyCasesCountry': 'Daily Cases'})
        fig.update_layout(width=700)
        st.write(fig)
    if daily == "Cumulative Report":
        st.markdown(
            '##### Cumulative Report of Covid Cases in Canada by Province')
        fig1 = px.bar(province_data, x='Cumulative_Cases', y="Province", color="Province", orientation="h", range_x=[
            0, 1200000], animation_frame='Date', animation_group="Province")
        fig1.layout.updatemenus[0].buttons[0].args[1]['frame']['duration'] = 20
        fig1.layout.updatemenus[0].buttons[0].args[1]['transition']['duration'] = 5
        fig1.update_layout(width=700)
        st.write(fig1)
    if daily == "Pie Chart":
        st.markdown("""
        ##### Percentage of Covid Cases in Canada by Province
        """)
        fig4 = px.pie(province_data, values="Cumulative_Cases",
                      names="Province", color_discrete_sequence=px.colors.sequential.Plasma_r)
        st.write(fig4)
```
##### Mortality
Here, these lines are for making multiselect option for provinces.
```
 province_options = province_data['Province'].unique().tolist()
        province_select = st.multiselect(
            'Select Province', province_options, 'Alberta')
        province_data = province_data[province_data['Province'].isin(
            province_select)]
```
This is the code for morality page. In this page, a range slider is used to change the dates.
```
# Mortality
if selected == "Mortality":
    st.title("Daily Reported Deaths in Canada")
    st.markdown('##### Which Report You Want to See?')
    rad = st.radio("", ["Daily Death Report",
                   "Cumulative Reported Death", "Pie Chart"])
    if rad == "Daily Death Report":
        st.markdown('###### Date Range')
        c = st.date_input("From", datetime.date(2020, 1, 25), min_value=(
            datetime.date(2020, 1, 25)), max_value=(datetime.date.today()))
        d = st.date_input("To", min_value=(datetime.date(
            2020, 1, 25)), max_value=(datetime.date.today()))
        st.markdown('##### Daily Death Reports in Canada')
        fig2 = px.bar(df, x='Date', y="DailyDeathsCountry", range_y=[
            0, 280], range_x=[c, d], labels={'DailyDeathsCountry': 'Daily Deaths by Country'})
        fig2.update_layout(width=700)
        st.write(fig2)
        st.markdown('##### Daily Death Reports in Canada by Province')
        province_options = province_data['Province'].unique().tolist()
        province_select = st.multiselect(
            'Select Province', province_options, 'Alberta')
        province_data = province_data[province_data['Province'].isin(
            province_select)]
        fig3 = px.line(province_data, x='Date', y="Daily_Deaths", color="Province", range_y=[
            0, 210], range_x=[c, d], labels={'Daily_Deaths': 'Daily Deaths by Province'})
        fig3.update_layout(width=700)
        st.write(fig3)
    if rad == "Cumulative Reported Death":
        st.markdown('##### Cumulative Reported Death in Canada by Province')
        fig = px.line(province_data, x='Date',
                      y="Cumulative_Deaths", color="Province", labels={'Cumulative_Deaths': 'Cumulative Deaths by Province'})

        # Add range slider
        fig.update_layout(
            xaxis=dict(
                rangeselector=dict(
                    buttons=list([
                        dict(count=1,
                             label="1 month",
                             step="month",
                             stepmode="backward"),
                        dict(count=3,
                             label="3 month",
                             step="month",
                             stepmode="backward"),
                        dict(count=6,
                             label="6 month",
                             step="month",
                             stepmode="backward"),
                        dict(count=1,
                             label="Year to date",
                             step="year",
                             stepmode="todate"),
                        dict(count=1,
                             label="1 year",
                             step="year",
                             stepmode="backward"),
                        dict(step="all")
                    ])
                ),
                rangeslider=dict(
                    visible=True
                ),
                type="date"
            )
        )

        st.write(fig)

    if rad == "Pie Chart":
        st.markdown(
            '##### Percentage of Reported Deaths in Canada by Province')
        fig3 = px.pie(province_data, values="Cumulative_Deaths",
                      names="Province", color_discrete_sequence=px.colors.sequential.Plasma_r)
        st.write(fig3)
```
##### Recovered
This is the code for recovered page. `st.selectbox` is used to select among other options. `px.line` is for the line chart.
```
# Recovered
if selected == "Recovered":
    st.title("Daily Recovered Report in Canada")
    st.markdown('##### Which Report You Want to See?')
    rad = st.selectbox("", [
                       "Daily Recovered", "Cumulative Recovered"])
    if rad == "Daily Recovered":
        st.markdown('##### Daily Recovered Report in Canada')
        st.markdown('###### Date Range')
        e = st.date_input("From", datetime.date(2020, 1, 25), min_value=(
            datetime.date(2020, 1, 25)), max_value=(datetime.date.today()))
        f = st.date_input("To", min_value=(datetime.date(
            2020, 1, 25)), max_value=(datetime.date.today()))
        fig2 = px.bar(df, x='Date', y="DailyRecoveredCountry", range_y=[
            0, 60000], range_x=[e, f], labels={'DailyRecoveredCountry': 'Daily Recovered'})
        fig2.update_layout(width=800)
        st.write(fig2)
        st.markdown('##### Daily Recovered Report in Canada by Province')
        province_options = province_data['Province'].unique().tolist()
        province_select = st.multiselect(
            'Select Province', province_options, 'Alberta')
        province_data = province_data[province_data['Province'].isin(
            province_select)]
        fig3 = px.line(province_data, x='Date', y="Daily_Recovered", color="Province", range_y=[
            0, 35000], range_x=[e, f], labels={'Daily_Recovered': 'Daily Recovered by Province'})
        fig3.update_layout(width=800)
        st.write(fig3)
    if rad == "Cumulative Recovered":
        st.markdown('##### Cumulative Recovered Report in Canada by Province')
        st.markdown('###### Date Range')
        g = st.date_input("From", datetime.date(2020, 1, 25), min_value=(
            datetime.date(2020, 1, 25)), max_value=(datetime.date.today()))
        h = st.date_input("To", min_value=(datetime.date(
            2020, 1, 25)), max_value=(datetime.date.today()))
        fig2 = px.line(province_data, x='Date', y="Cumulative_Recovered", color="Province", range_y=[
            0, 1110000], range_x=[g, h], labels={'Cumulative_Recovered': 'Cumulative Recovered by Province'})
        fig2.update_layout(width=800)
        st.write(fig2)

```
##### Testing 
This is the code for testing page.
```
# Testing
if selected == "Testing":
    st.title('Daily Testing Report in Canada')
    st.markdown('##### Which Report You Want to See?')
    rad = st.radio("", [
        "Daily Testing", "Cumulative Testing"])
    if rad == "Daily Testing":
        st.markdown('##### Daily Testing Report in Canada')
        st.markdown('###### Date Range')
        i = st.date_input("From", datetime.date(2020, 1, 25), min_value=(
            datetime.date(2020, 1, 25)), max_value=(datetime.date.today()))
        j = st.date_input("To", min_value=(datetime.date(
            2020, 1, 25)), max_value=(datetime.date.today()))
        fig2 = px.bar(df, x='Date', y="DailyTestingCountry", range_y=[
            0, 200000], range_x=[i, j], labels={'DailyTestingCountry': 'Daily Testing in Canada'})
        fig2.update_layout(width=800)
        st.write(fig2)
        st.markdown('##### Daily Testing Report in Canada by Province')
        province_options = province_data['Province'].unique().tolist()
        province_select = st.multiselect(
            'Select Province', province_options, 'Alberta')
        province_data = province_data[province_data['Province'].isin(
            province_select)]
        fig3 = px.bar(province_data, x='Date', y="Daily_Testing", color="Province", range_y=[
            0, 150000], range_x=[i, j], labels={'Daily_Testing': 'Daily Testing by Province'})
        fig3.update_layout(width=800)
        st.write(fig3)
    if rad == "Cumulative Testing":
        st.markdown('##### Cumulative Testing Report in Canada by Province')
        province_options = province_data['Province'].unique().tolist()
        st.markdown('###### Date Range')
        k = st.date_input("From", datetime.date(2020, 1, 25), min_value=(
            datetime.date(2020, 1, 25)), max_value=(datetime.date.today()))
        l = st.date_input("To", min_value=(datetime.date(
            2020, 1, 25)), max_value=(datetime.date.today()))
        fig2 = px.line(province_data, x='Date', y="Cumulative_Testing", color="Province", range_y=[
            0, 25000000], range_x=[k, l], labels={'Cumulative_Testing': 'Cumulative Testing Report'})
        fig2.update_layout(width=800)
        st.write(fig2)
```
##### Age wise Data
This is the code for age wise data page. For this we have used [Age Dataset](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Data/datasetage.csv).
```
# Age Wise Data
if selected == "Age wise Data":
    st.markdown("""
    #  Age Distribution of COVID-19 Cases in Canada
    We have detailed case report data from 3,284,725 cases. We know the age of patients in 99.96% of cases, and both age and gender in 99.70% of cases Of the cases reported in Canada so far, 52.7% were female and 36.7% were between 20 and 39 years old.
    """)
    ageData = pd.read_csv("Data\datasetage.csv")

    ageData.columns = ["Age group (years)", "Number of cases with case reports", "Percentage", "Number of male cases", "Number of female cases"
                       ]
    st.markdown('##### Age Distribution by Number')
    rad = st.radio("", ["Age", "Age by Gender"])
    if rad == "Age":
        fig2 = px.bar(
            ageData, x='Number of cases with case reports', y="Age group (years)", color="Age group (years)", color_discrete_sequence=px.colors.sequential.RdBu, orientation="h", range_x=[0, 700000], labels={'Number of cases with case reports': 'Number'})
        fig2.update_layout(width=800)
        st.write(fig2)
        st.markdown('##### Age Distribution by Percentage')
        fig3 = px.pie(ageData, values="Number of cases with case reports",
                      names="Age group (years)", color_discrete_sequence=px.colors.sequential.RdBu)
        st.write(fig3)
    if rad == "Age by Gender":
        fig2 = px.bar(
            ageData, x=["Number of male cases", "Number of female cases"], y="Age group (years)", orientation="h", color_discrete_sequence=px.colors.sequential.RdBu, range_x=[0, 700000], labels={'value': 'Number'})
        fig2.update_layout(width=800)
        st.write(fig2)
        st.markdown('##### Age Distribution by Percentage by Gender')
        gender = st.radio("", options=["Male", "Female"])
        if gender == "Male":
            fig3 = px.pie(ageData, values="Number of male cases",
                          names="Age group (years)", color_discrete_sequence=px.colors.sequential.RdBu)
            st.write(fig3)
        if gender == "Female":
            fig4 = px.pie(ageData, values="Number of female cases",
                          names="Age group (years)", color_discrete_sequence=px.colors.sequential.Plasma_r)
            st.write(fig4)
```
##### How to use the website:
* The pages of this website include:
     * Homepage
     * Cases
     * Mortality
     * Recovered
     * Testing
     * Age wise Data
##### Homepage:
This is the homepage of our website. In the homepage, there is a sidebar with navigation menu.
![Homepage](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/home.png)
##### Cases:
If you click on the Daily report button, it will show the daily report of the covid cases on that particular date range.
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases1.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases2.png)
If you click on the Cumulative report, it will show an animation of consolidate confimed cases of all the provinces on a particular date range.
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases3.png)
If you click on the Pie chart, it will show province wise confirmed cases in percentage.
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases4.png)

###### Mortality
If you click on the Cumulative report, it will show a consolidate death report of all the provinces. There is a range slider to change the date range.
![Mortality](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/mortality2.png)
If you click on the Pie chart, it will show province wise death reports in percentage.
![Mortality](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/Mortality3.png)

###### Recovered
In this section, there is a select box, where you can either fetch the daily recovered reports by province or Canada or cumulative recovered report by province.
![Recovered](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/recovered1.png)
![Recovered](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/recovered2.png)

###### Testing
In this section, there is a daily report of Canada and provinces by using multiselect and cumulative report of all the provinces regarding the recovered cases.
![Testing](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/Testing1.png)
![Testing](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/testing2.png)

###### Ages:
In this section, you can fetch age wise report either by number or by gender which is further divided into the percentage by showing in the form of pie chart. 
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age1.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age2.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age3.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age4.png)
