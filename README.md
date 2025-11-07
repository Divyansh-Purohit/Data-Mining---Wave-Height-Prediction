# Data-Mining & EMM: Wave-Height-Prediction
Group Project for EMM Group 3 

[Report Link Overleaf](https://www.overleaf.com/7341144173nwmdnfbcvbpn#0e3392)

Overview

A significant portion of global trade and industrial activity relies on maritime operations, including shipping, oil extraction, and naval logistics. Accurate wave height prediction is therefore critical for ensuring the safety, efficiency, and scheduling of offshore activities. However, despite major advances in data-driven forecasting, predicting ocean dynamics remains a challenging time series problem due to their non-stationary and highly context-dependent nature.

Deep learning models, particularly recurrent and sequence-based architectures, have demonstrated strong predictive power for such tasks. But, their performance often fluctuates with changing environmental conditions, seasonal variations, and regional differences. This raises a fundamental question under what conditions does a forecasting model perform particularly well or poorly? Answering this requires not only high predictive accuracy but also a deeper understanding of model behavior across different temporal and environmental contexts.

Exceptional Model Mining (EMM) provides a framework for this kind of model introspection by identifying subgroups in which a predictive model behaves differently from its overall trend. However, existing approaches such as SCaPE (Subgroup Characterization and Performance Explanation) are designed for static datasets and cannot directly capture how subgroup behavior evolves over time. Temporal dependencies—crucial in time series forecasting—remain unaddressed in this framework.

To bridge this gap, we extend the SCaPE methodology into the temporal domain. Our proposed Temporal SCaPE framework evaluates model performance across time windows, enabling the discovery of subgroups that exhibit distinct and consistent temporal behaviors. This adaptation allows EMM to handle sequential data, providing insights into when and why model errors change over time.

This helps not only with accurate forecasting but also with understanding model behavior under different ocean conditions.

Data Used:

This study leverages historical data offered by National Data Bouy Center, which captures 24-hour every 10 minutes information about meteorological condition in various part of the world. Since there are 377 buoys in total, a heuristic was used to select the buoys for analysis. The selection was inspired by previous studies, under the assumption that those works—focused on time series forecasting—had already identified buoys with high-quality data. We selected two Hawaii buoys 51001 and 51101. These stations are close to each other which was crucial, in creation of the final data set. This will be further discussed in data reprocessing. In this report, the data covers the period from September 19, 2018, at 04:40 to December 31, 2018, at 23:40. This represents the most recent time span, chosen to minimize the number of missing values.

In the final dataset, significant wave height was selected as the dependent variable. This choice considerably influenced the dataset’s structure, as all data were aggregated to an hourly frequency, matching the interval at which wave height was measured. Additionally, thirteen other variables were included in the final dataset, three of which were engineered using the available 

| **Variable**                       | **Description (Short)**                   | **Type**    |
| ---------------------------------- | ----------------------------------------- | ----------- |
| **Significant Wave Height (WVHT)** | Avg height of highest one-third waves (m) | Continuous  |
| **Wind Direction (WDIR)**          | Direction wind is coming from (°)         | Discrete    |
| **Wind Speed (WSPD)**              | Avg wind speed over 8 min (m/s)           | Continuous  |
| **Gust Speed (GST)**               | Peak 5–8 sec gust within 8 min (m/s)      | Continuous  |
| **Dominant Wave Period (DPD)**     | Period of waves carrying max energy (s)   | Continuous  |
| **Average Wave Period (APD)**      | Mean wave period over 20 min (s)          | Continuous  |
| **Wave Direction (MWD)**           | Dominant wave direction (°)               | Discrete    |
| **Sea Level Pressure (PRES)**      | Atmospheric pressure at sea level (hPa)   | Continuous  |
| **Air Temperature (ATMP)**         | Air temperature (°C)                      | Continuous  |
| **Sea Surface Temperature (WTMP)** | Sea surface temperature (°C)              | Continuous  |
| **Air–Water Temp. Diff. (AWTPDF)** | Engineered feature: ATMP − WTMP           | Continuous  |
| **isSummer?**                      | 1 = summer month, 0 = otherwise           | Categorical |
| **Hour**                           | Hour of the day (0–23)                    | Discrete    |

Engineered features were created to capture additional information not directly available from the original variables. Previous studieshave analyzed the effect of the air–water temperature difference on wave height [Kettle 2015], which motivated the inclusion of this feature. Furthermore, visual inspection of the data revealed clear seasonal patterns. To account for these, two additional features — isSummer? and Hour — were introduced to analyze variations inbehavior across specific seasons and times of day.

| **Variable**                                  | **Description (Short)**        | **Type**   |
| --------------------------------------------- | ------------------------------ | ---------- |
| **Air–Water Temperature Difference (AWTPDF)** | ( T_{air} - T_{sea} ) (°C)     | Continuous |
| **isSummer?**                                 | 1 if month ∈ {May–Oct}, else 0 | Binary     |
| **Hour**                                      | Measurement hour (0–23)        | Discrete   |

Target:
WVHT — Significant Wave Height (m)

Experimaental setup: 
Prior to model training, feature selection was performed using correlation analysis to identify predictors most strongly associated with the target variable. The dataset preserved its temporal order, with shuffling disabled, to maintain the sequential nature of the data for accurate time series modeling.

For sequence modeling, time series windows of length 24 were constructed to capture temporal dependencies within the data. The predictive model consisted of a single LSTM layer with 50 units, followed by a dense output layer. Non-linear dependencies were modeled using the tanh activation function. The LSTM was trained to predict wave height based on the selected environmental predictors within each time window.

To analyze model behavior across different conditions, a temporal Exceptional Model Mining (EMM) approach was applied. Due to computational constraints and the exploratory scope of this study, the subgroup search was limited to single and dual predicate subgroups. This approach strikes a balance between interpretability and tractability while still highlighting key patterns in model performance.

For single predicate subgroups, each binned feature was evaluated independently. Continuous variables were first discretized into quantile-based bins, converting them into categorical ranges. This stabilizes the subgroup search and improves interpretability, allowing patterns to be analyzed over specific intervals rather than individual continuous values. Each binned feature was then assessed using a time-aware quality measure Q, which accounts for both deviations from the global RMSE and temporal variance. The quality measure is defined as:

**Q = (mean_RMSE − global_RMSE) − λt · var_RMSE**

| Term        | Description                                            |
| ----------- | ------------------------------------------------------ |
| mean_RMSE   | Average RMSE of the subgroup across months             |
| global_RMSE | Overall RMSE of the model on the entire test set       |
| var_RMSE    | Temporal variance of the subgroup’s RMSE across months |
| (\lambda_t) | Tunable weight parameter for temporal variance         |

For dual-predicate subgroups, all unique pairs of features were combined, and only samples satisfying both conditions simultaneously were evaluated using the same quality measure. Subgroups with fewer than 100 samples were discarded to ensure statistical reliability, and only the top-ranking subgroups based on 
∣Q| were retained.

