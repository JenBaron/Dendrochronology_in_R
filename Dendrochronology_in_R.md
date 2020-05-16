---
title: "Dendrochronology in R"
author: "Jen Baron, j.baron@alumni.ubc.ca, UBC Tree Ring Lab"
date: "May 15, 2020"
output:
  html_document:
    keep_md: yes
    theme: flatly
    number_sections: yes
    toc: yes
    toc_float: yes
---


```r
knitr::opts_chunk$set(warning=FALSE, message=FALSE)
```

# Packages


```r
library(dplR) # dendrochronology
library(dplyr)
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
master_all.rwl <- read.rwl("data/master_all.rwl")
```

```
## Attempting to automatically detect format.
## Assuming a Tucson format file.
## There does not appear to be a header in the rwl file
## There are 119 series
## 1    	058021  	 1679	 1981	0.01
## 2    	058022  	 1721	 1981	0.01
## 3    	058031  	 1702	 1939	0.01
## 4    	058032  	 1689	 1939	0.01
## 5    	058051  	 1720	 1956	0.01
## 6    	058052  	 1693	 1940	0.01
## 7    	058061  	 1685	 1967	0.01
## 8    	058062  	 1687	 1968	0.01
## 9    	058071  	 1700	 1970	0.01
## 10   	058072  	 1691	 1970	0.01
## 11   	058081  	 1706	 1970	0.01
## 12   	058082  	 1693	 1967	0.01
## 13   	058101  	 1693	 1970	0.01
## 14   	058102  	 1698	 1970	0.01
## 15   	058121  	 1696	 1981	0.01
## 16   	058122  	 1711	 1981	0.01
## 17   	058141  	 1712	 1981	0.01
## 18   	058142  	 1699	 1981	0.01
## 19   	058151  	 1705	 1970	0.01
## 20   	058152  	 1692	 1981	0.01
## 21   	058191  	 1685	 1968	0.01
## 22   	058201  	 1730	 1940	0.01
## 23   	058202  	 1684	 1981	0.01
## 24   	058211  	 1749	 1979	0.01
## 25   	dlw01a  	 1764	 1994	0.01
## 26   	dlw02a  	 1716	 1994	0.01
## 27   	dlw03a  	 1762	 1994	0.01
## 28   	dlw07a  	 1889	 1994	0.01
## 29   	dlw08a  	 1788	 1994	0.01
## 30   	dlw10a  	 1798	 1994	0.01
## 31   	dlw11a  	 1741	 1994	0.01
## 32   	dlw12a  	 1704	 1994	0.01
## 33   	dlw13a  	 1743	 1994	0.01
## 34   	dlw14a  	 1662	 1994	0.01
## 35   	dlw16a  	 1676	 1991	0.01
## 36   	dlw15a  	 1830	 1994	0.01
## 37   	dlw17a  	 1730	 1992	0.01
## 38   	dlw18b  	 1777	 1994	0.01
## 39   	dlw19b  	 1862	 1994	0.01
## 40   	dlw20b  	 1805	 1994	0.01
## 41   	dlw23b  	 1762	 1994	0.01
## 42   	dlw24b  	 1775	 1994	0.01
## 43   	dlw25b  	 1777	 1994	0.01
## 44   	dlw26b  	 1790	 1994	0.01
## 45   	dlw27b  	 1757	 1994	0.01
## 46   	dlw029  	 1800	 1994	0.01
## 47   	dlw01b  	 1770	 1994	0.01
## 48   	dlw03b  	 1762	 1994	0.01
## 49   	dlw05a  	 1773	 1994	0.01
## 50   	dlw06a  	 1757	 1994	0.01
## 51   	dlw07b  	 1880	 1994	0.01
## 52   	dlw08b  	 1766	 1994	0.01
## 53   	dlw11c  	 1687	 1994	0.01
## 54   	dlw12b  	 1665	 1992	0.01
## 55   	dlw14b  	 1669	 1994	0.01
## 56   	dlw15b  	 1860	 1994	0.01
## 57   	dlw16b  	 1693	 1994	0.01
## 58   	dlw17b  	 1692	 1992	0.01
## 59   	dlw18a  	 1765	 1940	0.01
## 60   	DLW20A  	 1763	 1994	0.01
## 61   	dlw22a  	 1830	 1994	0.01
## 62   	dlw24a  	 1850	 1994	0.01
## 63   	DLW25A  	 1764	 1994	0.01
## 64   	dlw26a  	 1707	 1994	0.01
## 65   	dlw27a  	 1752	 1994	0.01
## 66   	dlw28a  	 1830	 1994	0.01
## 67   	DLA015  	 1648	 1908	0.01
## 68   	DLA025  	 1658	 1974	0.01
## 69   	DLA014  	 1675	 1944	0.01
## 70   	DLA020  	 1675	 1947	0.01
## 71   	DLA024  	 1843	 1945	0.01
## 72   	DLA011  	 1680	 1920	0.01
## 73   	DLA013  	 1665	 1874	0.01
## 74   	DLA023  	 1699	 1906	0.01
## 75   	DLA008  	 1737	 1915	0.01
## 76   	DLA018  	 1665	 1873	0.01
## 77   	DLA012  	 1707	 1823	0.01
## 78   	DLA005  	 1835	 1992	0.01
## 79   	DLA03   	 1793	 1983	0.01
## 80   	DLA022  	 1709	 1912	0.01
## 81   	DLA097  	 1719	 1967	0.01
## 82   	DLA067  	 1637	 1782	0.01
## 83   	DLA087  	 1684	 1800	0.01
## 84   	DLA098  	 1776	 1917	0.01
## 85   	DLA041  	 1625	 1895	0.01
## 86   	DLA047  	 1687	 1804	0.01
## 87   	DLA107  	 1675	 1876	0.01
## 88   	DLA051  	 1884	 1993	0.01
## 89   	DLA126  	 1650	 1822	0.01
## 90   	DLA101  	 1662	 1869	0.01
## 91   	DLA105  	 1665	 1810	0.01
## 92   	DLA075  	 1705	 1938	0.01
## 93   	DLA128  	 1800	 1954	0.01
## 94   	DLA123  	 1750	 1948	0.01
## 95   	DLA130  	 1605	 1831	0.01
## 96   	DLA052  	 1754	 1944	0.01
## 97   	DLA050  	 1636	 1803	0.01
## 98   	DLA131  	 1730	 1841	0.01
## 99   	361     	 1920	 2002	0.01
## 100  	133     	 1912	 2002	0.01
## 101  	193     	 1937	 2002	0.01
## 102  	281     	 1933	 2002	0.01
## 103  	51      	 1937	 2002	0.01
## 104  	104     	 1928	 2002	0.01
## 105  	263     	 1929	 2002	0.01
## 106  	124     	 1962	 2002	0.01
## 107  	14      	 1950	 2002	0.01
## 108  	21      	 1936	 2002	0.01
## 109  	323     	 1916	 2002	0.01
## 110  	121     	 1958	 2002	0.01
## 111  	342     	 1943	 2002	0.01
## 112  	64      	 1987	 2002	0.01
## 113  	331     	 1917	 2002	0.01
## 114  	92      	 1916	 2002	0.01
## 115  	72      	 1973	 2002	0.01
## 116  	144     	 1973	 2002	0.01
## 117  	283     	 1922	 2002	0.01
## 118  	62      	 1928	 2002	0.01
## 119  	81      	 1984	 2002	0.01
```

