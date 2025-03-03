### handle datasets

- [get datasets](/bookmarks.md#datasets)
- investigate -> parse -> visualize -> assess data quality -> curate(modify) -> save

### google colab notebook

- use dataset - [amazon reviews 2023 - appliances](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023/blob/main/raw/meta_categories/meta_Appliances.jsonl)

```ipynb
import os
from dotenv import load_dotenv
from huggingface_hub import login
from datasets import load_dataset, Dataset, DatasetDict
from items import Item
from matplotlib.pyplot as plt
```

```ipynb
load_dotenv()
os.environ['OPENAI_API_KEY'] = os.getenv('OPENAI_API_KEY', 'your-key-if-not-using-env')
os.environ['ANTHROPIC_API_KEY'] = os.getenv('ANTHROPIC_API_KEY', 'your-key-if-not-using-env')
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
dataset = load_dataset('McAuley-Lab/Amazon-Reviews-2023', f'raw_meta_Appliances', split='full', trust_remote_code=True)
```

```ipynb
print(f'number of appliances: {len(dataset):,}')
```
