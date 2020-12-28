---
layout: post
title: Zipline trading environment
excerpt_separator: <!--more-->
---
{% if page.title == "Home" %}
  ![zipline+quandl+pyfolio](../images/zipline_pyfolio.jpg)
{% else %}
  ![zipline+quandl+pyfolio](/images/zipline_pyfolio.jpg)
{% endif %}


```python
conda create -n env_zipline python=3.5
```


```python
conda install -c conda-forge zipline
```


```python
conda install -c conda-forge pyfolio
```


```python
zipline ingest -b quantopian-quandl
```


```python
conda install -c quantopian ta-lib
```


```python
conda install mkl-service
```
