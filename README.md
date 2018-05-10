
# childhoodmortality
Tools to calculate childhood mortality rates (neonatal, postneonatal, infant, child, under-five) using DHS data. 

Install this package using the following command: ```install.packages("childhoodmortality")```

Alternatively: 
```install.packages("devtools")```

```devtools::install_github("caseybreen/childhoodmortality")```
### Overview

The *childhoodmortality* package offers a straightforward approach to computing childhood mortality rates. The package was developed in accordance with the “Methodology of DHS Mortality Rates Estimation” section of the DHS Guide to Statistics (Rutstein 2006:90-95). Specifically, the package uses a synthetic cohort life table approach, combining mortality probabilities for age segments with actual cohort mortality experience. The `childhoodmortality` package defaults to the DHS Program’s practice of calculating mortality rates for five-year periods preceding the start date of the survey. By adhering to [DHS Guidelines](https://dhsprogram.com/pubs/pdf/DHSG1/Guide_to_DHS_Statistics_29Oct2012_DHSG1.pdf), estimates produced from the package can be compared to those published in the DHS Final Reports.

DHS surveys are conducted using a multi-stage stratified design, so standard sampling error formulae for simple random samples cannot be applied. For mortality rates, the DHS Program uses a jackknife repeated replication approach outlined in Appendix C of the DHS Final Reports [DHS Final Reports](https://dhsprogram.com/pubs/pdf/FR293/FR293.pdf) (Rutstein 2006). This resampling technique systematically omits a single cluster from the dataset, replicates the mortality rate estimate, repeats this replication for every cluster, and then uses the mortality rates computed in the replications to calculate standard errors. This approach controls for sample design. The *childhoodmortality* package computes standard errors for the mortality rate type specified.


### Using the childhoodmortality package

The three required arguments for the primary function childhoodmortality() are:

- __data__: the data frame containing the IPUMS-DHS microdata (or DHS data with column names renamed to match IPUMS-DHS). Six variables, available in all IPUMS-DHS datasets, are necessary to compute child mortality:

    * `KIDDOCCMC`, reporting the date of birth of the child in century month code

    * `KIDAGEDIEDIMP`, reporting the age of the child at death in months

    * `INTDATECMC`, reporting interview date in century month code

    * `YEAR`, reporting the year the survey was fielded

    * `PSU`, reporting the primary sampling unit
  
    * `PERWEIGHT`, reporting the individual weights assigned to each woman in the survey


- __grouping__: a categorical variable in data which the mortality rates will be disaggregated (e.g. IPUMS-DHS integrated geography variables, wealth quintile, race/ethnicity variables, etc.)

    
- __rate_type__: the type of mortality rate to be computed: 

    * Neonatal: probability of dying within 0-30 days of birth
    
    * Postneonatal: probability of dying within 30-365 days of birth
    
    * Infant: probability of dying within 0-365 days of birth
    
    * Child: probability of dying within 1-5 years of birth
    
    * Under five: probability of dying within 5 years of birth
    

### Input Data

The variable names must match the variable names in IPUMS-DHS. If data is obtained directly from the DHS program, column names must be renamed to match IPUMS-DHS. Head of example input data: 

| YEAR| WEALTHQ| PSU| PERWEIGHT| KIDDOBCMC| INTDATECMC| KIDAGEDIEDIMP|
|----:|-------:|---:|---------:|---------:|----------:|-------------:|
| 2015|       1|  53|  2.097505|      1323|       1387|            NA|
| 2015|       1|  36|  0.743239|      1381|       1387|             0|
| 2015|       2| 190|  1.063310|      1371|       1389|            NA|
| 2015|       2|  21|  1.729982|      1375|       1387|            NA|
| 2015|       1|  82|  0.875351|      1385|       1386|            NA|
| 2015|       2| 159|  0.940895|      1337|       1388|            NA|


This dataframe includes the 6 variables necessary for computing childhoodmortality rates and includes the grouping variable `CHILDHOODMORTALITY`. The package only needs to be installed once, but it must be reloaded every time a new session is started. 

### Installing the childhoodmortality package

The childhoodmortality package in on the Comprehensive R Archive Network (CRAN). This makes installation straightforward:   
  
    
```{r, eval = FALSE}
install.packages("childhoodmortality")
library(childhoodmortality)

```

Alternatively, install through github: 
  
  
```{r, eval = FALSE}
install.packages("devtools")
devtools::install_github("caseybreen/childhoodmortality")

```
  
    
### Childhoodmortality function

The call to the `childhoodmortality` function is as follows:  



```{r, eval = FALSE}  
underfive_mortality_rates <- childhoodmortality(
 data = model_ipums_dhs_dataset,
 grouping ="WEALTHQ",
 rate_type = "underfive"
)

```


| WEALTHQ| underfive|       SE| Lower_confidence_interval| Upper_confidence_interval|
|-------:|---------:|--------:|-------------------------:|-------------------------:|
|       1|  102.5227| 21.17584|                  60.17105|                  144.8744|
|       2|  133.4626| 32.65866|                  68.14528|                  198.7799|  

  
The `childhoodmortality` function returns a data frame containing:: 

- Unique values of the categorical disaggregation variable (e.g. region)

- Subpopulation estimates of the mortality rates specified in the `rate_type` argument 

- Standard errors for each subpopulation estimate

- Lower and upper bounds of the 95% confidence interval (rate +/- 2 SEs)




