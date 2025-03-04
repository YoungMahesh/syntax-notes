### handle datasets

- [get datasets](/bookmarks.md#datasets)
- investigate -> parse -> visualize -> assess data quality -> curate(modify) -> save

### google colab notebook

- use dataset - [amazon reviews 2023 - appliances](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023/blob/main/raw/meta_categories/meta_Appliances.jsonl)

```ipynb
!pip install dotenv huggingface_hub datasets matplotlib
```

```ipynb
import os
from dotenv import load_dotenv
from huggingface_hub import login
from datasets import load_dataset, Dataset, DatasetDict

# python will first look for local directory for package, hence it will pick up ./items.py file instead of going to package-manager
# 1. mount google-drive
from google.colab import drive
drive.mount('/content/drive')
# 2. mount folder containing items.py
import sys
sys.path.insert(0, '/content/drive/MyDrive/Colab Notebooks/dataset-curation/')
# 3. import items.py file
# use !pip uninstall items, if you alrady installed items-module
from items import Item
# verify if you imported desired class properly
print(dir(Item))

# Import the matplotlib.pyplot module, which provides functions for creating a variety of charts.
import matplotlib.pyplot as plt
```

```ipynb
load_dotenv()

# https://platform.openai.com/settings/organization/billing/overview
# https://platform.openai.com/settings/organization/api-keys
os.environ['OPENAI_API_KEY'] = os.getenv('OPENAI_API_KEY', 'your-key-if-not-using-env')

# https://console.anthropic.com/settings/billing
# https://console.anthropic.com/settings/keys
os.environ['ANTHROPIC_API_KEY'] = os.getenv('ANTHROPIC_API_KEY', 'your-key-if-not-using-env')

# https://huggingface.co/settings/tokens
os.environ['HF_TOKEN'] = os.getenv('HF_TOKEN', 'your-key-if-not-using-env')
```

```ipynb
# log in to HuggingFace
hf_token = os.environ['HF_TOKEN']
login(hf_token, add_to_git_credential=True)
```

```ipynb
# render Matplotlib plots immediately below the code cell that generates them, rather than opening a new window for the plot
%matplotlib inline
```

```ipynb
# load out dataset
# size == ~285 MB
dataset = load_dataset('McAuley-Lab/Amazon-Reviews-2023', f'raw_meta_Appliances', split='full', trust_remote_code=True)
```

```ipynb
print(f'number of appliances: {len(dataset):,}')
# result == 94,327
```

```ipynb
# investiage a particular datapoint
datapoint = dataset[0]
datapoint
```

```ipynb
# Investigate
print(datapoint['title'])
print(datapoint['description'])
print(datapoint['features'])
print(datapoint['details'])
print(datapoint['price'])
```

```ipynb
# how many have prices?
prices = 0
for datapoint in dataset:
  try:
    price = float(datapoint['price'])
    if price > 0:
      prices += 1
  except ValueError as e:
    pass

# :, in prices:, formats number such that there will be commas to make it human readable. e.g. 46,726 instead of 46726
print(f'There are {prices:,} with prices which is {prices/len(dataset)*100:,.1f}%')
```

```ipynb
# for those with prices, gather the price and the length
prices = []
lengths = []
for datapoint in dataset:
  try:
    price = float(datapoint['price'])
    if price > 0:
      prices.append(price)
      contents = datapoint['title'] + str(datapoint['description']) + str(datapoint['features']) + str(datapoint['details'])
      lengths.append(len(contents))
  except ValueError as e:
    pass
```

```ipynb
# plot the distribution of lengths

# Create a new figure with a specified size (15 inches wide, 6 inches tall).
plt.figure(figsize=(15, 6))

# Set the title of the chart. The title includes the average and maximum lengths.
plt.title(f"Lengths: Avg {sum(lengths)/len(lengths):,.0f} and highest {max(lengths):,}\n")

# Set the label for the x-axis, which represents the length in characters.
plt.xlabel('Length (chars)')

# Set the label for the y-axis, which represents the count of occurrences.
plt.ylabel('Count')

# Create a histogram of the lengths. 
# The rwidth parameter sets the relative width of the bars (0.7 means bars will be 70% of the bin width).
# The color parameter sets the color of the bars to light blue.
# The bins parameter specifies the range and step size for the histogram bins (from 0 to 6000 with a step of 100).
plt.hist(lengths, rwidth=0.7, color="lightblue", bins=range(0, 6000, 100))

# Display the chart.
plt.show()
```

```ipynb
# Plot the distribution of prices

plt.figure(figsize=(15, 6))
plt.title(f"Prices: Avg {sum(prices)/len(prices):,.2f} and highest {max(prices):,}\n")
plt.xlabel('Price ($)')
plt.ylabel('Count')
plt.hist(prices, rwidth=0.7, color="orange", bins=range(0, 1000, 10))
plt.show()
```

```ipynb

```

```ipynb

```
