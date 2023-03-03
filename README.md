# Easy Elasticsearch

Original project can be found here: https://github.com/kwang2049/easy-elasticsearch
This repository contains a high-level encapsulation for using [Elasticsearch](https://www.elastic.co/downloads/elasticsearch) with python in just a few lines.

## Installation
Via pip:
```bash
pip install easy-elasticsearch
```
Via git repo:
```bash
git clone https://github.com/sirCamp/easy-elasticsearch
pip install -e . 
```

## Usage
To utilize the elasticsearch service, one can select from 3 ways:
- (1) Start an ES service manually and then indicate the `host` and `port_http` (please refere to [download_and_run.sh](easy_elasticsearch/examples/download_and_run.sh)); 
- (2) Or leave `host=None` by default to start a docker container itself;
- (3) Or leava `host=None` and setting `service_type=executable` to download an ES executable and start it in the back end.

Finally, just either call its ```rank``` or ```score``` function for retrieval or calculating BM25 scores.
```python
from easy_elasticsearch import ElasticSearchBM25

corpus = [
    {'id': 1, 'title' : 'what are cats', 'text':'cats are ....'},
    {'id': 2, 'title' : 'what is python', 'text':'python is ....'},
    {'id': 3, 'title' : 'the meaning of life', 'text':'42 is the mneaning of ....'},
]
bm25 = ElasticSearchBM25(index_name='new_index', port_http='9222', port_tcp='9333')  # By default, when `host=None` and `mode="docker"`, a ES docker container will be started at localhost.

bm25._add_corpus(corpus=corpus, index_name='new_index')

query = "What is Python?"
rank = bm25.query(query, topk=10)  # topk should be <= 10000

print(query, rank)
```