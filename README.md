# quantile-decomposition-KLoSA
Code by Katherine Ford for doi: 10.1177/23337214211004366

Please cite this paper when using the code: Ford, K. J., & Leist, A. K. (2021). Returns to educational and occupational attainment in cognitive performance for middle-aged South Korean men and women. Gerontology and Geriatric Medicine, 7, 23337214211004366.

## Data availability
The data analyzed here is publicly available from the Korea Employment Information Service (https://survey.keis.or.kr/eng/).

## Funding
KJF’s doctoral training is supported by the Luxembourg National Research Fund (FNR) under grant no. 10949242. This project has also received funding from the European Research Council (ERC) under the European Union’s Horizon 2020 research and innovation programme (grant agreement
no. 803239).


## Main Code Details
We use data from the Wave 1 main file and the Wave 1 imputation file. Household income is the only variable used from the imputation file. The most recent data files available on December 9th, 2019, were downloaded that day. 

### Variable Descriptions
`w01mmse` MMSE score

`educ` Categories of education level recoded from `w01A003` in the original main file

`education1 – education5` Education categories operationalized as multiple binary variables as the `cdeco` command does not allow the prefix `i.`

`collar` Categories of occupational class recoded from `w01D109occ` in the original main file. Missing values are replaced by additional questions on occupational class (`w01D314occ`, `w01D407occ`, `w01D519occ`, `w01D710occ`, and `w01D610occ_p` in the original main file)

`occcollar1 – occcollar6` Occupational categories operationalized as multiple binary variables as the `cdeco` command does not allow the prefix `i.`

`work` Currently working, renaming of `w01D001` in the original main file

`lnhhinc` Natural logarithm of household adjusted income, the natural logarithm of the average of 5 imputations of the household income (`w01hhinc1` through `w01hhinc5` in the original imputation file) adjusted for household size (`w01hhsize` in the original main file) 

`mar` Married or living with a partner, recoding of `w01A006` in the original main file

`obese` Obesity (BMI >= 30 kg/m2), recoding of `w01bmi` in the original main file

`hiBP` High blood pressure, renaming of `w01C006` in the original main file

`hearloss` Daily difficulty with hearing, renaming of `w01C084` in the original main file

`agescld` Age in years, scaled at 45 years (the youngest age for eligibility in KLoSA), recoding of `w01A001_age` in the original main file

`agescld_sq` Squaring of age scaled at 45 years

`kwar` Born before or during the Korean War, recoding of the year of respondent’s birth or `w01year1` in the original main file

`female` Gender, renaming of `w01gender1` in the original main file

### Quantile Regression
`qreg w01mmse i.educ i.collar work lnhhinc mar obese hiBP hearloss agescld agescld_sq kwar female, q(0.1)`  

Replace `0.1` in the brackets with other numbers to indicate desired decile output.
### Quantile Regression Decomposition (Full Sample)
`cdeco w01mmse education2-education5 occcollar2-occcollar6 work lnhhinc mar obese hiBP hearloss agescld agescld_sq kwar, group(female) reps(1000)`

Repeat command with just the three exposures of interest, then just the two age variables, and finally with just the remaining covariates to isolate the drivers of the cognitive performance gap between genders.
### Logistic Regression for Restricting Sample
`logit female education2-education5 occcollar2-occcollar6 work lnhhinc mar obese hiBP hearloss agescld agescld_sq kwar`

`predict p`
### Quantile Regression Decomposition (Restricted Sample)
`cdeco w01mmse education2-education5 occcollar2-occcollar6 work lnhhinc mar obese hiBP hearloss agescld agescld_sq kwar if p>=0.1 & p<=0.9, group(female) reps(1000)`

See https://sites.google.com/site/blaisemelly/home/computer-programs/inference-on-counterfactual-distributions for `cdeco` code instructions. For more details on methods implemented by the `cdeco` command, see the following article: Chernozhukov, V., Fernandez-Val, I., and Melly, B. (2013). Inference on Counterfactual Distributions. *Econometrica* 81(6):2205–2268.
