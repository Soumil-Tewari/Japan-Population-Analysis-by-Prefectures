Modeling Regional Population Decline in Japan
A prefecture-level study of population growth, fertility, migration, demographic structure, and predictive performance
Study period: 2006–2025 for the main prefectural panel; 2013–2022 for the economic-feature analysis.

Data: Official Japanese population, demographic, migration, and economic statistics. I verified the underlying source series against the original releases before constructing the analytical dataset.
Overview
Japan's population decline varies substantially across prefectures. Fertility, migration, age structure, economic conditions, and the persistence of past population change all differ regionally. I use prefecture-year data to examine these patterns through three complementary analyses:
1. Panel inference: estimate conditional associations between demographic characteristics and prefectural population growth using prefecture and year fixed effects.
2. Continuous prediction: forecast the annual population growth rate and compare machine-learning models with simple persistence baselines.
3. Binary classification: estimate whether a prefecture experiences population decline, defined as a negative annual population growth rate, and compare logistic regression and random forest against baseline classifiers.
Each predictive model is evaluated against simple persistence and historical-mean baselines. This makes it possible to assess whether additional model complexity produces a measurable improvement.
Research questions
I organize the analysis around five questions:
- How are lagged total fertility rate, net migration rate, and dependency ratio associated with annual population growth across Japan's prefectures?
- Do these associations remain when prefecture-specific and year-specific effects are controlled for?
- Does adding economic information improve forecasts of prefectural population growth?
- Can models classify whether a prefecture will experience population decline, and do they improve on a previous-year persistence baseline?
- How does severe class imbalance affect the interpretation of classification accuracy and other evaluation metrics?
Working hypotheses
ID	Working hypothesis	Evidence in the current analysis	Assessment
H1	Higher lagged fertility is positively associated with population growth.	Full-sample two-way fixed-effects estimate: +0.4415, clustered SE 0.1296, p = 0.000686, 95% CI [0.1872, 0.6958].	Supported as a conditional association in the main panel model. It is not a causal estimate. The sign of fertility in the inferential decline-logit models is unexpectedly negative and requires further investigation.
H2	Higher lagged net migration rate is positively associated with population growth.	Full-sample estimate: +0.07236, clustered SE 0.00742, p < 0.001, 95% CI [0.05781, 0.08691].	Supported as a conditional association in the main panel model.
H3	A higher lagged dependency ratio is associated with lower population growth.	Full-sample estimate: −0.01787, clustered SE 0.00297, p < 0.001, 95% CI [−0.02371, −0.01204].	Supported in the main full-sample panel model. In the shorter 2013–2022 economic-feature specification, the dependency coefficient is not robustly significant.
H4	Population dynamics exhibit persistence from one year to the next.	Previous-year growth is strongly correlated with migration (r = 0.822) and dependency (r = −0.792). The previous-year growth baseline is strong in most forecasting comparisons; the previous-year decline baseline reaches 94.68% accuracy in the 2020–2025 classification test.	Supported descriptively and predictively in the observed sample, but the classification persistence baseline is a one-step-ahead comparator that uses each test year's actual prior-year status.
H5	Adding economic variables improves continuous growth prediction.	In 2016–2019 walk-forward validation, demographic-only Ridge: MAE 0.0799, RMSE 0.1047; Ridge with economics: MAE 0.0816, RMSE 0.1062. In 2016–2022 expanding validation, Ridge demographics: RMSE 0.1207; Ridge plus economics: 0.1217.	Not supported by the reported validation results. There is a very small improvement in MAE in one 2020–2022 test comparison, but RMSE is unchanged and R² is effectively unchanged.
H6	Logistic regression and random forest outperform simple baselines for decline classification.	The previous-year baseline has the highest test accuracy (94.68%). Tuned classifiers achieve higher balanced accuracy (93.21% logistic; 92.34% random forest) than the persistence baseline (83.41%).	Partially supported, depending on the metric. The classifiers improve balance across the two classes but do not outperform the baseline in overall accuracy.


These hypotheses are tested as empirical associations and predictive comparisons. Fixed-effects regression on observational data does not establish that fertility, migration, or age structure causally changes population growth.
Study design and data
Primary analysis panel
The main panel used for the central demographic fixed-effects model contains:
Dimension	Value
Geographic units	47 Japanese prefectures
Years	2006–2025
Number of years	20
Observations	940
Panel structure	Balanced in the reported primary model (47 × 20)
National aggregate	Excluded from the 47-prefecture panel


