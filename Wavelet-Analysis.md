---
title: "Wavelet Analysis"
author: "Jen Baron, j.baron@alumni.ubc.ca, UBC Tree Ring Lab"
date: "Juen 5, 2020"
output:
  html_document:
    keep_md: yes
    theme: flatly
    number_sections: yes
    toc: yes
    toc_float: yes
---



# Packages

```r
library(dplR)
library(forecast)
```

```
## Registered S3 method overwritten by 'quantmod':
##   method            from
##   as.zoo.data.frame zoo
```

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

# Load Data

```r
pinery.STD <- read.crn("outputs/pinery.STD", header=NULL)
```

```
## There does not appear to be a header in the crn file
## There is 1 series
```

```r
pinery.RES <- read.crn("outputs/pinery.RES", header=NULL)
```

```
## There does not appear to be a header in the crn file
## There is 1 series
```


```r
years <- as.numeric(rownames(pinery.STD))
pinery.STD.0 <- pinery.STD[, 1]
na.omit(pinery.RES) -> pinery.RES
years2 <- as.numeric(rownames(pinery.RES))
pinery.RES.0 <- pinery.RES[, 1]
```

# Autocorrelation

We will start our analysis on the chronology by looking at its autocorrelation structure using R’s acf and pacf functions.


```r
op <- par(no.readonly = TRUE) # Save to reset on exit
par(mfcol=c(1, 2))
acf(pinery.STD.0)
pacf(pinery.STD.0)
```

