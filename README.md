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
df = pd.read_csv("data\cases_timeseries_canada.csv")
df.columns = ["Country", "Date", "DailyCasesCountry", "CumulativeDailyCases_country", "DailyDeathsCountry",
              "CumulativeDailyDeaths", "DailyRecoveredCountry", "CumulativeDailyRecoveredCountry", "DailyTestingCountry", "CumulativeDailyTestingCountry"]
df['Date'] = pd.to_datetime(df['Date']).dt.strftime('%Y-%m-%d')
province_data = pd.read_csv("data\cases_timeseries_prov.csv")
province_data.columns = ["Country", "prov_code", "Province", "Date", "Daily_Cases", "Cumulative_Cases", "Daily_Deaths",
                         "Cumulative_Deaths", "Daily_Recovered", "Cumulative_Recovered", "Daily_Testing", "Cumulative_Testing"]
province_data['Date'] = pd.to_datetime(
    province_data['Date']).dt.strftime('%Y-%m-%d')
```
###### Sidebar
This is the code for sidebar. `st.sidebar
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
##### How to use the website:
* The pages of this website include:
     * Homepage
     * Cases
     * Mortality
     * Recovered
     * Testing
     * Age wise Data
###### Homepage:
This is the homepage of our website. In the homepage, there is a sidebar with navigation menu.
![Homepage](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/home.png)
###### Cases:
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases1.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases2.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases3.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/cases4.png)

###### Mortality
![Mortality](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/mortality2.png)
![Mortality](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/Mortality3.png)

###### Recovered
![Recovered](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/recovered1.png)
![Recovered](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/recovered2.png)

###### Testing
![Testing](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/Testing1.png)

###### Ages:
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age1.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age2.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age3.png)
![Cases](https://github.com/SabrinaNasrin/Covid-19DataVisualization/blob/main/Website_ScreenShots/age4.png)