The economic-feature analysis uses a shorter common sample:
Dimension	Value
Geographic units	47 prefectures
Years	2013–2022
Number of years	10
Observations	470


Main outcome variables
- Population Growth Rate (%): the continuous outcome used in panel regression and forecasting. The rate follows the population concept and reference dates in the underlying source series.
- Population Decline: binary outcome defined as 1 when Population Growth Rate (%) < 0, and 0 otherwise. A zero growth rate is therefore classified as no decline.
The decline indicator is consistent with the rule Population Growth Rate (%) < 0 across all 940 observations checked.
Main explanatory variables
The main demographic predictors are lagged by one year so that the model uses information from the preceding year:
Variable	Definition	Role
Total fertility rate (lag 1)	Previous-year total fertility rate	Main demographic predictor
Japanese Net Migration Rate (lag 1)	Previous-year Japanese net migration rate	Main demographic predictor
Dependency Ratio (lag 1)	Previous-year dependency ratio	Main demographic predictor
Previous-Year Growth Rate	Prior-year population growth rate	Persistence feature in forecasting and classification
Previous-Year Decline	Prior-year binary decline status	Persistence classification baseline


I retain the variable names and units of the source series. Rate denominators and demographic definitions follow the corresponding official statistical definitions.
Additional variables present in the wider data extract
The broader dataset also contains the following variables. They were considered during exploratory analysis; not all are included in the main specification:
- Total population (both sexes)
- Population Growth Rate (%)
- Population Decline
- Total fertility rate
- Natural Change
- Net Migration
- Birth Rate
- Death Rate
- Net Migration Rate
- Dependency Ratio
- Population Density
- DID share (%)
- Prefectural income per person (2015 base), in thousand yen
- Monthly household income (2+ person workers' households), in yen
- Unemployment rate (both sexes), percent
- Life expectancy (male) and life expectancy (female)
- Crude marriage rates (per 1,000 population)
- Crude divorce rates (per 1,000 population)
The economic extension specifically used:
- Prefectural income per person (2015 base)[thousand yen] (lag 1)
- Monthly household income (2+ person workers' households)[yen] (lag 1)
Data provenance and source documentation
I verified the underlying series against their original releases. The dataset draws on official Japanese statistics, including e-Stat, Statistics Bureau of Japan, IPSS, and Ministry of Justice sources for the population, demographic, migration, and economic variables used in the analysis.

For reproducibility, I document each source by institution, table or series title, table ID where available, URL, release vintage, access date, geographic coverage, population concept, unit, and any transformation applied. The source-to-variable mapping is important because population estimates, census counts, and administrative-register statistics can differ in reference date and coverage.
Data quality and missingness checks
The broader raw dataset has a different coverage from the 940-observation prefectural panel. Its missing-value audit is:
Variable	Missing values	Missing (%) reported
Population Growth Rate (%)	48	1.94%
Population Decline	48	1.94%
Total fertility rate	498	20.14%
Natural Change	968	39.14%
Net Migration	1,897	76.71%
Birth Rate	968	39.14%
Death Rate	28	Not reported
Net Migration Rate	1,897	76.71%
Dependency Ratio	1,482	Not reported
Population Density	0	0%
DID share (%)	1,993	80.59%
Prefectural income per person	1,993	80.59%
Monthly household income	25	Not reported
Unemployment rate	1,993	80.59%
Male life expectancy	1,993	80.59%
Female life expectancy	1,993	80.59%
Crude marriage rates	1,955	79.05%
Crude divorce rates	1,955	79.05%


Range checks found no negative birth rates, death rates, or dependency ratios, and no DID-share values below 0 or above 100.
Broader exploratory dataset: descriptive statistics
The broader exploratory dataset contains 2,425 non-missing population-growth observations:
Statistic	Population Growth Rate (%)
Count	2,425
Mean	0.012834
Standard deviation	0.666215
Minimum	−2.112017
25th percentile	−0.434573
Median	−0.008412
75th percentile	0.436899
Maximum	2.958928


The binary label count was 1,220 decline, 1,205 no decline, and 48 missing. This broader extract includes a Japan national series beginning in 1950 alongside regional data. Its statistics are separate from the primary prefectural panel, which contains 794 declines and 146 non-declines among 940 observations.

Bivariate correlations in the broader exploratory dataset between population growth rate and the listed variables were: death rate −0.860280; dependency ratio −0.791721; female life expectancy −0.664753; male life expectancy −0.606223; monthly household income −0.491754; unemployment −0.315014; DID share +0.217896; population density +0.243553; total fertility rate +0.258799; prefectural income per person +0.444730; crude divorce rate +0.498522; natural change +0.504893; net migration +0.562745; birth rate +0.757587; net migration rate +0.790384; and crude marriage rate +0.843481. These are unadjusted correlations from the broader dataset, not fixed-effects estimates. They may reflect common time trends, differences in coverage, and the mixture of national and prefectural series.
The broad raw extract and the balanced 47 × 20 modelling panel are different analysis objects. Their observation counts and class proportions must not be mixed.

Methodology
1. Two-way fixed-effects panel regression
The primary panel model is:
\[
Growth_{i,t} = \beta_1 Fertility_{i,t-1} + \beta_2 Migration_{i,t-1}
+ \beta_3 Dependency_{i,t-1} + \alpha_i + \gamma_t + \epsilon_{i,t},
\]
where:
- \(i\) identifies a prefecture;
- \(t\) identifies a year;
- \(\alpha_i\) represents prefecture fixed effects;
- \(\gamma_t\) represents year fixed effects;
- \(\epsilon_{i,t}\) is the idiosyncratic error.
Prefecture fixed effects control for time-invariant differences between prefectures. Year fixed effects control for shocks shared across prefectures in a given year. Standard errors in the main specification were clustered by prefecture.

I interpret each coefficient conditionally. For example, the fertility coefficient measures the within-model association between a one-unit change in lagged fertility and population growth, holding the other predictors and both sets of fixed effects constant. It is an association, not a causal effect.
2. Economic-feature extension
An extended specification was estimated on the shorter 2013–2022 common sample, adding lagged prefectural income per person and household income. The shorter period is due to the more limited coverage of the economic variables.

I tested the joint contribution of the two economic variables using a Wald test. The result was \(\chi^2(2)=1.9158\), p = 0.3837, which does not provide evidence that the two added economic coefficients are jointly different from zero at conventional significance levels.
3. Panel diagnostics and robustness
I assessed multicollinearity, serial correlation, and cross-sectional dependence using:
- VIF: used to quantify predictor redundancy.
- Wooldridge-style serial-correlation test: panel residual/first-difference diagnostic.
- Pesaran CD test: tests cross-sectional dependence among panel units.
- Driscoll–Kraay standard errors: used as a sensitivity method where cross-sectional dependence may be present.
- Sensitivity to influential years: re-estimation excluding 2011 and excluding 2025.
The full-sample Pesaran CD test gives a statistic of −2.1412 (p = 0.0323), indicating cross-sectional dependence at the 5% level. A full-sample serial-correlation check gives p = 0.0749, while a separate shorter-sample Wooldridge-style test gives F = 7.3236 (p = 0.0095). These tests use different model/sample contexts and are reported separately.
4. Continuous forecasting
The continuous prediction target is annual population growth. The main predictor set included previous-year growth and lagged demographic predictors. The following algorithms were explored:
- Previous-year growth persistence baseline
- Historical-mean baseline
- Ridge regression
- Random forest regression
- Gradient boosting regression
Performance was assessed using:
\[
MAE = \frac{1}{n}\sum_{j=1}^{n}|y_j-\hat y_j|,
\]
\[
RMSE = \sqrt{\frac{1}{n}\sum_{j=1}^{n}(y_j-\hat y_j)^2},
\]
and the coefficient of determination \(R^2\).

I use chronological splits with walk-forward and expanding-window validation. I also examined the 2020–2025 test results during model development; I therefore treat this period as an exploratory temporal test, not as an untouched final holdout.
5. Binary classification
The target is:
\[
Decline_{i,t}=\begin{cases}
1 & \text{if } Growth_{i,t}<0,\\
0 & \text{if } Growth_{i,t}\geq 0.
\end{cases}
\]
Predictors included prior-year growth and the three lagged demographic variables. The compared models were:
- Previous-year decline persistence baseline
- Majority-class baseline
- Logistic regression
- Random forest classifier
Class weights were balanced for the machine-learning classifiers. The logistic regression pipeline used median imputation, standardization, and logistic regression. The random forest used median imputation and a forest of 500 trees, maximum depth 5, minimum leaf size 5, and class weighting. The reported random seed was 42.

Classification was evaluated using accuracy, balanced accuracy, class-specific recall, precision, F1, ROC-AUC, average precision, and confusion matrices. Because the test period is highly imbalanced, balanced accuracy and the recall of the minority no-decline class are important complements to accuracy.

Thresholds were selected from expanding-window validation over 2016–2019 by maximizing balanced accuracy, with F1 used as a tie-breaker. The chosen thresholds were 0.90 for logistic regression and 0.81 for random forest.
Results
A. Descriptive population trends
Average prefectural population growth became more negative over the study period:
Year	Mean population growth rate (%)
2006	−0.1891
2007	−0.2119
2008	−0.2867
2009	−0.2925
2010	−0.2486
2011	−0.3910
2012	−0.3884
2013	−0.3857
2014	−0.4130
2015	−0.4261
2016	−0.3581
2017	−0.4217
2018	−0.4722
2019	−0.5146
2020	−0.6113
2021	−0.7323
2022	−0.7173
2023	−0.8184
2024	−0.8386
2025	−1.0659


The number of prefectures with negative growth rose from 35 of 47 in 2006 to 43 of 47 in 2025, peaking at 46 of 47 in 2021.
Year	Declining prefectures	Decline share
2006	35	74.47%
2007	32	68.09%
2008	38	80.85%
2009	38	80.85%
2010	38	80.85%
2011	39	82.98%
2012	39	82.98%
2013	38	80.85%
2014	39	82.98%
2015	40	85.11%
2016	38	80.85%
2017	38	80.85%
2018	39	82.98%
2019	38	80.85%
2020	41	87.23%
2021	46	97.87%
2022	45	95.74%
2023	45	95.74%
2024	45	95.74%
2025	43	91.49%


In 2025, the highest-growth (or least-negative) prefectures were Tokyo (+0.4812%), Okinawa (+0.1514%), Chiba (+0.1202%), and Osaka (+0.0865%). The lowest-growth prefectures were Aomori (−2.1120%), Shimane (−1.9533%), Kochi (−1.9151%), Wakayama (−1.7884%), and Fukushima (−1.7822%). These rankings depend on the population-growth definition and geographic coverage used in the dataset.
B. Main fixed-effects model
Primary two-way fixed-effects model, 2006–2025, 940 observations, 47 prefectures
Predictor	Coefficient	Clustered SE	p-value	95% confidence interval
Total fertility rate (lag 1)	0.441499	0.129578	0.000686	[0.187176, 0.695821]
Japanese net migration rate (lag 1)	0.072358	0.007415	<0.001	[0.057805, 0.086910]
Dependency ratio (lag 1)	−0.017874	0.002973	<0.001	[−0.023710, −0.012038]


Model fit: overall R² 0.2456, within R² 0.6028, between R² 0.8113. The robust F-statistic was 40.021, with p < 0.001.

Interpretation: Within the two-way fixed-effects specification, higher lagged fertility and net migration are positively associated with population growth; higher lagged dependency ratio is negatively associated with growth. All three coefficient estimates are statistically significant under prefecture-clustered standard errors. These estimates support H1–H3 as conditional associations, not causal effects.
C. Robustness to excluding selected years
Predictor	Full sample	Without 2011	Without 2025
Fertility	0.441499	0.482632	0.391519
Migration	0.072358	0.072282	0.073429
Dependency	−0.017874	−0.015947	−0.015931


The signs remain unchanged and the magnitudes are broadly similar in both exclusions. All three predictors remain statistically significant in these sensitivity regressions. Migration is especially stable across the three specifications.
D. Economic-feature extension
The economic extension uses 470 observations covering 47 prefectures and 2013–2022. The model adds lagged prefectural income per person and monthly household income.

The estimated coefficients were:
Predictor	Coefficient	Reported inference
Total fertility rate (lag 1)	+0.6241	Significant with clustered SE (p = 0.0023)
Japanese net migration rate (lag 1)	+0.0659	Significant (p < 0.001)
Dependency ratio (lag 1)	+0.00649	Not significant with clustered SE
Prefectural income per person (lag 1)	+0.000215	Not significant / at most borderline under a sensitivity specification
Monthly household income (lag 1)	−1.14 × 10⁻⁷	Not significant


The joint Wald test for the two economic variables was χ²(2) = 1.9158, p = 0.3837. There is no statistically persuasive evidence in this model that the two economic variables are jointly associated with growth after the included controls and fixed effects. The dependency estimate changes sign relative to the main model and is not robustly distinguishable from zero in this shorter sample.

The Driscoll–Kraay sensitivity specification for 2013–2022 gives fertility 0.6799 (SE 0.2098, p = 0.0013), migration 0.0706 (SE 0.0053, p < 0.001), and dependency 0.0099 (SE 0.0071, p = 0.1660). This reinforces the need to describe the dependency finding as sensitive to model/sample/standard-error specification.
E. Continuous forecasting results
2020–2025 temporal test
Model	MAE	RMSE	R²
Previous-year baseline	0.1453	0.2078	0.8139
Historical-mean baseline	0.4293	0.4832	−0.0060
Ridge regression	0.1273	0.1881	0.8476
Random forest	0.1637	0.2313	0.7695
Gradient boosting	0.1743	0.2556	0.7185


Ridge regression outperformed the previous-year growth baseline on this 2020–2025 test by MAE, RMSE, and R². This is encouraging, but must be interpreted alongside walk-forward validation, where the persistence baseline was often stronger.
Walk-forward validation, 2015–2019
Model	Observations	MAE	RMSE	R²
Previous-year baseline	235	0.0714	0.0957	0.9548
Random forest	235	0.0855	0.1070	0.9435
Gradient boosting	235	0.0884	0.1123	0.9378
Ridge, α = 0.01	235	0.0964	0.1216	0.9271
Ridge, α = 0.1	235	0.0965	0.1217	0.9270
Ridge, α = 1	235	0.0970	0.1222	0.9264
Ridge, α = 10	235	0.1012	0.1262	0.9215
Ridge, α = 100	235	0.1145	0.1415	0.9013
Historical-mean baseline	235	0.1226	0.1515	0.8869


On this earlier walk-forward period, the previous-year growth baseline was best. Corrections to the persistence baseline also failed to beat it: the baseline RMSE was 0.0957, compared with 0.1034 for random-forest correction, 0.1099 for gradient-boosting correction, and 0.1202 for Ridge correction.
Economic-feature walk-forward validation, 2016–2019
The common economic sample contains 470 observations (47 prefectures × 10 years, 2013–2022). For the 2016–2019 walk-forward validation (188 predictions):
Model	MAE	RMSE	R²
Previous-year baseline	0.0748	0.1000	0.9515
Ridge — demographics	0.0799	0.1047	0.9469
Random forest — demographics	0.0818	0.1049	0.9467
Ridge — demographics + economics	0.0816	0.1062	0.9453
Random forest — demographics + economics	0.0831	0.1083	0.9432


The addition of economic features did not improve this walk-forward validation aggregate.
Economic-feature temporal test, 2020–2022
Model	MAE	RMSE	R²
Ridge — demographics	0.1008	0.1403	0.8714
Ridge — demographics + economics	0.0999	0.1403	0.8713
Random forest — demographics + economics	0.1070	0.1484	0.8561
Random forest — demographics	0.1075	0.1496	0.8537
Previous-year baseline	0.1170	0.1564	0.8402


The Ridge model with economics has a slightly lower MAE than the demographic-only Ridge model, but RMSE is equal to the displayed precision and R² is marginally lower. This is not compelling evidence that economic features add predictive value.
Expanding-window validation, 2016–2022
Model	Observations	MAE	RMSE	R²
Ridge — demographics	329	0.0886	0.1207	0.9265
Ridge — demographics + economics	329	0.0896	0.1217	0.9253
Previous-year baseline	329	0.0929	0.1273	0.9183
Random forest — demographics	329	0.0942	0.1278	0.9177
Random forest — demographics + economics	329	0.0962	0.1326	0.9114


In this expanding-window comparison, Ridge with demographic features beat the persistence baseline modestly. Adding economic features slightly worsened the Ridge results. The advantage is not large and model selection was performed after examining overlapping validation/test periods, so it should be treated as exploratory.
F. Classification audit and descriptive differences
The classification sample contains 940 observations across 47 prefectures and 20 years: 794 decline and 146 no-decline observations (84.47% decline). The chronological split contains 658 training observations (80.40% decline) and 282 test observations (93.97% decline).

Feature summaries by class:
Feature	No decline: mean	No decline: median	No decline: SD	Decline: mean	Decline: median	Decline: SD
Previous-year growth	0.2882	0.2443	0.2994	−0.5840	−0.5545	0.3538
Fertility (lag 1)	1.3849	1.3400	0.2158	1.4302	1.4400	0.1365
Japanese net migration rate (lag 1)	1.5075	1.2394	1.9405	−2.0643	−2.0292	1.5867
Dependency ratio (lag 1)	55.5278	54.6017	6.1009	69.7782	69.2047	8.7705


The Mann–Whitney U tests returned displayed p-values of 0.0 for all four features, including after FDR adjustment. These are rounded/display-underflow outputs and should be reported as very small p-values (or recomputed at higher display precision), not as mathematically exact zero. Furthermore, the tests treat observations as independent even though each prefecture appears repeatedly; they are exploratory group comparisons, not the final clustered panel inference.
G. Predictor correlation and multicollinearity
Predictor pair	Correlation
Previous-year growth vs fertility	−0.042
Previous-year growth vs migration	+0.822
Previous-year growth vs dependency	−0.792
Fertility vs migration	−0.273
Fertility vs dependency	+0.271
Migration vs dependency	−0.522


Predictor	VIF
Previous-year growth	13.037
Japanese net migration rate (lag 1)	6.271
Dependency ratio (lag 1)	5.310
Fertility (lag 1)	2.076


The strong correlations between previous-year growth, migration, and dependency help explain why coefficients can change in the multivariable logistic regression after prior growth is included. High VIF particularly cautions against treating individual coefficients as stable, independent effects. It does not automatically imply poor prediction, but it does make inference more difficult.
H. Inferential logistic regression
Two GLM binomial-logit models with year indicators and prefecture-clustered standard errors were reported, each with 940 observations.
Predictor	Model A coefficient	Model A clustered SE	Model A p	Model B coefficient	Model B clustered SE	Model B p
Fertility (lag 1)	−13.7788	3.5590	0.0001	−6.7473	3.2723	0.0392
Migration (lag 1)	−1.5340	0.2909	<0.001	−0.4171	0.4028	0.3003
Dependency (lag 1)	+0.5394	0.0906	<0.001	+0.3240	0.1314	0.0137
Previous-year growth	—	—	—	−10.0103	5.1599	0.0524


Model A Cox–Snell pseudo-R² was 0.4900. Selected odds ratios:
Predictor	Model	Odds ratio	95% CI for OR	p-value
Fertility	A	approximately 0.0000 (reported near 0)	[approximately 0, 0.0011]	0.0001
Migration	A	0.2157	[0.1220, 0.3814]	<0.001
Dependency	A	1.7149	[1.4359, 2.0481]	<0.001
Fertility	B	0.0012	[approximately 0, 0.7162]	0.0392
Migration	B	0.6589	[0.2992, 1.4510]	0.3003
Dependency	B	1.3827	[1.0687, 1.7890]	0.0137
Previous-year growth	B	approximately 0.000045	[approximately 0, 1.1085]	0.0524


Important interpretive issue: In the simple descriptive table, fertility is slightly higher among decline observations (mean 1.4302, median 1.44) than among no-decline observations (mean 1.3849, median 1.34). Yet the adjusted logistic regression coefficient for fertility is strongly negative. This reversal can arise from conditional adjustment, year effects, correlation among predictors, scaling, or separation/near-separation. It requires additional specification checks before a substantive interpretation. The large coefficient magnitudes and near-zero odds ratios also make rescaling and model-stability checks advisable.
I. Classification model performance, 2020–2025
The 2020–2025 temporal test set contains 282 observations: 265 declining and 17 non-declining (decline share 93.97%). The training set contained 658 observations and had a decline share of 80.40%.
Model	Threshold	Accuracy	Balanced accuracy	Recall: no decline	Recall: decline	F1: decline
Previous-year baseline	—	94.68%	83.41%	70.59%	96.23%	97.14%
Majority-class baseline (always decline)	—	93.97%	50.00%	0.00%	100.00%	96.89%
Logistic regression	0.50	92.20%	90.34%	88.24%	92.45%	95.70%
Logistic regression (tuned)	0.90	87.23%	93.21%	100.00%	86.42%	92.71%
Random forest	0.50	92.55%	85.03%	76.47%	93.58%	95.94%
Random forest (tuned)	0.81	90.78%	92.34%	94.12%	90.57%	94.86%


Thresholds were selected using 2016–2019 expanding-window validation. Threshold validation results were:
Model	Selected threshold	Validation balanced accuracy	Validation F1 (decline)
Logistic regression	0.90	0.9804	0.9800
Random forest	0.81	0.9837	0.9834


Confusion matrices (rows = actual [no decline, decline]; columns = predicted [no decline, decline]):

Previous-year baseline
[[ 12   5]
 [ 10 255]]
Majority-class baseline
[[  0  17]
 [  0 265]]
Logistic regression, threshold 0.50
[[ 15   2]
 [ 20 245]]
Random forest, threshold 0.50
[[ 13   4]
 [ 17 248]]
Logistic regression, tuned threshold 0.90
[[ 17   0]
 [ 36 229]]
Random forest, tuned threshold 0.81
[[ 16   1]
 [ 25 240]]
The persistence baseline has the highest overall accuracy, whereas tuned logistic regression has the highest overall balanced accuracy. The tuned logistic model correctly identifies all 17 non-declining observations but misses 36 declining observations. Tuned random forest identifies 16 of 17 non-declining observations and 240 of 265 declining observations. With only 17 non-decline cases in the whole test set, those minority-class metrics are inherently uncertain.

The per-year classification results also show the problem: the non-decline class count is only 1 in 2021, 2 in each of 2022–2024, and 4 in 2025. Annual balanced accuracy and minority recall can therefore swing sharply with one prediction.
Project progression
The research developed through the following stages:
1. Exploratory data work: inspect the national and prefectural population series, calculate/inspect growth and decline labels, assess missingness, and examine regional and annual trends.
2. Panel construction: focus on 47 prefectures and 2006–2025, producing the balanced 940-observation primary panel.
3. Demographic fixed-effects model: estimate fertility, migration, and dependency associations with prefecture and year effects; use clustered standard errors.
4. Diagnostics and robustness: inspect VIF, serial correlation, cross-sectional dependence, alternative standard errors, and exclusion of 2011/2025.
5. Economic extension: add income measures using the 2013–2022 common sample; assess whether the extra predictors improve inference or prediction.
6. Continuous forecasting: compare Ridge, random forest, and gradient boosting against persistence and historical-mean baselines under chronological evaluation.
7. Classification expansion: construct the binary decline target; verify labels; report class imbalance and group-wise feature differences.
8. Inferential logistic models: estimate conditional odds associations with year effects and prefecture-clustered standard errors; compare a demographic-only model with one adding prior-year growth.
9. Predictive classifiers and threshold selection: compare logistic regression and random forest with baselines; select thresholds using 2016–2019 validation and evaluate on 2020–2025.
10. Manuscript preparation: consolidate the results, cite the verified source tables, resolve remaining implementation inconsistencies, and write the discussion and limitations.
Reproducibility
Environment
I implemented the analysis in Python using:
- pandas
- numpy
- scipy
- statsmodels
- linearmodels (for panel fixed-effects estimation)
- scikit-learn
- matplotlib (for exploratory plots)
Pin the package versions used for the final analysis in a requirements or environment file (for example, with pip freeze > requirements-lock.txt).
Expected data objects and variables
The main analysis uses an in-memory DataFrame named analysis_df; forecasting cells use forecast_df. The main panel requires these columns:
Region Code
Region
Time
Population Growth Rate (%)
Population Decline
Total fertility rate (lag 1)
Japanese Net Migration Rate (lag 1)
Dependency Ratio (lag 1)
The economic analysis additionally requires the two lagged income columns documented above. Do not commit restricted or non-redistributable source files without checking their terms. A data dictionary and source manifest should accompany the raw data or document where readers can retrieve it.
Core formulas
If the source population series is an annual population count, the conventional growth-rate construction is:
population_growth_rate = (
    (population_t - population_t_minus_1) / population_t_minus_1
) * 100

population_decline = (population_growth_rate < 0).astype(int)
Use the population concept, reference date, and denominator specified by the underlying source series. The binary-label audit verifies consistency between the growth-rate column and the decline indicator; independently reproducing the growth-rate calculation requires the original population series and preprocessing steps.

A one-year lag within prefecture is conventionally constructed as:
df = df.sort_values(["Region Code", "Time"]).copy()
df["Previous-Year Growth Rate"] = (
    df.groupby("Region Code")["Population Growth Rate (%)"].shift(1)
)
Apply the same prefecture-level grouped shift to each demographic predictor.
Reproduction cautions
- The main regression sample contains 940 observations (47 × 20).
- Some forecasting experiments report 611 training observations because the lagged growth feature causes the first year to be unavailable, followed by 282 test observations.
- The classification split contains 658 training and 282 test observations, totalling 940. If groupby(...).shift(1) is applied only after restricting the source to 2006–2025, the first year for each prefecture will have no prior-year feature and should be dropped. Construct the lag before restricting the panel to 2006–2025. This requires 2005 prefecture observations to supply the 2006 lag; otherwise, drop the first year for each prefecture and update the sample size accordingly.
- I define feature lists explicitly in each modelling stage so the models can be rerun independently.
- A separate two-way fixed-effects implementation reports R² 0.7931, while the main PanelOLS summary reports R² 0.2456 and within R² 0.6028. These values appear to use different implementations or R² definitions. I use the PanelOLS estimates as the main reported specification. The two R² values should not be compared until their definitions are reconciled.
- The preprocessing scripts and notebooks are the reproducibility record for the tables, figures, and model results.
- Displayed Mann–Whitney p-values of 0.0000 are rounded values, not mathematically exact zero.
- The wider exploratory dataset, primary prefectural panel, economic-feature subset, and forecasting/classification samples have different coverage and observation counts. Keep each result attached to its sample definition and do not combine statistics across these datasets.
Interpretation and limitations
1. Observational design: fixed effects and lagged predictors improve the structure of the analysis but do not remove all time-varying confounding. The coefficients are associations, not causal effects.
2. Population-growth definition: exact population count series, coverage, denominator, and annual reference date must be verified. Administrative population data, census counts, and estimates may not be interchangeable.
3. Source documentation: the underlying data series have been checked against their original official releases. Maintain the exact source URLs, table IDs, release dates, definitions, and source-to-variable mappings in the repository data documentation so that each analytical variable can be traced directly to its source.
4. Missingness and sample selection: economic variables cover a shorter period. Complete-case samples can differ from the full panel, making coefficients across samples not directly comparable.
5. Multicollinearity: previous-year growth is highly correlated with migration and dependency, and has VIF 13.037. Coefficient shifts in multivariable models should be interpreted cautiously.
6. Cross-sectional dependence: the Pesaran CD test rejects the null at 5% in the reported full-panel test. Driscoll–Kraay or other appropriate inference sensitivity should be documented consistently.
7. Serial dependence: different model/sample diagnostics yield different conclusions. Report the test, sample, and specification alongside every diagnostic result.
8. Fertility-logit sign reversal: descriptive group differences and adjusted logistic regression point in different directions. Do not claim a simple protective fertility effect on the decline outcome until the specification, scale, year effects, and separation are checked.
9. Class imbalance: 265 of 282 test observations are decline cases. Accuracy is insufficient on its own, and metrics for the 17 non-decline cases are unstable.
10. Persistence baseline: previous-year status is a strong one-step-ahead comparator. It is not equivalent to forecasting all six years from a single 2019 information set; define its operational use clearly.
11. Repeated test inspection: the 2020–2025 period was inspected during model development. Its results are exploratory temporal evaluation, not a pristine final holdout.
12. Model-selection uncertainty: overlapping validation exercises and choosing models after comparing several windows can make apparent gains optimistic. An additional untouched future period or nested time-series validation would strengthen the study.
13. External validity: results describe the observed Japanese prefectural panel and should not automatically be generalized to other countries, administrative units, or future regimes.
Repository structure
.
├── README.md
├── data/
│   ├── README.md              # Source manifest, definitions, and retrieval steps
│   └── processed/             # Only data that may legally be redistributed
├── notebooks/
│   ├── 01_data_audit.ipynb
│   ├── 02_panel_regression.ipynb
│   ├── 03_forecasting.ipynb
│   └── 04_decline_classification.ipynb
├── src/                       # Reusable analysis functions/scripts
├── results/
│   ├── tables/
│   └── figures/
├── requirements.txt
└── CITATION.cff
I keep source data documentation, analysis code, generated outputs, and citation metadata in separate directories.
References and citation status
I verified the official Japanese statistical sources used to construct the dataset. The source documentation records the release, table or series, and variables derived from each source.
- e-Stat, Portal Site of Official Statistics of Japan — official statistical tables used for the relevant population, demographic, and regional series.
- Statistics Bureau of Japan — population and prefectural statistics used in the dataset, as applicable to each variable.
- National Institute of Population and Social Security Research (IPSS) — demographic series used where applicable.
- Ministry of Justice — migration-related statistics used where applicable.
- Wooldridge, Econometric Analysis of Cross Section and Panel Data (2002).
- Pesaran (2004), “General Diagnostic Tests for Cross Section Dependence.”
- Jarque and Bera (1980), “Efficient Tests for Normality, Homoscedasticity and Serial Independence of Regression Residuals,” Economics Letters (include only if this test is retained in the final analysis).
- Official documentation for pandas, statsmodels, linearmodels, and scikit-learn — cite the specific versions used if software citations are required.
For the final release, connect each source citation to the variables derived from it and include stable URLs or table identifiers wherever available. Keep data citations distinct from methodological references.
How to cite this project
A CITATION.cff file can be added for the repository release metadata and preferred citation format.
Repository scope
This repository contains the data preparation workflow, statistical models, forecasting and classification experiments, and results for the study of prefecture-level population change in Japan.