```r
master.rwl <- read.rwl("data/master_all.rwl")
```

```
## Attempting to automatically detect format.
## Assuming a Tucson format file.
## There does not appear to be a header in the rwl file
## There are 119 series
## 1    	058021  	 1679	 1981	0.01
## 2    	058022  	 1721	 1981	0.01
## 3    	058031  	 1702	 1939	0.01
## 4    	058032  	 1689	 1939	0.01
## 5    	058051  	 1720	 1956	0.01
## 6    	058052  	 1693	 1940	0.01
## 7    	058061  	 1685	 1967	0.01
## 8    	058062  	 1687	 1968	0.01
## 9    	058071  	 1700	 1970	0.01
## 10   	058072  	 1691	 1970	0.01
## 11   	058081  	 1706	 1970	0.01
## 12   	058082  	 1693	 1967	0.01
## 13   	058101  	 1693	 1970	0.01
## 14   	058102  	 1698	 1970	0.01
## 15   	058121  	 1696	 1981	0.01
## 16   	058122  	 1711	 1981	0.01
## 17   	058141  	 1712	 1981	0.01
## 18   	058142  	 1699	 1981	0.01
## 19   	058151  	 1705	 1970	0.01
## 20   	058152  	 1692	 1981	0.01
## 21   	058191  	 1685	 1968	0.01
## 22   	058201  	 1730	 1940	0.01
## 23   	058202  	 1684	 1981	0.01
## 24   	058211  	 1749	 1979	0.01
## 25   	dlw01a  	 1764	 1994	0.01
## 26   	dlw02a  	 1716	 1994	0.01
## 27   	dlw03a  	 1762	 1994	0.01
## 28   	dlw07a  	 1889	 1994	0.01
## 29   	dlw08a  	 1788	 1994	0.01
## 30   	dlw10a  	 1798	 1994	0.01
## 31   	dlw11a  	 1741	 1994	0.01
## 32   	dlw12a  	 1704	 1994	0.01
## 33   	dlw13a  	 1743	 1994	0.01
## 34   	dlw14a  	 1662	 1994	0.01
## 35   	dlw16a  	 1676	 1991	0.01
## 36   	dlw15a  	 1830	 1994	0.01
## 37   	dlw17a  	 1730	 1992	0.01
## 38   	dlw18b  	 1777	 1994	0.01
## 39   	dlw19b  	 1862	 1994	0.01
## 40   	dlw20b  	 1805	 1994	0.01
## 41   	dlw23b  	 1762	 1994	0.01
## 42   	dlw24b  	 1775	 1994	0.01
## 43   	dlw25b  	 1777	 1994	0.01
## 44   	dlw26b  	 1790	 1994	0.01
## 45   	dlw27b  	 1757	 1994	0.01
## 46   	dlw029  	 1800	 1994	0.01
## 47   	dlw01b  	 1770	 1994	0.01
## 48   	dlw03b  	 1762	 1994	0.01
## 49   	dlw05a  	 1773	 1994	0.01
## 50   	dlw06a  	 1757	 1994	0.01
## 51   	dlw07b  	 1880	 1994	0.01
## 52   	dlw08b  	 1766	 1994	0.01
## 53   	dlw11c  	 1687	 1994	0.01
## 54   	dlw12b  	 1665	 1992	0.01
## 55   	dlw14b  	 1669	 1994	0.01
## 56   	dlw15b  	 1860	 1994	0.01
## 57   	dlw16b  	 1693	 1994	0.01
## 58   	dlw17b  	 1692	 1992	0.01
## 59   	dlw18a  	 1765	 1940	0.01
## 60   	DLW20A  	 1763	 1994	0.01
## 61   	dlw22a  	 1830	 1994	0.01
## 62   	dlw24a  	 1850	 1994	0.01
## 63   	DLW25A  	 1764	 1994	0.01
## 64   	dlw26a  	 1707	 1994	0.01
## 65   	dlw27a  	 1752	 1994	0.01
## 66   	dlw28a  	 1830	 1994	0.01
## 67   	DLA015  	 1648	 1908	0.01
## 68   	DLA025  	 1658	 1974	0.01
## 69   	DLA014  	 1675	 1944	0.01
## 70   	DLA020  	 1675	 1947	0.01
## 71   	DLA024  	 1843	 1945	0.01
## 72   	DLA011  	 1680	 1920	0.01
## 73   	DLA013  	 1665	 1874	0.01
## 74   	DLA023  	 1699	 1906	0.01
## 75   	DLA008  	 1737	 1915	0.01
## 76   	DLA018  	 1665	 1873	0.01
## 77   	DLA012  	 1707	 1823	0.01
## 78   	DLA005  	 1835	 1992	0.01
## 79   	DLA03   	 1793	 1983	0.01
## 80   	DLA022  	 1709	 1912	0.01
## 81   	DLA097  	 1719	 1967	0.01
## 82   	DLA067  	 1637	 1782	0.01
## 83   	DLA087  	 1684	 1800	0.01
## 84   	DLA098  	 1776	 1917	0.01
## 85   	DLA041  	 1625	 1895	0.01
## 86   	DLA047  	 1687	 1804	0.01
## 87   	DLA107  	 1675	 1876	0.01
## 88   	DLA051  	 1884	 1993	0.01
## 89   	DLA126  	 1650	 1822	0.01
## 90   	DLA101  	 1662	 1869	0.01
## 91   	DLA105  	 1665	 1810	0.01
## 92   	DLA075  	 1705	 1938	0.01
## 93   	DLA128  	 1800	 1954	0.01
## 94   	DLA123  	 1750	 1948	0.01
## 95   	DLA130  	 1605	 1831	0.01
## 96   	DLA052  	 1754	 1944	0.01
## 97   	DLA050  	 1636	 1803	0.01
## 98   	DLA131  	 1730	 1841	0.01
## 99   	361     	 1920	 2002	0.01
## 100  	133     	 1912	 2002	0.01
## 101  	193     	 1937	 2002	0.01
## 102  	281     	 1933	 2002	0.01
## 103  	51      	 1937	 2002	0.01
## 104  	104     	 1928	 2002	0.01
## 105  	263     	 1929	 2002	0.01
## 106  	124     	 1962	 2002	0.01
## 107  	14      	 1950	 2002	0.01
## 108  	21      	 1936	 2002	0.01
## 109  	323     	 1916	 2002	0.01
## 110  	121     	 1958	 2002	0.01
## 111  	342     	 1943	 2002	0.01
## 112  	64      	 1987	 2002	0.01
## 113  	331     	 1917	 2002	0.01
## 114  	92      	 1916	 2002	0.01
## 115  	72      	 1973	 2002	0.01
## 116  	144     	 1973	 2002	0.01
## 117  	283     	 1922	 2002	0.01
## 118  	62      	 1928	 2002	0.01
## 119  	81      	 1984	 2002	0.01
```

