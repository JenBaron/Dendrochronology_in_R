---
title: "Dendrochronology in R"
author: "Jen Baron, j.baron@alumni.ubc.ca, UBC Tree Ring Lab"
date: 'May 8, 2020'
output:
  html_document:
    keep_md: yes
    number_sections: yes
    theme: sandstone
    toc: yes
    toc_float: yes
---

# Packages


```r
library(dplR) # dendrochronology
library(tidyr)
library(ggplot2)
```

# Load Data


```r
pinery.rwl <- read.rwl("data/pinery.rwl")
```

```
## Attempting to automatically detect format.
## Assuming a Tucson format file.
## There appears to be a header in the rwl file
## There are 12 series
## 1    	P1809Ae 	 1946	 2017	0.001
## 2    	P1809Be 	 1892	 2017	0.001
## 3    	P1809Ce 	 1888	 2017	0.001
## 4    	P1810Ae 	 1853	 2018	0.001
## 5    	P1810Be 	 1907	 2017	0.001
## 6    	P1810Ce 	 1898	 2017	0.001
## 7    	P1811Ae 	 1891	 2017	0.001
## 8    	P1811Be 	 1887	 2017	0.001
## 9    	P1811Ce 	 1889	 2017	0.001
## 10   	P1812Ae 	 1889	 2018	0.001
## 11   	P1812Be 	 1906	 2017	0.001
## 12   	P1812De 	 1905	 2017	0.001
```

```r
#master_all.rwl <- read.rwl("data/master_all.rwl")
```


```r
rwl.report(pinery.rwl)
```

```
## Number of dated series: 12 
## Number of measurements: 1467 
## Avg series length: 122.25 
## Range:  166 
## Span:  1853 - 2018 
## Mean (Std dev) series intercorrelation: 0.4705409 (0.08457867)
## Mean (Std dev) AR1: 0.81975 (0.1065681)
## -------------
## Years with absent rings listed by series 
##     None 
## -------------
## Years with internal NA values listed by series 
##     None
```

## Basic Summary Statistics


```r
rwl.stats(pinery.rwl)
```

```
##     series first last year  mean median stdev  skew  gini   ar1
## 1  P1809Ae  1946 2017   72 1.261  1.215 0.415 0.758 0.181 0.642
## 2  P1809Be  1892 2017  126 2.074  1.894 1.119 1.197 0.288 0.731
## 3  P1809Ce  1888 2017  130 1.861  1.315 1.610 2.616 0.368 0.908
## 4  P1810Ae  1853 2018  166 1.564  0.861 2.062 3.789 0.501 0.697
## 5  P1810Be  1907 2017  111 1.437  1.316 1.012 3.582 0.300 0.710
## 6  P1810Ce  1898 2017  120 1.585  1.276 1.143 2.899 0.312 0.829
## 7  P1811Ae  1891 2017  127 1.760  0.804 1.997 1.656 0.535 0.937
## 8  P1811Be  1887 2017  131 2.092  1.037 2.257 1.574 0.529 0.951
## 9  P1811Ce  1889 2017  129 2.109  1.228 2.117 1.597 0.491 0.950
## 10 P1812Ae  1889 2018  130 1.303  1.035 0.922 2.228 0.333 0.864
## 11 P1812Be  1906 2017  112 0.957  0.614 0.892 2.125 0.406 0.822
## 12 P1812De  1905 2017  113 0.825  0.745 0.497 2.902 0.270 0.796
```


```r
seg.plot(pinery.rwl)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

## Truncate Based on Date / Sample Depth

Truncate to 1898 (sample depth = 8)


```r
pinery.rwl.trunc <- pinery.rwl[46:166,]
```



# Crossdating



```r
pinery.corr <- corr.rwl.seg(pinery.rwl.trunc, seg.length = 50, 1918)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


```r
#identify flags
flags <- as.data.frame(pinery.corr$flags) 
flags
```

```
## [1] pinery.corr$flags
## <0 rows> (or 0-length row.names)
```

```r
#correlation by series
overall <- pinery.corr$overall
overall %>% round(2)
```

```
##          rho p-val
## P1809Ae 0.60     0
## P1809Be 0.46     0
## P1809Ce 0.52     0
## P1810Ae 0.41     0
## P1810Be 0.58     0
## P1810Ce 0.50     0
## P1811Ae 0.45     0
## P1811Be 0.46     0
## P1811Ce 0.52     0
## P1812Ae 0.53     0
## P1812Be 0.47     0
## P1812De 0.61     0
```

```r
#average series correlation
avg.seg.rho <- as.data.frame(pinery.corr$avg.seg.rho)
avg.seg.rho %>% round(2)
```

```
##           pinery.corr$avg.seg.rho
## 1918.1967                    0.54
## 1943.1992                    0.53
## 1968.2017                    0.51
```

```r
#correlation by segment and series
rho <- as.data.frame(pinery.corr$spearman.rho)
rho %>% round(2)
```

```
##         1918.1967 1943.1992 1968.2017
## P1809Ae        NA        NA      0.69
## P1809Be      0.40      0.37      0.55
## P1809Ce      0.67      0.37      0.54
## P1810Ae      0.49      0.45      0.43
## P1810Be      0.65      0.73      0.62
## P1810Ce      0.61      0.58      0.37
## P1811Ae      0.34      0.49      0.56
## P1811Be      0.50      0.60      0.37
## P1811Ce      0.44      0.49      0.42
## P1812Ae      0.55      0.63      0.64
## P1812Be      0.51      0.50      0.42
## P1812De      0.73      0.66      0.56
```

