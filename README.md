# Analiza-danych-historycznych

import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import warnings

def make_graph(stock_data, revenue_data, stock):
    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing = .3)
    fig.add_trace(go.Scatter(x=stock_data.Date, y=stock_data.Close, name="Share Price"), row=1, col=1)
    fig.add_trace(go.Scatter(x=revenue_data.Date, y=revenue_data.Revenue, name="Revenue"), row=2, col=1)
    fig.update_xaxes(title_text="Date", row=1, col=1)
    fig.update_xaxes(title_text="Date", row=2, col=1)
    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)
    fig.update_layout(showlegend=False,
    height=900,
    title=stock,
    xaxis_rangeslider_visible=True)
    fig.show()

warnings.filterwarnings("ignore", category=FutureWarning)

tesla_stock = yf.Ticker('TSLA')
tesla_data = tesla_stock.history(period='max')
tesla_data.reset_index(inplace=True)
tesla_data.head(5)

url = 'https://companiesmarketcap.com/tesla/revenue/'
tesla_revenue_data  = requests.get(url).text

soup = BeautifulSoup(tesla_revenue_data, 'html.parser')

tesla_revenue = pd.DataFrame(columns=["Date", "Revenue"])

for row in soup.find("tbody").find_all('tr'):
    col = row.find_all("td")
    date = col[0].text
    revenue = col[1].text

    tesla_revenue = pd.concat([tesla_revenue, pd.DataFrame({"Date":[date], "Revenue":[revenue]})], ignore_index=True)

tesla_revenue.tail(5)

gme_stock = yf.Ticker('TSLA')
gme_data = gme_stock.history(period='max')
gme_data.reset_index(inplace=True)
gme_data.head(5)

url = 'https://companiesmarketcap.com/eur/gamestop/revenue/'
gme_revenue_data  = requests.get(url).text

soup = BeautifulSoup(gme_revenue_data, 'html.parser')

gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])

for row in soup.find("tbody").find_all('tr'):
    col = row.find_all("td")
    date = col[0].text
    revenue = col[1].text

    gme_revenue = pd.concat([gme_revenue, pd.DataFrame({"Date":[date], "Revenue":[revenue]})], ignore_index=True)

gme_revenue.tail(5)

make_graph(tesla_data, tesla_revenue, 'Tesla')

make_graph(gme_data, gme_revenue, 'GameStop')