```r
mi015.rwl <- read.rwl("data/mi015.rwl")
```

```
## Attempting to automatically detect format.
## Assuming a Tucson format file.
## There appears to be a header in the rwl file
## There are 21 series
## 1    	36-1    	 1920	 2002	0.01
## 2    	13-3    	 1912	 2002	0.01
## 3    	19-3    	 1937	 2002	0.01
## 4    	28-1    	 1933	 2002	0.01
## 5    	5-1     	 1937	 2002	0.01
## 6    	10-4    	 1928	 2002	0.01
## 7    	26-3    	 1929	 2002	0.01
## 8    	12-4    	 1962	 2002	0.01
## 9    	1-4     	 1950	 2002	0.01
## 10   	2-1     	 1936	 2002	0.01
## 11   	32-3    	 1916	 2002	0.01
## 12   	12-1    	 1958	 2002	0.01
## 13   	34-2    	 1943	 2002	0.01
## 14   	6-4     	 1987	 2002	0.01
## 15   	33-1    	 1917	 2002	0.01
## 16   	9-2     	 1916	 2002	0.01
## 17   	7-2     	 1973	 2002	0.01
## 18   	14-4    	 1973	 2002	0.01
## 19   	28-3    	 1922	 2002	0.01
## 20   	6-2     	 1928	 2002	0.01
## 21   	8-1     	 1984	 2002	0.01
```

```r
cana127.rwl <- read.rwl("data/cana127.rwl")
```

```
## Attempting to automatically detect format.
## Assuming a Tucson format file.
## There appears to be a header in the rwl file
## There are 47 series
## 1    	dlw01a  	 1764	 1994	0.01
## 2    	dlw02a  	 1716	 1994	0.01
## 3    	dlw03a  	 1762	 1994	0.01
## 4    	DLW04B  	 1688	 1994	0.01
## 5    	dlw05b  	 1771	 1994	0.01
## 6    	dlw07a  	 1889	 1994	0.01
## 7    	dlw08a  	 1788	 1994	0.01
## 8    	DLW09A  	 1736	 1994	0.01
## 9    	dlw10a  	 1798	 1994	0.01
## 10   	dlw11a  	 1741	 1994	0.01
## 11   	dlw12a  	 1704	 1994	0.01
## 12   	dlw13a  	 1743	 1994	0.01
## 13   	dlw14a  	 1662	 1994	0.01
## 14   	dlw16a  	 1676	 1991	0.01
## 15   	dlw15a  	 1830	 1994	0.01
## 16   	dlw17a  	 1730	 1992	0.01
## 17   	dlw18b  	 1777	 1994	0.01
## 18   	dlw19b  	 1862	 1994	0.01
## 19   	dlw20b  	 1805	 1994	0.01
## 20   	dlw23b  	 1762	 1994	0.01
## 21   	dlw24b  	 1775	 1994	0.01
## 22   	dlw25b  	 1777	 1994	0.01
## 23   	dlw26b  	 1790	 1994	0.01
## 24   	dlw27b  	 1757	 1994	0.01
## 25   	dlw029  	 1800	 1994	0.01
## 26   	dlw01b  	 1770	 1994	0.01
## 27   	dlw03b  	 1762	 1994	0.01
## 28   	dlw05a  	 1773	 1994	0.01
## 29   	dlw06a  	 1757	 1994	0.01
## 30   	dlw07b  	 1880	 1994	0.01
## 31   	dlw08b  	 1766	 1994	0.01
## 32   	dlw09b  	 1741	 1994	0.01
## 33   	dlw11c  	 1687	 1994	0.01
## 34   	dlw12b  	 1665	 1992	0.01
## 35   	dlw14b  	 1669	 1994	0.01
## 36   	dlw15b  	 1860	 1994	0.01
## 37   	dlw16b  	 1693	 1994	0.01
## 38   	dlw17b  	 1692	 1992	0.01
## 39   	dlw18a  	 1765	 1940	0.01
## 40   	dlw19a  	 1784	 1994	0.01
## 41   	DLW20A  	 1763	 1994	0.01
## 42   	dlw22a  	 1830	 1994	0.01
## 43   	dlw24a  	 1850	 1994	0.01
## 44   	DLW25A  	 1764	 1994	0.01
## 45   	dlw26a  	 1707	 1994	0.01
## 46   	dlw27a  	 1752	 1994	0.01
## 47   	dlw28a  	 1830	 1994	0.01
```

