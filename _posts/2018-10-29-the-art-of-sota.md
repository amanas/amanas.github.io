---
layout: post
title: "The art of SoTA"
date: 2018-10-29T00:19:18+02:00
subtitle: "How to write the State of The Art of a research"
author: amanas
sitemap: true
keywords: "Research State Art"
description: "How to write the State of The Art of a research"
meta: ""
---


State of The Art
================

When you start a research, tipically the first step is to review the **"State of The Art"**.

Benefits
--------

-   it demonstrates the novelty of your research results
-   you learn about your research problem
-   it shows different approaches to a solution
-   you can reuse from what others have done

Currently, I am starting to write my MSc. Thesis. The research is about forecasting the flow traffic in the city of Madrid. In order to elaborate the SoTA, I have proceed as follows.

Step 1: search related terms
----------------------------

Choose many search terms and find related publications on [Google scholar](https://scholar.google.es/). The list of terms used was:

-   traffic forecasting
-   traffic forecast model comparison
-   traffic forecasting machine learning
-   traffic forecasting time series models
-   traffic prediction models (filtering from 2014)
-   traffic forecast multivariate (filtering from desde 2014)
-   traffic forecasting artificial intelligence
-   traffic forecasting neural networks models

For each result, choose 10 results (those that most attract you) and put all of them into a single csv document. Do not worry if some are picked several times:

``` r
library(knitr)
library(dplyr)

data <- read.csv(find.file('SoTA.csv'), header = T, sep = ';')
data %>%
  mutate(title = paste0(substr(title,1,40), '...')) %>%
  mutate(url = paste0(substr(url,1,20), '...')) %>%
  select(title, url, year, cited) %>% head %>% kable
```

| title                                       | url                       |  year|  cited|
|:--------------------------------------------|:--------------------------|-----:|------:|
| A forecast comparison of volatility mode... | <https://onlinelibrar>... |  2005|   1427|
| A survey of techniques for internet traf... | <https://ieeexplore.i>... |  2008|   1123|
| Comparison of parametric and nonparametr... | <https://www.scienced>... |  2002|    762|
| Comparison of parametric and nonparametr... | <https://www.scienced>... |  2002|    762|
| Traffic Flow Prediction With Big Data: A... | <https://ieeexplore.i>... |  2015|    562|
| Traffic flow prediction with big data: A... | <https://ieeexplore.i>... |  2015|    562|

Step 2: arrange
---------------

Now we can sort by year, cited and times we selected it:

``` r
data.sum <- data %>%
  group_by(url) %>%
  summarise(freq = length(url), title = first(title), 
            URL = first(url), year = first(year), 
            cited = first(cited)) %>%
  mutate(url = URL) %>%
  mutate(title = paste0(substr(title,1,50), '...')) %>%
  arrange(desc(freq), desc(year), desc(cited)) 
  
data.sum %>% select(title, year, cited, freq) %>% head %>% kable
```

| title                                                 |  year|  cited|  freq|
|:------------------------------------------------------|-----:|------:|-----:|
| An object-oriented neural network approach to shor... |  2001|    343|     4|
| Traffic Flow Forecasting: Comparison of Modeling A... |  1997|    491|     4|
| Should we use neural networks or statistical model... |  1997|    201|     4|
| Traffic Flow Prediction With Big Data: A Deep Lear... |  2015|    562|     3|
| Short-Term Traffic Flow Forecasting: An Experiment... |  2013|    184|     3|
| A Bayesian network approach to traffic flow foreca... |  2006|    385|     3|

> Sort according to your own criteria. It could be interesting to select recent stuff, cited many times.

### Step 2: weight

We can even add a weight:

``` r
data.sum %>%
  mutate(W = (cited + (2018 - year)) * freq) %>% 
  select(title, year, cited, freq, W) %>%
  mutate(title = paste0(substr(title,1,50), '...')) %>%
  arrange(desc(W)) %>% kable
```

| title                                                 |  year|  cited|  freq|     W|
|:------------------------------------------------------|-----:|------:|-----:|-----:|
| Traffic Flow Forecasting: Comparison of Modeling A... |  1997|    491|     4|  2048|
| Traffic Flow Prediction With Big Data: A Deep Lear... |  2015|    562|     3|  1695|
| Comparison of parametric and nonparametric models ... |  2002|    762|     2|  1556|
| An object-oriented neural network approach to shor... |  2001|    343|     4|  1440|
| A forecast comparison of volatility models: does a... |  2005|   1427|     1|  1440|
| Combining Kohonen maps with ARIMA time series mode... |  1996|    401|     3|  1269|
| A Bayesian network approach to traffic flow foreca... |  2006|    385|     3|  1191|
| A survey of techniques for internet traffic classi... |  2008|   1123|     1|  1133|
| Modeling and Forecasting Vehicular Traffic Flow as... |  2003|    526|     2|  1082|
| Short-Term Freeway Traffic Flow Prediction: Bayesi... |  2006|    300|     3|   936|
| Should we use neural networks or statistical model... |  1997|    201|     4|   888|
| Nonparametric Regression and Short-Term Freeway Tr... |  1991|    375|     2|   804|
| Short-term traffic forecasting: Overview of object... |  2004|    386|     2|   800|
| Short-term traffic forecasting: Where we are and w... |  2014|    331|     2|   670|
| Short-term inter-urban traffic forecasts using neu... |  1997|    271|     2|   584|
| Short-Term Traffic Flow Forecasting: An Experiment... |  2013|    184|     3|   567|
| Optimized and meta-optimized neural networks for s... |  2005|    468|     1|   481|
| A multivariate state space approach for urban traf... |  2003|    437|     1|   452|
| Short-term traffic flow prediction models-a compar... |  1994|    390|     1|   414|
| Use of sequential learning for short-term traffic ... |  2001|    189|     2|   412|
| Time Delay Neural Networks Designed Using Genetic ... |  2004|    386|     1|   400|
| Statistical methods versus neural networks in tran... |  2011|    380|     1|   387|
| Online-SVR for short-term traffic flow prediction ... |  2009|    304|     1|   313|
| Urban Freeway Traffic Flow Prediction: Application... |  1998|    283|     1|   303|
| Application of Subset Autoregressive Integrated Mo... |  1999|    250|     1|   269|
| Dynamic Wavelet Neural Network Model for Traffic F... |  2005|    250|     1|   263|
| Multi-scale Internet traffic forecasting using neu... |  2012|    117|     2|   246|
| Time series models for internet traffic......         |  1996|    200|     1|   222|
| An urban traffic flow model integrating neural net... |  1997|    193|     1|   214|
| The use of neural networks and time series models ... |  1993|     76|     2|   202|
| Long short-term memory neural network for traffic ... |  2015|    189|     1|   192|
| Different methods of traffic forecast based on rea... |  2004|    176|     1|   190|
| Bayesian Time-Series Model for Short-Term Traffic ... |  2013|    184|     1|   189|
| Neural-Network-Based Models for Short-Term Traffic... |  2012|    176|     1|   182|
| Prediction, Expectation, and Surprise: Methods, De... |  2012|    172|     1|   178|
| A time series model of long-term NSFNET backbone t... |  1994|    153|     1|   177|
| Multivariate Short-Term Traffic Flow Forecasting U... |  2009|    146|     1|   155|
| Short term traffic forecasting using the local lin... |  2003|    128|     1|   143|
| An SVM-based machine learning method for accurate ... |  2010|    134|     1|   142|
| MODELING AND FORECASTING VEHICULAR TRAFFIC FLOW AS... |  1999|    122|     1|   141|
| Recent advances and applications in the field of s... |  1997|    113|     1|   134|
| A performance evaluation of neural network models ... |  1998|    100|     1|   120|
| LSTM network: a deep learning approach for short-t... |  2017|     58|     2|   118|
| Use of Local Linear Regression Model for Short-Ter... |  2003|     99|     1|   114|
| Forecasting urban traffic flow by SVR with continu... |  2011|     95|     1|   102|
| ANALYSIS OF FREEWAY TRAFFIC TIME-SERIES DATA BY US... |  1993|     76|     1|   101|
| Short-term traffic speed forecasting hybrid model ... |  2013|     94|     1|    99|
| Internet Traffic Forecasting using Neural Networks... |  2006|     85|     1|    97|
| Traffic forecasting using least squares support ve... |  2009|     48|     1|    57|
| Time-Aware Multivariate Nearest Neighbor Regressio... |  2015|     19|     1|    22|
| Forecasting traffic time series with multivariate ... |  2016|     13|     1|    15|
| A Comparative Study of Three Multivariate Short-Te... |  2016|     10|     1|    12|

Finally
-------

Now you are ready; just start reading documents from the beginning of this list until you feel confortable with the amount of info you get for your research.

References
----------

-   <https://blog.babak.no/en/2007/05/22/why-and-how-to-write-the-state-of-the-art/>