![](Wavelet-Analysis_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
par(op)
```

The ACF function indicates significant autocorrelation out to a lag of about 5 years (which is not uncommon in tree-ring data) while the PACF plot suggests that the autocorrelation drops off after lag 1. 

We now have the first bit of solid information about the time-series properties of these data, it looks like they fit an AR(1) model.


The easiest way is to use the ar function which fits an autoregressive model and selects the order by AIC.


```r
pinery.ar <- ar(pinery.STD.0)
pinery.ar
```

```
## 
## Call:
## ar(x = pinery.STD.0)
## 
## Coefficients:
##      1  
## 0.6293  
## 
## Order selected 1  sigma^2 estimated as  0.03543
```


Indeed, ar produces an AR(1) model. We can do the same sort of analysis by automatically fitting an ARMA model using the auto.arima function in the package "forecast".


```r
pinery.arima <- auto.arima(pinery.STD.0, ic="bic")
summary(pinery.arima)
```

```
## Series: pinery.STD.0 
## ARIMA(0,1,1) 
## 
## Coefficients:
##           ma1
##       -0.3125
## s.e.   0.1021
## 
## sigma^2 estimated as 0.03341:  log likelihood=34.11
## AIC=-64.23   AICc=-64.13   BIC=-58.65
## 
## Training set error measures:
##                     ME      RMSE       MAE       MPE     MAPE      MASE
## Training set 0.0119641 0.1812632 0.1476532 -1.592693 15.79989 0.9592019
##                    ACF1
## Training set 0.01830801
```

```r
head(residuals(pinery.arima))
```

```
## Time Series:
## Start = 1 
## End = 6 
## Frequency = 1 
## [1]  0.0008669995 -0.0028631781  0.1186317116 -0.1310320127  0.3160561942
## [6]  0.1207643328
```

```r
coef(pinery.arima)
```

```
##        ma1 
## -0.3125041
```

```r
acf(residuals(pinery.arima),plot=FALSE)
```

```
## 
## Autocorrelations of series 'residuals(pinery.arima)', by lag
## 
##      0      1      2      3      4      5      6      7      8      9     10 
##  1.000  0.018 -0.033 -0.090 -0.262  0.076 -0.035  0.058  0.041 -0.077 -0.002 
##     11     12     13     14     15     16     17     18     19     20 
## -0.017  0.148 -0.037 -0.096  0.068 -0.044  0.077 -0.119 -0.149  0.001
```


Instead of an AR(1) model, auto.arima went for an ARMA(0,1,1) model. The parsimony principle certainly likes a nice simple ARMA(1,1) model. Note that we could look at the residuals (just the first few), model coefficients, etc. quite easily. And indeed the residuals are quite clean as we would expect.




# Standard Morlet wavelet transform analysis

## St


```r
out.wave <- morlet(y1 = pinery.STD.0, x1 = years, p2 = 6, dj = 0.1,
                   siglvl = 0.95)
wavelet.plot(out.wave, useRaster = NA)
```

![](Wavelet-Analysis_files/figure-html/unnamed-chunk-7-1.png)<!-- -->




```r
out.wave2 <- morlet(y1 = pinery.RES.0, x1 = years2, p2 = 6, dj = 0.1,
                   siglvl = 0.95)
wavelet.plot(out.wave2, useRaster = NA)
```

![](Wavelet-Analysis_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

# Cross-wavelet Analysis

For cross-wavelet analysis (chronology vs another index such as the PDO index) I used the "WaveletComp" package in R.




# Reproducibility

```r
citation("dplR")
```

```
## 
## Bunn AG (2008). "A dendrochronology program library in R (dplR)."
## _Dendrochronologia_, *26*(2), 115-124. ISSN 1125-7865, doi:
## 10.1016/j.dendro.2008.01.002 (URL:
## https://doi.org/10.1016/j.dendro.2008.01.002).
## 
## Bunn AG (2010). "Statistical and visual crossdating in R using the dplR
## library." _Dendrochronologia_, *28*(4), 251-258. ISSN 1125-7865, doi:
## 10.1016/j.dendro.2009.12.001 (URL:
## https://doi.org/10.1016/j.dendro.2009.12.001).
## 
##   Andy Bunn, Mikko Korpela, Franco Biondi, Filipe Campelo, Pierre
##   Mérian, Fares Qeadan and Christian Zang (2020). dplR:
##   Dendrochronology Program Library in R. R package version 1.7.1.
##   https://CRAN.R-project.org/package=dplR
## 
## To see these entries in BibTeX format, use 'print(<citation>,
## bibtex=TRUE)', 'toBibtex(.)', or set
## 'options(citation.bibtex.max=999)'.
```

```r
Sys.time()
```

```
## [1] "2020-06-05 15:42:49 PDT"
```

```r
git2r::repository()
```

```
## Local:    master /Users/JenBaron/Documents/UWO/UWO NSERC/Statistical Analysis/Dendrochronology/Dendrochronology in R/Dendrochronology_in_R
## Remote:   master @ origin (https://github.com/JenBaron/Dendrochronology_in_R.git)
## Head:     [f63e16a] 2020-06-02: Residual chronology
```

```r
sessionInfo()
```

```
## R version 4.0.0 (2020-04-24)
## Platform: x86_64-apple-darwin17.0 (64-bit)
## Running under: macOS Catalina 10.15.3
## 
## Matrix products: default
## BLAS:   /Library/Frameworks/R.framework/Versions/4.0/Resources/lib/libRblas.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/4.0/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] en_CA.UTF-8/en_CA.UTF-8/en_CA.UTF-8/C/en_CA.UTF-8/en_CA.UTF-8
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] dplyr_0.8.5   forecast_8.12 dplR_1.7.1   
## 
## loaded via a namespace (and not attached):
##  [1] zoo_1.8-7          tidyselect_1.0.0   xfun_0.13          purrr_0.3.4       
##  [5] urca_1.3-0         lattice_0.20-41    colorspace_1.4-1   vctrs_0.2.4       
##  [9] htmltools_0.4.0    yaml_2.2.1         XML_3.99-0.3       rlang_0.4.5       
## [13] R.oo_1.23.0        pillar_1.4.3       glue_1.4.0         R.utils_2.9.2     
## [17] TTR_0.23-6         matrixStats_0.56.0 lifecycle_0.2.0    plyr_1.8.6        
## [21] quantmod_0.4.17    stringr_1.4.0      timeDate_3043.102  munsell_0.5.0     
## [25] gtable_0.3.0       R.methodsS3_1.8.0  evaluate_0.14      knitr_1.28        
## [29] tseries_0.10-47    lmtest_0.9-37      parallel_4.0.0     curl_4.3          
## [33] xts_0.12-0         Rcpp_1.0.4.6       scales_1.1.0       fracdiff_1.5-1    
## [37] ggplot2_3.3.0      png_0.1-7          digest_0.6.25      stringi_1.4.6     
## [41] grid_4.0.0         quadprog_1.5-8     tools_4.0.0        magrittr_1.5      
## [45] tibble_3.0.1       crayon_1.3.4       pkgconfig_2.0.3    MASS_7.3-51.6     
## [49] ellipsis_0.3.0     Matrix_1.2-18      assertthat_0.2.1   rmarkdown_2.1     
## [53] R6_2.4.1           git2r_0.26.1       nlme_3.1-147       signal_0.7-6      
## [57] nnet_7.3-14        compiler_4.0.0
```