```r
cana148.rwl <- read.rwl("data/cana148.rwl")
```

```
## Attempting to automatically detect format.
## Assuming a Tucson format file.
## There appears to be a header in the rwl file
## There are 50 series
## 1    	DLA015  	 1648	 1908	0.01
## 2    	DLA025  	 1658	 1974	0.01
## 3    	DLA014  	 1675	 1944	0.01
## 4    	DLA020  	 1675	 1947	0.01
## 5    	DLA024  	 1843	 1945	0.01
## 6    	DLA011  	 1680	 1920	0.01
## 7    	DLA007  	 1551	 1668	0.01
## 8    	DLA013  	 1665	 1874	0.01
## 9    	DLA023  	 1699	 1906	0.01
## 10   	DLA008  	 1737	 1915	0.01
## 11   	DLA018  	 1665	 1873	0.01
## 12   	DLA012  	 1707	 1823	0.01
## 13   	DLA005  	 1835	 1992	0.01
## 14   	DLA03   	 1793	 1983	0.01
## 15   	DLA022  	 1709	 1912	0.01
## 16   	dla100  	 1531	 1756	0.01
## 17   	DLA097  	 1719	 1967	0.01
## 18   	DLA069  	 1568	 1692	0.01
## 19   	DLA109  	 1555	 1668	0.01
## 20   	DLA067  	 1637	 1782	0.01
## 21   	DL122B  	 1555	 1780	0.01
## 22   	DLA087  	 1684	 1800	0.01
## 23   	DLA044  	 1552	 1709	0.01
## 24   	DLA098  	 1776	 1917	0.01
## 25   	DLA060  	 1398	 1574	0.01
## 26   	DLA041  	 1625	 1895	0.01
## 27   	DLA047  	 1687	 1804	0.01
## 28   	DLA111  	 1552	 1688	0.01
## 29   	DLA104  	 1477	 1725	0.01
## 30   	DLA107  	 1675	 1876	0.01
## 31   	DLA051  	 1884	 1993	0.01
## 32   	DLA066  	  950	 1152	0.01
## 33   	DLA126  	 1650	 1822	0.01
## 34   	DLA101  	 1662	 1869	0.01
## 35   	DLA064  	 1320	 1436	0.01
## 36   	DLA105  	 1665	 1810	0.01
## 37   	DLA075  	 1705	 1938	0.01
## 38   	DLA128  	 1800	 1954	0.01
## 39   	DLA068  	 1042	 1319	0.01
## 40   	DLA123  	 1750	 1948	0.01
## 41   	DLA130  	 1605	 1831	0.01
## 42   	DLA057  	 1507	 1701	0.01
## 43   	DLA052  	 1754	 1944	0.01
## 44   	DLA050  	 1636	 1803	0.01
## 45   	DLA056  	 1568	 1734	0.01
## 46   	DLA131  	 1730	 1841	0.01
## 47   	DLA63   	 1392	 1493	0.01
## 48   	DLA122  	 1517	 1650	0.01
## 49   	DLA65B  	 1322	 1442	0.01
## 50   	DLA088  	 1388	 1550	0.01
```

```r
pa008.rwl <- read.rwl("data/pa008.rwl")
```

```
## Attempting to automatically detect format.
## Assuming a Tucson format file.
## There does not appear to be a header in the rwl file
## There are 24 series
## 1    	058021  	 1679	 1981	0.01
## 2    	058022  	 1721	 1981	0.01
## 3    	058031  	 1702	 1939	0.01
## 4    	058032  	 1689	 1939	0.01
## 5    	058051  	 1720	 1956	0.01
## 6    	058052  	 1693	 1940	0.01
## 7    	058061  	 1685	 1967	0.01
## 8    	058062  	 1687	 1968	0.01
## 9    	058071  	 1700	 1970	0.01
## 10   	058072  	 1691	 1970	0.01
## 11   	058081  	 1706	 1970	0.01
## 12   	058082  	 1693	 1967	0.01
## 13   	058101  	 1693	 1970	0.01
## 14   	058102  	 1698	 1970	0.01
## 15   	058121  	 1696	 1981	0.01
## 16   	058122  	 1711	 1981	0.01
## 17   	058141  	 1712	 1981	0.01
## 18   	058142  	 1699	 1981	0.01
## 19   	058151  	 1705	 1970	0.01
## 20   	058152  	 1692	 1981	0.01
## 21   	058191  	 1685	 1968	0.01
## 22   	058201  	 1730	 1940	0.01
## 23   	058202  	 1684	 1981	0.01
## 24   	058211  	 1749	 1979	0.01
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

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

## Truncate Based on Date / Sample Depth

Truncate to 1898 (sample depth = 8)


```r
pinery.rwl.trunc <- pinery.rwl[46:166,]
```



# Crossdating



```r
pinery.corr <- corr.rwl.seg(pinery.rwl.trunc, seg.length = 50, 1918)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


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
a measure of average interseries correlation that is different from the rbar
measurements from rwi.stats. In this function, correlations are calculated
serially between each tree-ring series and a master chronology built from
all the other series in the rwl object (leave-one-out principle).


