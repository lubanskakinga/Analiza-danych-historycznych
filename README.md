# Analiza-danych-historycznych

```
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
```

```
tesla_stock = yf.Ticker('TSLA')
tesla_data = tesla_stock.history(period='max')
tesla_data.reset_index(inplace=True)
tesla_data.head(5)
```

![teslastock yfinance](https://github.com/user-attachments/assets/f65c8156-b70d-40f5-ab69-0055db90a6f9)

```
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
```
![tesla-revenue](https://github.com/user-attachments/assets/b0a7ef3c-7afe-4f4d-a446-e2c6f53af32a)

```
gme_stock = yf.Ticker('TSLA')
gme_data = gme_stock.history(period='max')
gme_data.reset_index(inplace=True)
gme_data.head(5)
```
![zad3](https://github.com/user-attachments/assets/c764d4c7-6db9-4c46-8030-34c69578c3f0)

```
url = 'https://companiesmarketcap.com/eur/gamestop/revenue/'
gme_revenue_data  = requests.get(url).text

soup = BeautifulSoup(gme_revenue_data, 'html.parser')

gme_revenue = pd.DataFrame(columns=["Date", "Revenue"])

for row in soup.find("tbody").find_all('tr'):
    col = row.find_all("td")
    date = col[0].text
    revenue = col[1].text

    gme_revenue = pd.concat([gme_revenue, pd.DataFrame({"Date":[date], "Revenue":[revenue]})], ignore_index=True)```

gme_revenue.tail(5)
```
![gme-revenue](https://github.com/user-attachments/assets/b01b67e9-c48f-42f0-8dd4-e813876f7c92)

```
make_graph(tesla_data, tesla_revenue, 'Tesla')

make_graph(gme_data, gme_revenue, 'GameStop')
```
![tesla-price](https://github.com/user-attachments/assets/c8b57c04-b292-497d-9229-b067242af89a)

![historia-revenue](https://github.com/user-attachments/assets/1156d807-6ff1-44ba-8f80-a6b882f93112)

![gamestop-price](https://github.com/user-attachments/assets/adc66981-a6e5-47ad-9dcb-2c71ae1c549c)

![historia-revenue2](https://github.com/user-attachments/assets/596b608e-1316-4efb-8a71-df25fe54d156)
