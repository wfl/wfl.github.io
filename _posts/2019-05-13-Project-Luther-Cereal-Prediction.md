---
Project Luther: Cereal Yield Prediction
---

### Motivation: 

Increasing the world agricultural productivity is necessary to ensure there is enough food to feed the growing world population that is estimated to double every 61 years. Note that the number 61 is cited from the article ["Population Growth Rates” written by Matt Rosenberg that was published on ThoughtCo](https://www.thoughtco.com/population-growth-rates-1435469). Let’s consider cereal productivity, what are some of the determinants that positively or negatively impact it? 


### Data

Nineteen determinants (i.e., the features) and cereal yield (i.e., the target) for 206 countries in a period of at least 25 years were downloaded or scraped from databases that are opened to public, which are [The Food Security Portal](http://www.foodsecurityportal.org/api), [The World Bank Data]( https://data.worldbank.org), [Climate Change Knowledge Portal]( https://climateknowledgeportal.worldbank.org), [Wikipedia]( https://en.wikipedia.org/wiki/International_wheat_production_statistics), and [Food and Agriculture Organization of United Nations](http://www.fao.org). The countries’ codes list was downloaded from [JohnSnowLabs](https://datahub.io/JohnSnowLabs/country-and-continent-codes-list). After exploring and cleaning all the data, only 11 features (see table below) in 111 countries were used to investigate their relations with the target, Ckg (Cereal yield in kg per hectare). 

| Category | Features | Short Description |
| :---------------: | :--------:  | :--------------------------------------------------- |
| Country | code |Three letters country codes|
| Nature | A | Agricultural arable land (% of land area) |
| Nature | Rmin | Minimum precipitation in mm |
| Nature | Tavg | Mean air temperature in  degree C |
| Population | R | Rural population (% of total population) |
| Technology | F | Fertilizer consumption (kilograms per hectare of arable land) |
| Technology | M| Agricultural machinery, tractors per 100 sq. km of arable land |
| Economy | V | Agriculture, forestry, and fishing, value added (% of GDP) |
| Economy | Gc | GDP per capita (current US$) |
| Economy | E | Employment in agriculture (% of total employment) |
| Cost | Dl | Pump price for diesel fuel (US$ per liter) |


### Approach

The dataset is a panel data since the observations were recorded yearly for the same countries. The years in this data were limited to 1991 to 2015 due to the data availability in most of the features. For this initial study, I am assuming the features either don’t change or change in a constant rate through the years. Thus, they are treated as constant, and a fixed-effect model called the Least-Square Dummy Variable (LSDV) is selected as the base linear regression model. The idea of dummy variables in the LSDV model is transforming the countries’ codes into dummy variables. In this case, the transformation leads to a total of 121 features. It is unclear to me how standardization would change the characteristic in the panel data. Therefore, the features aren’t standardized. In addition to the linear regression model, Lasso and Ridge regressions were considered.

A nested cross-validation approach, called forward chaining is implemented to split the training data in a temporal manner [in order to prevent data leakage]( https://towardsdatascience.com/time-series-nested-cross-validation-76adba623eb9). After obtaining the optimized alpha parameters with 10 folds cross-validation, they are used in the 10-fold nested cross-validation procedure to select the best model among the three regression models. LSDV regression was the best model with average R-square of 0.868(+/- 0.042) and average RMSE of 892.637. Ridge regression came in a close second place with average R-square of 0.868 (+/- 0.042) and average RMSE of 895.044. Lasso regression yielded average R-square of 0.853 (+/- 0.062) and average RMSE of 930.557.

At the final step, the LSDV regression model was fit again with the training data in order to estimate the features’ coefficients (see Table below). Next, the regression was applied to predict the holdout set (i.e., test dataset). The R-square, adjusted R-square, and RMSE are 0.79, 0.74, and 1214.560. The residual plot looks fairly good with some outliers. 

| Features| Estimated Coefficients |
| :---------- | :--------------------------: |
| Dl |  325.24 |
| Tavg |  54.48|
| A |  18.48|
| V |  6.79 |
| Rmin | 1.25|
| M |  0.35 |
| Gc |  0.02|
| E |  -19.90 |
| R| -30.69 |

![Figure1: Residual plot for test data](https://github.com/wfl/Project-Luther/blob/master/figures/LSDVregression_testdata_residualplot.png)


### Thoughts

Since no standardization applies to the feature data, it is hard to assume which coefficients are the important determinant for cereal yield by just looking at their values. However, all the positive coefficients indicate the positive relationship with the cereal yield. The negative coefficients are associated to the percent rural population and employment, indicating the decline in population in the rural region, which is one of the determinants that indirectly affect the employment in agriculture. In another perspective, the negative coefficient for employment is also attributed to the advancement of technology in the bioscience and agricultural fields, one of the leading factors that contribute to the high crop productivity.

The actual and predicted cereal yields were plotted against years for a few countries to evaluate the LSDV regression’s goodness-of-fit. The markers on the figures 2 and 3 below represent the actual data while the dotted lines represent the regression lines. The black markers are the training data and the blue markers are the test data. It seems that the regression model is able to provide good prediction for some countries (India, Canada, Indonesia); while failed in other countries (USA, Brazil, Ghana).

![Figure 2a: Cereal Yield trend for India](https://github.com/wfl/Project-Luther/blob/master/figures/Cereal_yield_plot_India.png)

![Figure 2b: Cereal Yield trend for Canada](https://github.com/wfl/Project-Luther/blob/master/figures/Cereal_yield_plot_Canada.png)

![Figure 2c: Cereal Yield trend for Indonesia](https://github.com/wfl/Project-Luther/blob/master/figures/Cereal_yield_plot_Indonesia.png)

![Figure 3a: Cereal Yield trend for USA](https://github.com/wfl/Project-Luther/blob/master/figures/Cereal_yield_plot_USA.png)

![Figure 3b: Cereal Yield trend for Brazil](https://github.com/wfl/Project-Luther/blob/master/figures/Cereal_yield_plot_Brazil.png)

![Figure 3c: Cereal Yield trend for Ghana](https://github.com/wfl/Project-Luther/blob/master/figures/Cereal_yield_plot_Ghana.png)


### Future Work

Obviously the selected regression model is far from perfect. There are always ways to improve the estimation of the coefficients that will maximize the good-of-fit such as revisiting the feature engineering step, choosing a complex model to take into account of the non-linearity in cereal yield trends for some countries, or implementing statistical test analysis to determine the coefficients’ significant. 


Please visit my [github repository](https://github.com/wfl/Project-Luther) for the detail implementation of this project. Thank you for reading. 