```r
interseries.cor(pinery.rwl) %>% round(2)
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


# Compare to Master Chronologies 

See Master Chronology .Rmd for details


## Compare to Filtered Regional Chronology


```r
master.corr <- corr.rwl.seg(pinery.rwl.trunc, master=master.rwl,seg.length=30, 1853)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


```r
#identify flags
flags.2 <- as.data.frame(master.corr$flags) 
flags.2
```

```
##                                             master.corr$flags
## P1809Ae                                  1958.1987, 1973.2002
## P1809Be                       1913.1942, 1943.1972, 1958.1987
## P1809Ce            1913.1942, 1928.1957, 1943.1972, 1973.2002
## P1810Ae 1913.1942, 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1810Be 1913.1942, 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1810Ce                       1913.1942, 1928.1957, 1943.1972
## P1811Ae                       1943.1972, 1958.1987, 1973.2002
## P1811Be 1913.1942, 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1811Ce            1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1812Ae            1913.1942, 1928.1957, 1943.1972, 1958.1987
## P1812Be 1913.1942, 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1812De            1928.1957, 1943.1972, 1958.1987, 1973.2002
```

```r
#correlation by series
overall.2 <- master.corr$overall %>% as.data.frame()
overall.2 %>% round(2)
```

```
##          rho p-val
## P1809Ae 0.18  0.09
## P1809Be 0.26  0.00
## P1809Ce 0.17  0.05
## P1810Ae 0.02  0.41
## P1810Be 0.04  0.35
## P1810Ce 0.24  0.01
## P1811Ae 0.23  0.01
## P1811Be 0.17  0.04
## P1811Ce 0.21  0.02
## P1812Ae 0.21  0.02
## P1812Be 0.10  0.17
## P1812De 0.15  0.07
```

```r
#average series correlation
avg.seg.rho.2 <- as.data.frame(master.corr$avg.seg.rho)
avg.seg.rho.2 %>% round(2)
```

```
##           master.corr$avg.seg.rho
## 1853.1882                     NaN
## 1868.1897                     NaN
## 1883.1912                     NaN
## 1898.1927                     NaN
## 1913.1942                    0.25
## 1928.1957                    0.19
## 1943.1972                    0.08
## 1958.1987                    0.14
## 1973.2002                    0.16
## 1988.2017                     NaN
```

```r
#correlation by segment and series
rho.2 <- as.data.frame(master.corr$spearman.rho)
rho.2 %>% round(2)
```

```
##         1853.1882 1868.1897 1883.1912 1898.1927 1913.1942 1928.1957 1943.1972
## P1809Ae        NA        NA        NA        NA        NA        NA        NA
## P1809Be        NA        NA        NA        NA      0.25      0.48      0.17
## P1809Ce        NA        NA        NA        NA      0.30      0.26      0.28
## P1810Ae        NA        NA        NA        NA      0.30      0.00     -0.11
## P1810Be        NA        NA        NA        NA      0.09      0.18      0.04
## P1810Ce        NA        NA        NA        NA      0.11      0.14      0.22
## P1811Ae        NA        NA        NA        NA      0.35      0.41      0.14
## P1811Be        NA        NA        NA        NA      0.23      0.02      0.04
## P1811Ce        NA        NA        NA        NA      0.39      0.11     -0.06
## P1812Ae        NA        NA        NA        NA      0.24      0.13      0.05
## P1812Be        NA        NA        NA        NA      0.16      0.17      0.03
## P1812De        NA        NA        NA        NA      0.39      0.16      0.06
##         1958.1987 1973.2002 1988.2017
## P1809Ae      0.16      0.29        NA
## P1809Be      0.02      0.31        NA
## P1809Ce      0.50      0.16        NA
## P1810Ae     -0.07     -0.14        NA
## P1810Be     -0.11      0.11        NA
## P1810Ce      0.46      0.34        NA
## P1811Ae      0.26      0.06        NA
## P1811Be      0.16      0.13        NA
## P1811Ce      0.17      0.27        NA
## P1812Ae      0.04      0.32        NA
## P1812Be      0.08      0.04        NA
## P1812De      0.02      0.03        NA
```

```r
summarize(overall.2, mean = mean(rho)) %>% round(2)
```

```
##   mean
## 1 0.17
```


## Compare to Full Regional Chronology