There is (at least) one other way of looking at the average interseries
correlation of a data set. The interseries.cor function in dplR gives
a measure of average interseries correlation that is dierent from the rbar
measurements from rwi.stats. In this function, correlations are calculated
serially between each tree-ring series and a master chronology built from
all the other series in the rwl object (leave-one-out principle).


```r
interseries.cor(pinery.rwl) %>% round(2)
```

```
## Warning in cor.test.default(series[, i], master[, i], method = method2, : Cannot
## compute exact p-value with ties
```

```
##         res.cor p.val
## P1809Ae    0.59     0
## P1809Be    0.39     0
## P1809Ce    0.50     0
## P1810Ae    0.35     0
## P1810Be    0.57     0
## P1810Ce    0.48     0
## P1811Ae    0.45     0
## P1811Be    0.50     0
## P1811Ce    0.39     0
## P1812Ae    0.37     0
## P1812Be    0.47     0
## P1812De    0.60     0
```



# Detrending

 Detrend all series (with ModNegExp)

```r
pinery.rwi <- detrend(rwl = pinery.rwl.trunc, method = "ModNegExp")
#pinery.rwi$model.info
```



## Descriptive Statistics


```r
pinery.ids <- read.ids(pinery.rwl.trunc)
pinery.ids
```

```
##         tree core
## P1809Ae    9    1
## P1809Be    9    2
## P1809Ce    9    3
## P1810Ae   10    1
## P1810Be   10    2
## P1810Ce   10    3
## P1811Ae   11    1
## P1811Be   11    2
## P1811Ce   11    3
## P1812Ae   12    1
## P1812Be   12    2
## P1812De   12    3
```


```r
rwi.stats(pinery.rwi, pinery.ids, prewhiten = TRUE)
```

```
##   n.cores n.trees    n n.tot n.wt n.bt rbar.tot rbar.wt rbar.bt c.eff rbar.eff
## 1      12       4 3.95    66   12   54    0.325   0.399   0.309     3    0.516
##     eps   snr
## 1 0.808 4.204
```


# Building a Mean Value Chronology


```r
pinery.crn <- chron(pinery.rwi, prewhiten=FALSE)
str(pinery.crn)
```

```
## Classes 'crn' and 'data.frame':	121 obs. of  2 variables:
##  $ xxxstd    : num  0.867 0.864 0.984 0.816 1.173 ...
##  $ samp.depth: num  8 8 8 8 8 8 8 9 10 11 ...
```

```r
pinery.crn$samp.depth <- NULL
```

## Plot Chronology


```r
crn.plot(pinery.crn, add.spline = TRUE, nyrs=15)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


## Add Uncertainty Estimates


```r
pinery.avg <- apply(pinery.rwi,1,mean,na.rm=TRUE)

yrs <- time(pinery.crn)


se <- function(x){
  x2 <- na.omit(x)
  n <- length(x2)
  sd(x2)/sqrt(n)}


pinery.se <- apply(pinery.rwi,1,se)

pinery.sd <- apply(pinery.rwi,1,sd,na.rm=TRUE)

par(mar = c(2, 2, 2, 2), mgp = c(1.1, 0.1, 0), tcl = 0.25, xaxs='i')

plot(yrs, pinery.avg, type = "n", xlab = "Year", ylab = "RWI", axes=FALSE)
xx <- c(yrs,rev(yrs))
yy <- c(pinery.avg+pinery.se*2,rev(pinery.avg-pinery.se*2))
polygon(xx,yy,col="grey80",border = NA)
lines(yrs, pinery.avg, col = "black")
#lines(yrs, ffcsaps(pinery.avg, nyrs = 15), col = "red", lwd = 2) #adds a spline
axis(1); axis(2); axis(3); axis(4)
box()
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


# Conclude Session


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
## [1] ggplot2_3.3.0 tidyr_1.0.2   dplR_1.7.1   
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.4.6       compiler_4.0.0     pillar_1.4.3       plyr_1.8.6        
##  [5] iterators_1.0.12   R.methodsS3_1.8.0  R.utils_2.9.2      tools_4.0.0       
##  [9] digest_0.6.25      gtable_0.3.0       evaluate_0.14      tibble_3.0.1      
## [13] lifecycle_0.2.0    lattice_0.20-41    pkgconfig_2.0.3    png_0.1-7         
## [17] rlang_0.4.5        foreach_1.5.0      Matrix_1.2-18      yaml_2.2.1        
## [21] xfun_0.13          withr_2.2.0        stringr_1.4.0      dplyr_0.8.5       
## [25] knitr_1.28         vctrs_0.2.4        grid_4.0.0         tidyselect_1.0.0  
## [29] glue_1.4.0         R6_2.4.1           XML_3.99-0.3       rmarkdown_2.1     
## [33] purrr_0.3.4        magrittr_1.5       codetools_0.2-16   scales_1.1.0      
## [37] matrixStats_0.56.0 htmltools_0.4.0    ellipsis_0.3.0     MASS_7.3-51.6     
## [41] assertthat_0.2.1   colorspace_1.4-1   stringi_1.4.6      munsell_0.5.0     
## [45] signal_0.7-6       crayon_1.3.4       R.oo_1.23.0
```

