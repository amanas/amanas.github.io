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
tags: [SoTA, research]
---


Intro
-----

Currently, I am starting to write my MSc. Thesis. The research is about forecasting the flow traffic in the city of Madrid.

When you start a research, tipically the first step is to review the **"State of The Art"**.

Benefits:

-   it demonstrates the novelty of your research results
-   you learn about your research problem
-   it shows different approaches to a solution
-   you can reuse from what others have done

In order to elaborate the SoTA, I have proceeded as follows.

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
data %>% select(title, url, year, cited) %>% head %>% kable
```

| title                                                                             | url                                                                   |  year|  cited|
|:----------------------------------------------------------------------------------|:----------------------------------------------------------------------|-----:|------:|
| A forecast comparison of volatility models: does anything beat a GARCH(1,1)?      | <https://onlinelibrary.wiley.com/doi/full/10.1002/jae.800>            |  2005|   1427|
| A survey of techniques for internet traffic classification using machine learning | <https://ieeexplore.ieee.org/abstract/document/4738466>               |  2008|   1123|
| Comparison of parametric and nonparametric models for traffic flow forecasting    | <https://www.sciencedirect.com/science/article/pii/S0968090X02000098> |  2002|    762|
| Comparison of parametric and nonparametric models for traffic flow forecasting    | <https://www.sciencedirect.com/science/article/pii/S0968090X02000098> |  2002|    762|
| Traffic Flow Prediction With Big Data: A Deep Learning Approach                   | <https://ieeexplore.ieee.org/document/6894591>                        |  2015|    562|
| Traffic flow prediction with big data: A deep learning approach                   | <https://ieeexplore.ieee.org/document/6894591>                        |  2015|    562|

Step 2: arrange
---------------

Now we can sort by year, cited and times we selected it:

``` r
data.sum <- data %>% group_by(url) %>%
  summarise(title = first(title), year = first(year),
            cited = first(cited), freq = length(url)) %>%
  arrange(desc(freq), desc(year), desc(cited))

data.sum %>% head %>% kable
```

| url                                                                                                                                                                    | title                                                                                                           |  year|  cited|  freq|
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------|-----:|------:|-----:|
| <https://www.sciencedirect.com/science/article/abs/pii/S0377221700001259>                                                                                              | An object-oriented neural network approach to short-term traffic forecasting                                    |  2001|    343|     4|
| <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-947X(1997)123:4(261)>                                                                                              | Traffic Flow Forecasting: Comparison of Modeling Approaches                                                     |  1997|    491|     4|
| <https://www.sciencedirect.com/science/article/pii/S0169207096006991>                                                                                                  | Should we use neural networks or statistical models for short-term motorway traffic forecasting?                |  1997|    201|     4|
| <https://ieeexplore.ieee.org/document/6894591>                                                                                                                         | Traffic Flow Prediction With Big Data: A Deep Learning Approach                                                 |  2015|    562|     3|
| <https://ieeexplore.ieee.org/abstract/document/6482260>                                                                                                                | Short-Term Traffic Flow Forecasting: An Experimental Comparison of Time-Series Analysis and Supervised Learning |  2013|    184|     3|
| <https://www.researchgate.net/profile/Changshui_Zhang2/publication/3427947_A_Bayesian_Network_Approach_to_Traffic_Flow_Forecasting/links/55b1ea2208aed621ddfd6e22.pdf> | A Bayesian network approach to traffic flow forecasting                                                         |  2006|    385|     3|

> Sort according to your own criteria. It could be interesting to select recent stuff, cited many times.

Step 3: weight
--------------

We can even add a weight:

``` r
data.sum %>%
  mutate(W = (cited + (2018 - year)) * freq) %>%
  arrange(desc(W)) %>% kable