```r
master.corr.all <- corr.rwl.seg(pinery.rwl.trunc, master=master_all.rwl, seg.length=50, 1853)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


```r
#identify flags
flags.3 <- as.data.frame(master.corr.all$flags) 
flags.3
```

```
##                   master.corr.all$flags
## P1809Ae                       1953.2002
## P1809Be            1928.1977, 1953.2002
## P1810Ae 1903.1952, 1928.1977, 1953.2002
## P1810Be            1928.1977, 1953.2002
## P1810Ce                       1928.1977
## P1811Ae                       1953.2002
## P1811Be            1928.1977, 1953.2002
## P1811Ce            1928.1977, 1953.2002
## P1812Ae 1903.1952, 1928.1977, 1953.2002
## P1812Be            1928.1977, 1953.2002
## P1812De            1928.1977, 1953.2002
```

```r
#correlation by series
overall.3 <- master.corr.all$overall %>% as.data.frame()
overall.3 %>% round(2)
```

```
##          rho p-val
## P1809Ae 0.18  0.09
## P1809Be 0.26  0.00
## P1809Ce 0.17  0.05
## P1810Ae 0.02  0.41
## P1810Be 0.04  0.35
## P1810Ce 0.24  0.01
## P1811Ae 0.23  0.01
## P1811Be 0.17  0.04
## P1811Ce 0.21  0.02
## P1812Ae 0.21  0.02
## P1812Be 0.10  0.17
## P1812De 0.15  0.07
```

```r
#average series correlation
avg.seg.rho.3 <- as.data.frame(master.corr.all$avg.seg.rho)
avg.seg.rho.3 %>% round(2)
```

```
##           master.corr.all$avg.seg.rho
## 1853.1902                         NaN
## 1878.1927                         NaN
## 1903.1952                        0.26
## 1928.1977                        0.13
## 1953.2002                        0.09
```

```r
#correlation by segment and series
rho.3 <- as.data.frame(master.corr.all$spearman.rho)
rho.3 %>% round(2)
```

```
##         1853.1902 1878.1927 1903.1952 1928.1977 1953.2002
## P1809Ae        NA        NA        NA        NA      0.21
## P1809Be        NA        NA      0.33      0.19      0.20
## P1809Ce        NA        NA        NA      0.34      0.24
## P1810Ae        NA        NA      0.19     -0.03     -0.21
## P1810Be        NA        NA        NA      0.06      0.01
## P1810Ce        NA        NA        NA      0.18      0.29
## P1811Ae        NA        NA      0.32      0.30     -0.03
## P1811Be        NA        NA      0.25      0.06     -0.01
## P1811Ce        NA        NA      0.29      0.11      0.14
## P1812Ae        NA        NA      0.19      0.05      0.20
## P1812Be        NA        NA        NA      0.09      0.05
## P1812De        NA        NA        NA      0.10      0.03
```

```r
summarize(overall.3, mean = mean(rho)) %>% round(2)
```

```
##   mean
## 1 0.17
```


## Compare to Each Master Chronlogy

### Wyse - Platte Plains - PIST - ITRDB MI015



```r
rwl.report(mi015.rwl)
```

```
## Number of dated series: 21 
## Number of measurements: 1302 
## Avg series length: 62 
## Range:  91 
## Span:  1912 - 2002 
## Mean (Std dev) series intercorrelation: 0.4915569 (0.1124546)
## Mean (Std dev) AR1: 0.7149524 (0.1194401)
## -------------
## Years with absent rings listed by series 
##     Series 19-3 -- 2002 
## 1 absent rings (0.077%)
## -------------
## Years with internal NA values listed by series 
##     None
```

```r
master.corr.mi015 <- corr.rwl.seg(pinery.rwl.trunc, master=mi015.rwl, seg.length = 30, bin.floor = 1913)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


```r
#identify flags
flags.4 <- as.data.frame(master.corr.mi015$flags) 
flags.4
```

```
##                            master.corr.mi015$flags
## P1809Ae                       1958.1987, 1973.2002
## P1809Be 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1809Ce 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1810Ae 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1810Be 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1811Ae 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1811Be                       1958.1987, 1973.2002
## P1811Ce 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1812Ae 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1812Be 1928.1957, 1943.1972, 1958.1987, 1973.2002
## P1812De 1928.1957, 1943.1972, 1958.1987, 1973.2002
```

```r
#correlation by series
overall.4 <- master.corr.mi015$overall %>% as.data.frame()
overall.4 %>% round(2)
```

```
##           rho p-val
## P1809Ae  0.20  0.07
## P1809Be  0.14  0.10
## P1809Ce  0.19  0.04
## P1810Ae -0.04  0.64
## P1810Be  0.21  0.03
## P1810Ce  0.38  0.00
## P1811Ae  0.10  0.17
## P1811Be  0.23  0.02
## P1811Ce  0.07  0.26
## P1812Ae  0.13  0.12
## P1812Be  0.04  0.37
## P1812De  0.12  0.13
```

```r
#average series correlation
avg.seg.rho.4 <- as.data.frame(master.corr.mi015$avg.seg.rho)
avg.seg.rho.4 %>% round(2)
```

```
##           master.corr.mi015$avg.seg.rho
## 1913.1942                           NaN
## 1928.1957                          0.19
## 1943.1972                          0.06
## 1958.1987                          0.12
## 1973.2002                          0.12
## 1988.2017                           NaN
```

```r
#correlation by segment and series
rho.4 <- as.data.frame(master.corr.mi015$spearman.rho)
rho.4 %>% round(2)
```

```
##         1913.1942 1928.1957 1943.1972 1958.1987 1973.2002 1988.2017
## P1809Ae        NA        NA        NA      0.12      0.12        NA
## P1809Be        NA      0.19      0.10      0.02      0.16        NA
## P1809Ce        NA      0.23      0.01      0.23      0.20        NA
## P1810Ae        NA     -0.03      0.06      0.04     -0.15        NA
## P1810Be        NA      0.24      0.11      0.11      0.11        NA
## P1810Ce        NA      0.50      0.41      0.47      0.33        NA
## P1811Ae        NA      0.17      0.04      0.13      0.16        NA
## P1811Be        NA      0.36      0.31      0.21      0.15        NA
## P1811Ce        NA      0.25     -0.15     -0.16      0.16        NA
## P1812Ae        NA      0.04      0.02      0.09      0.14        NA
## P1812Be        NA      0.10     -0.10     -0.01      0.02        NA
## P1812De        NA      0.04     -0.14      0.16      0.08        NA
```

```r
summarize(overall.4, mean = mean(rho)) %>% round(2)
```

```
##   mean
## 1 0.15
```

### Guyette - Dividing Lake - PIST - ITRDB CANA127


```r
rwl.report(cana127.rwl)
```

```
## Number of dated series: 47 
## Number of measurements: 10839 
## Avg series length: 230.617 
## Range:  333 
## Span:  1662 - 1994 
## Mean (Std dev) series intercorrelation: 0.5399936 (0.1049041)
## Mean (Std dev) AR1: 0.815617 (0.1225253)
## -------------
## Years with absent rings listed by series 
##     None 
## -------------
## Years with internal NA values listed by series 
##     None
```