```

| url                                                                                                                                                                    | title                                                                                                                                      |  year|  cited|  freq|     W|
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------|-----:|------:|-----:|-----:|
| <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-947X(1997)123:4(261)>                                                                                              | Traffic Flow Forecasting: Comparison of Modeling Approaches                                                                                |  1997|    491|     4|  2048|
| <https://ieeexplore.ieee.org/document/6894591>                                                                                                                         | Traffic Flow Prediction With Big Data: A Deep Learning Approach                                                                            |  2015|    562|     3|  1695|
| <https://www.sciencedirect.com/science/article/pii/S0968090X02000098>                                                                                                  | Comparison of parametric and nonparametric models for traffic flow forecasting                                                             |  2002|    762|     2|  1556|
| <https://www.sciencedirect.com/science/article/abs/pii/S0377221700001259>                                                                                              | An object-oriented neural network approach to short-term traffic forecasting                                                               |  2001|    343|     4|  1440|
| <https://onlinelibrary.wiley.com/doi/full/10.1002/jae.800>                                                                                                             | A forecast comparison of volatility models: does anything beat a GARCH(1,1)?                                                               |  2005|   1427|     1|  1440|
| <https://research.utwente.nl/files/6769849/Voort96combining.pdf>                                                                                                       | Combining Kohonen maps with ARIMA time series models to forecast traffic flow                                                              |  1996|    401|     3|  1269|
| <https://www.researchgate.net/profile/Changshui_Zhang2/publication/3427947_A_Bayesian_Network_Approach_to_Traffic_Flow_Forecasting/links/55b1ea2208aed621ddfd6e22.pdf> | A Bayesian network approach to traffic flow forecasting                                                                                    |  2006|    385|     3|  1191|
| <https://ieeexplore.ieee.org/abstract/document/4738466>                                                                                                                | A survey of techniques for internet traffic classification using machine learning                                                          |  2008|   1123|     1|  1133|
| <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-947X(2003)129:6(664)>                                                                                              | Modeling and Forecasting Vehicular Traffic Flow as a Seasonal ARIMA Process: Theoretical Basis and Empirical Results                       |  2003|    526|     2|  1082|
| <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-947X(2006)132:2(114)>                                                                                              | Short-Term Freeway Traffic Flow Prediction: Bayesian Combined Neural Network Approach                                                      |  2006|    300|     3|   936|
| <https://www.sciencedirect.com/science/article/pii/S0169207096006991>                                                                                                  | Should we use neural networks or statistical models for short-term motorway traffic forecasting?                                           |  1997|    201|     4|   888|
| <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-947X(1991)117:2(178)>                                                                                              | Nonparametric Regression and Short-Term Freeway Traffic Forecasting                                                                        |  1991|    375|     2|   804|
| <https://www.tandfonline.com/doi/abs/10.1080/0144164042000195072>                                                                                                      | Short-term traffic forecasting: Overview of objectives and methods                                                                         |  2004|    386|     2|   800|
| <https://www.sciencedirect.com/science/article/pii/S0968090X14000096>                                                                                                  | Short-term traffic forecasting: Where we are and where we are going                                                                        |  2014|    331|     2|   670|
| <https://www.sciencedirect.com/science/article/pii/S0169207096006978>                                                                                                  | Short-term inter-urban traffic forecasts using neural networks                                                                             |  1997|    271|     2|   584|
| <https://ieeexplore.ieee.org/abstract/document/6482260>                                                                                                                | Short-Term Traffic Flow Forecasting: An Experimental Comparison of Time-Series Analysis and Supervised Learning                            |  2013|    184|     3|   567|
| <https://www.sciencedirect.com/science/article/pii/S0968090X05000276>                                                                                                  | Optimized and meta-optimized neural networks for short-term traffic flow prediction: A genetic approach                                    |  2005|    468|     1|   481|
| <https://www.sciencedirect.com/science/article/pii/S0968090X03000044>                                                                                                  | A multivariate state space approach for urban traffic flow modeling and prediction                                                         |  2003|    437|     1|   452|
| <https://ieeexplore.ieee.org/abstract/document/400094>                                                                                                                 | Short-term traffic flow prediction models-a comparison of neural network and nonparametric regression approaches                           |  1994|    390|     1|   414|
| <https://www.sciencedirect.com/science/article/pii/S0968090X00000395>                                                                                                  | Use of sequential learning for short-term traffic flow forecasting                                                                         |  2001|    189|     2|   412|
| <https://link.springer.com/chapter/10.1007/3-540-45517-5_33>                                                                                                           | Time Delay Neural Networks Designed Using Genetic Algorithms for Short Term Inter-City Traffic Forecasting                                 |  2004|    386|     1|   400|
| <https://www.sciencedirect.com/science/article/pii/S0968090X10001610>                                                                                                  | Statistical methods versus neural networks in transportation research: Differences, similarities and some insights                         |  2011|    380|     1|   387|
| <https://www.sciencedirect.com/science/article/pii/S0957417408004740>                                                                                                  | Online-SVR for short-term traffic flow prediction under typical and atypical traffic conditions                                            |  2009|    304|     1|   313|
| trrjournalonline.trb.org/doi/abs/10.3141/1644-14                                                                                                                       | Urban Freeway Traffic Flow Prediction: Application of Seasonal Autoregressive Integrated Moving Average and Exponential Smoothing Models   |  1998|    283|     1|   303|
| <https://trrjournalonline.trb.org/doi/abs/10.3141/1678-22>                                                                                                             | Application of Subset Autoregressive Integrated Moving Average Model for Short-Term Freeway Traffic Volume Forecasting                     |  1999|    250|     1|   269|
| <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-947X(2005)131:10(771)>                                                                                             | Dynamic Wavelet Neural Network Model for Traffic Flow Forecasting                                                                          |  2005|    250|     1|   263|
| <https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1468-0394.2010.00568.x>                                                                                             | Multi-scale Internet traffic forecasting using neural networks and time series methods                                                     |  2012|    117|     2|   246|
| <https://ieeexplore.ieee.org/abstract/document/493355>                                                                                                                 | Time series models for internet traffic                                                                                                    |  1996|    200|     1|   222|
| <https://www.sciencedirect.com/science/article/pii/S0968090X97000156>                                                                                                  | An urban traffic flow model integrating neural networks                                                                                    |  1997|    193|     1|   214|
| <https://trid.trb.org/view/390622>                                                                                                                                     | The use of neural networks and time series models for short term traffic forecasting: a comparative study                                  |  1993|     76|     2|   202|
| <https://www.sciencedirect.com/science/article/pii/S0968090X15000935>                                                                                                  | Long short-term memory neural network for traffic speed prediction using remote microwave sensor data                                      |  2015|    189|     1|   192|
| <https://www.sciencedirect.com/science/article/abs/pii/S0377221703004788>                                                                                              | Different methods of traffic forecast based on real data                                                                                   |  2004|    176|     1|   190|
| <https://ascelibrary.org/doi/abs/10.1061/(ASCE)0733-947X(2007)133:3(180)>                                                                                              | Bayesian Time-Series Model for Short-Term Traffic Flow Forecasting                                                                         |  2013|    184|     1|   189|
| <https://ieeexplore.ieee.org/abstract/document/6088012>                                                                                                                | Neural-Network-Based Models for Short-Term Traffic Flow Forecasting Using a Hybrid Exponential Smoothing and Levenberg-Marquardt Algorithm |  2012|    176|     1|   182|
| <https://arxiv.org/abs/1207.1352>                                                                                                                                      | Prediction, Expectation, and Surprise: Methods, Designs, and Study of a Deployed Traffic Forecasting Service                               |  2012|    172|     1|   178|
| <https://ieeexplore.ieee.org/abstract/document/368876>                                                                                                                 | A time series model of long-term NSFNET backbone traffic                                                                                   |  1994|    153|     1|   177|
| <https://ieeexplore.ieee.org/abstract/document/4908946>                                                                                                                | Multivariate Short-Term Traffic Flow Forecasting Using Time-Series Analysis                                                                |  2009|    146|     1|   155|
| 128.200.36.2/its/publications/papers/CTSS/UCI-ITS-TS-WP-02-5.pdf                                                                                                       | Short term traffic forecasting using the local linear regression model                                                                     |  2003|    128|     1|   143|
| <https://link.springer.com/article/10.1007/s10796-008-9131-2>                                                                                                          | An SVM-based machine learning method for accurate internet traffic classification                                                          |  2010|    134|     1|   142|
| <https://trid.trb.org/view/648324>                                                                                                                                     | MODELING AND FORECASTING VEHICULAR TRAFFIC FLOW AS A SEASONAL STOCHASTIC TIME SERIES PROCESS                                               |  1999|    122|     1|   141|
| <https://www.sciencedirect.com/science/article/pii/S0169207096006954>                                                                                                  | Recent advances and applications in the field of short-term traffic forecasting                                                            |  1997|    113|     1|   134|
| <https://www.sciencedirect.com/science/article/pii/S089571779800065X>                                                                                                  | A performance evaluation of neural network models in traffic volume forecasting                                                            |  1998|    100|     1|   120|
| <http://digital-library.theiet.org/content/journals/10.1049/iet-its.2016.0208>                                                                                         | LSTM network: a deep learning approach for short-term traffic forecast                                                                     |  2017|     58|     2|   118|
| <https://trrjournalonline.trb.org/doi/abs/10.3141/1836-18>                                                                                                             | Use of Local Linear Regression Model for Short-Term Traffic Forecasting                                                                    |  2003|     99|     1|   114|
| <https://www.sciencedirect.com/science/article/pii/S0307904X10003409>                                                                                                  | Forecasting urban traffic flow by SVR with continuous ACO                                                                                  |  2011|     95|     1|   102|
| <https://trid.trb.org/view/148123>                                                                                                                                     | ANALYSIS OF FREEWAY TRAFFIC TIME-SERIES DATA BY USING BOX-JENKINS TECHNIQUES                                                               |  1993|     76|     1|   101|
| <https://www.sciencedirect.com/science/article/pii/S0968090X1200109X>                                                                                                  | Short-term traffic speed forecasting hybrid model based on Chaos-Wavelet Analysis-Support Vector Machine theory                            |  2013|     94|     1|    99|
| <https://ieeexplore.ieee.org/abstract/document/1716452>                                                                                                                | Internet Traffic Forecasting using Neural Networks                                                                                         |  2006|     85|     1|    97|
| <https://www.tandfonline.com/doi/abs/10.1080/18128600902823216>                                                                                                        | Traffic forecasting using least squares support vector machines                                                                            |  2009|     48|     1|    57|
| <https://ieeexplore.ieee.org/abstract/document/7167678>                                                                                                                | Time-Aware Multivariate Nearest Neighbor Regression Methods for Traffic Flow Prediction                                                    |  2015|     19|     1|    22|
| <https://www.sciencedirect.com/science/article/abs/pii/S0096300316304477>                                                                                              | Forecasting traffic time series with multivariate predicting method                                                                        |  2016|     13|     1|    15|
| <https://www.tandfonline.com/doi/abs/10.1080/15472450.2016.1147813>                                                                                                    | A Comparative Study of Three Multivariate Short-Term Freeway Traffic Flow Forecasting Methods With Missing Data                            |  2016|     10|     1|    12|

Finally
-------

Now you are ready; just start reading documents from the beginning of this list until you feel confortable with the amount of info you get for your research.

References
----------

-   <https://blog.babak.no/en/2007/05/22/why-and-how-to-write-the-state-of-the-art/>