```r
master.corr.cana127 <- corr.rwl.seg(pinery.rwl.trunc, master=cana127.rwl, seg.length = 30, bin.floor=1905)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-16-1.png)<!-- -->


```r
#identify flags
flags.5 <- as.data.frame(master.corr.cana127$flags) 
flags.5
```

```
##                                     master.corr.cana127$flags
## P1809Ae                                  1950.1979, 1965.1994
## P1809Be 1905.1934, 1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1809Ce                       1905.1934, 1920.1949, 1935.1964
## P1810Ae 1905.1934, 1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1810Be            1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1810Ce 1905.1934, 1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1811Ae            1905.1934, 1935.1964, 1950.1979, 1965.1994
## P1811Be 1905.1934, 1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1811Ce 1905.1934, 1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1812Ae 1905.1934, 1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1812Be            1920.1949, 1935.1964, 1950.1979, 1965.1994
## P1812De            1920.1949, 1935.1964, 1950.1979, 1965.1994
```

```r
#correlation by series
overall.5 <- master.corr.cana127$overall %>% as.data.frame()
overall.5 %>% round(2)
```

```
##           rho p-val
## P1809Ae  0.02  0.45
## P1809Be  0.21  0.02
## P1809Ce  0.13  0.12
## P1810Ae  0.06  0.27
## P1810Be -0.05  0.68
## P1810Ce  0.10  0.18
## P1811Ae  0.21  0.02
## P1811Be  0.05  0.33
## P1811Ce  0.09  0.19
## P1812Ae  0.14  0.09
## P1812Be  0.09  0.20
## P1812De  0.14  0.10
```

```r
#average series correlation
avg.seg.rho.5 <- as.data.frame(master.corr.cana127$avg.seg.rho)
avg.seg.rho.5 %>% round(2)
```

```
##           master.corr.cana127$avg.seg.rho
## 1905.1934                            0.19
## 1920.1949                            0.07
## 1935.1964                            0.01
## 1950.1979                            0.02
## 1965.1994                            0.08
## 1980.2009                             NaN
```

```r
#correlation by segment and series
rho.5 <- as.data.frame(master.corr.cana127$spearman.rho)
rho.5 %>% round(2)
```

```
##         1905.1934 1920.1949 1935.1964 1950.1979 1965.1994 1980.2009
## P1809Ae        NA        NA        NA      0.01      0.09        NA
## P1809Be      0.19      0.19      0.13     -0.05      0.27        NA
## P1809Ce      0.11      0.07     -0.08      0.45      0.35        NA
## P1810Ae      0.25      0.17     -0.07     -0.25     -0.15        NA
## P1810Be        NA      0.08     -0.04     -0.12     -0.20        NA
## P1810Ce      0.12     -0.11      0.04      0.09      0.26        NA
## P1811Ae      0.17      0.39      0.21     -0.01      0.09        NA
## P1811Be      0.17     -0.05     -0.20     -0.10      0.04        NA
## P1811Ce      0.25      0.09     -0.04      0.01      0.12        NA
## P1812Ae      0.28     -0.10      0.04      0.10      0.10        NA
## P1812Be        NA      0.05      0.02      0.00      0.02        NA
## P1812De        NA     -0.01      0.05      0.07      0.01        NA
```

```r
summarize(overall.5, mean = mean(rho)) %>% round(2)
```

```
##   mean
## 1  0.1
```

### Guyette - Dividing Lake Aquatic - PIST - ITRDB CANA148


```r
rwl.report(cana148.rwl)
```

```
## Number of dated series: 50 
## Number of measurements: 9119 
## Avg series length: 182.38 
## Range:  1044 
## Span:  950 - 1993 
## Mean (Std dev) series intercorrelation: 0.4970208 (0.1131863)
## Mean (Std dev) AR1: 0.84128 (0.09196873)
## -------------
## Years with absent rings listed by series 
##     None 
## -------------
## Years with internal NA values listed by series 
##     None
```

```r
master.corr.cana148 <- corr.rwl.seg(pinery.rwl.trunc, master=cana148.rwl, seg.length = 30, bin.floor=1904)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-18-1.png)<!-- -->


```r
#identify flags
flags.6 <- as.data.frame(master.corr.cana148$flags) 
flags.6
```

```
##                                     master.corr.cana148$flags
## P1809Ae                                  1949.1978, 1964.1993
## P1809Be            1904.1933, 1919.1948, 1934.1963, 1949.1978
## P1809Ce            1919.1948, 1934.1963, 1949.1978, 1964.1993
## P1810Ae 1904.1933, 1919.1948, 1934.1963, 1949.1978, 1964.1993
## P1810Be                                  1919.1948, 1934.1963
## P1810Ce                       1904.1933, 1919.1948, 1934.1963
## P1811Ae 1904.1933, 1919.1948, 1934.1963, 1949.1978, 1964.1993
## P1811Be 1904.1933, 1919.1948, 1934.1963, 1949.1978, 1964.1993
## P1811Ce            1919.1948, 1934.1963, 1949.1978, 1964.1993
## P1812Ae            1904.1933, 1919.1948, 1934.1963, 1964.1993
## P1812Be            1919.1948, 1934.1963, 1949.1978, 1964.1993
## P1812De            1919.1948, 1934.1963, 1949.1978, 1964.1993
```

```r
#correlation by series
overall.6 <- master.corr.cana148$overall %>% as.data.frame()
overall.6 %>% round(2)
```

```
##          rho p-val
## P1809Ae 0.14  0.17
## P1809Be 0.23  0.01
## P1809Ce 0.10  0.19
## P1810Ae 0.09  0.19
## P1810Be 0.19  0.04
## P1810Ce 0.25  0.01
## P1811Ae 0.24  0.01
## P1811Be 0.16  0.07
## P1811Ce 0.15  0.08
## P1812Ae 0.20  0.03
## P1812Be 0.12  0.13
## P1812De 0.13  0.12
```

```r
#average series correlation
avg.seg.rho.6 <- as.data.frame(master.corr.cana148$avg.seg.rho) %>% as.data.frame
avg.seg.rho.6 %>% round(2)
```

```
##           master.corr.cana148$avg.seg.rho
## 1904.1933                            0.21
## 1919.1948                            0.13
## 1934.1963                            0.03
## 1949.1978                            0.15
## 1964.1993                            0.25
## 1979.2008                             NaN
```

```r
#correlation by segment and series
rho.6 <- as.data.frame(master.corr.cana148$spearman.rho)
rho.6 %>% round(2)
```

```
##         1904.1933 1919.1948 1934.1963 1949.1978 1964.1993 1979.2008
## P1809Ae        NA        NA        NA      0.06      0.22        NA
## P1809Be      0.07      0.17      0.25      0.04      0.33        NA
## P1809Ce        NA      0.18      0.07      0.13      0.13        NA
## P1810Ae      0.22      0.02     -0.11     -0.11      0.07        NA
## P1810Be        NA      0.18      0.16      0.37      0.34        NA
## P1810Ce      0.02      0.07      0.21      0.38      0.52        NA
## P1811Ae      0.28      0.24      0.07      0.14      0.20        NA
## P1811Be      0.24      0.04     -0.11     -0.01      0.24        NA
## P1811Ce      0.37      0.30     -0.03      0.08      0.12        NA
## P1812Ae      0.25      0.06      0.07      0.39      0.23        NA
## P1812Be        NA      0.10     -0.17      0.11      0.30        NA
## P1812De        NA      0.05     -0.12      0.18      0.27        NA
```

```r
summarize(overall.6, mean = mean(rho)) %>% round(2)
```

```
##   mean
## 1 0.17
```


### Cook - Longfellow Trail - PIST - ITRDB PA008



```r
rwl.report(pa008.rwl)
```

```
## Number of dated series: 24 
## Number of measurements: 6435 
## Avg series length: 268.125 
## Range:  303 
## Span:  1679 - 1981 
## Mean (Std dev) series intercorrelation: 0.6192482 (0.0761018)
## Mean (Std dev) AR1: 0.8445 (0.07192569)
## -------------
## Years with absent rings listed by series 
##     Series 058061 -- 1934 
## 1 absent rings (0.016%)
## -------------
## Years with internal NA values listed by series 
##     None
```

```r
master.corr.pa008 <- corr.rwl.seg(pinery.rwl.trunc, master=pa008.rwl, seg.length=40, bin.floor = 1902)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-20-1.png)<!-- -->




```r
#identify flags
flags.7 <- as.data.frame(master.corr.pa008$flags) 
flags.7
```

```
##                 master.corr.pa008$flags
## P1809Be                       1922.1961
## P1809Ce            1922.1961, 1942.1981
## P1810Ae 1902.1941, 1922.1961, 1942.1981
## P1810Be            1922.1961, 1942.1981
## P1810Ce            1922.1961, 1942.1981
## P1811Ae            1922.1961, 1942.1981
## P1811Be            1922.1961, 1942.1981
## P1811Ce                       1922.1961
## P1812Ae            1922.1961, 1942.1981
## P1812Be            1922.1961, 1942.1981
## P1812De            1922.1961, 1942.1981
```

```r
#correlation by series
overall.7 <- master.corr.pa008$overall %>% as.data.frame()
overall.7 %>% round(2)
```

```
##          rho p-val
## P1809Ae 0.11  0.26
## P1809Be 0.35  0.00
## P1809Ce 0.11  0.18
## P1810Ae 0.12  0.15
## P1810Be 0.06  0.31
## P1810Ce 0.13  0.13
## P1811Ae 0.25  0.01
## P1811Be 0.28  0.01
## P1811Ce 0.32  0.00
## P1812Ae 0.22  0.02
## P1812Be 0.14  0.12
## P1812De 0.28  0.01
```

```r
#average series correlation
avg.seg.rho.7 <- as.data.frame(master.corr.pa008$avg.seg.rho)
avg.seg.rho.7 %>% round(2)
```

```
##           master.corr.pa008$avg.seg.rho
## 1902.1941                          0.30
## 1922.1961                          0.08
## 1942.1981                          0.13
## 1962.2001                           NaN
```

```r
#correlation by segment and series
rho.7 <- as.data.frame(master.corr.pa008$spearman.rho)
rho.7 %>% round(2)
```

```
##         1902.1941 1922.1961 1942.1981 1962.2001
## P1809Ae        NA        NA        NA        NA
## P1809Be      0.38      0.25      0.35        NA
## P1809Ce        NA      0.19     -0.02        NA
## P1810Ae      0.18     -0.06      0.04        NA
## P1810Be        NA      0.14      0.12        NA
## P1810Ce        NA     -0.10     -0.05        NA
## P1811Ae      0.33      0.14      0.15        NA
## P1811Be        NA      0.06      0.00        NA
## P1811Ce        NA      0.02      0.37        NA
## P1812Ae      0.31     -0.08      0.15        NA
## P1812Be        NA      0.10      0.16        NA
## P1812De        NA      0.20      0.14        NA
```

```r
summarize(overall.7, mean = mean(rho)) %>% round(2)
```

```
##   mean
## 1  0.2
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

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-26-1.png)<!-- -->


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

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-27-1.png)<!-- -->


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
## [1] ggplot2_3.3.0 dplyr_0.8.5   dplR_1.7.1   
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.4.6       compiler_4.0.0     pillar_1.4.3       plyr_1.8.6        
##  [5] iterators_1.0.12   R.methodsS3_1.8.0  R.utils_2.9.2      tools_4.0.0       
##  [9] digest_0.6.25      gtable_0.3.0       evaluate_0.14      tibble_3.0.1      
## [13] lifecycle_0.2.0    lattice_0.20-41    pkgconfig_2.0.3    png_0.1-7         
## [17] rlang_0.4.5        foreach_1.5.0      Matrix_1.2-18      yaml_2.2.1        
## [21] xfun_0.13          withr_2.2.0        stringr_1.4.0      knitr_1.28        
## [25] vctrs_0.2.4        grid_4.0.0         tidyselect_1.0.0   glue_1.4.0        
## [29] R6_2.4.1           XML_3.99-0.3       rmarkdown_2.1      purrr_0.3.4       
## [33] magrittr_1.5       codetools_0.2-16   scales_1.1.0       matrixStats_0.56.0
## [37] htmltools_0.4.0    ellipsis_0.3.0     MASS_7.3-51.6      assertthat_0.2.1  
## [41] colorspace_1.4-1   stringi_1.4.6      munsell_0.5.0      signal_0.7-6      
## [45] crayon_1.3.4       R.oo_1.23.0
```

