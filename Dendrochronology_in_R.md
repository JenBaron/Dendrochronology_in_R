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
#write.rwl(pinery.rwl.trunc, "outputs/pinery_rwl_trunc.rwl", format=c("tucson"))
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

## Compare Methods


```r
detrend(rwl = pinery.rwl.trunc, method = c("ModNegExp", "Spline"), make.plot = TRUE, verbose=TRUE, return.info = TRUE)
```

```
## Verbose output: P1809Ae
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls failed... fitting linear model... Linear model fit
##  Intercept: 1.110284
##  Slope: 0.004122725
##  lm has a positive slope
##  pos.slope = FALSE
##  Detrend by mean.
##  Mean = 1.260764
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 48, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

```
## Verbose output: P1809Be
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls failed... fitting linear model... Linear model fit
##  Intercept: 1.502219
##  Slope: 0.01000698
##  lm has a positive slope
##  pos.slope = FALSE
##  Detrend by mean.
##  Mean = 2.107642
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 80, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-2.png)<!-- -->

```
## Verbose output: P1809Ce
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  4.3977470
##  b: -0.1766234
##  k:  1.2853264
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 80, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-3.png)<!-- -->

```
## Verbose output: P1810Ae
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  2.1988757
##  b: -0.1325223
##  k:  0.7798402
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 81, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-4.png)<!-- -->

```
## Verbose output: P1810Be
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  9.8709024
##  b: -0.3775278
##  k:  1.2427521
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 74, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-5.png)<!-- -->

```
## Verbose output: P1810Ce
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  5.3494137
##  b: -0.1212902
##  k:  1.2397702
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 80, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-6.png)<!-- -->

```
## Verbose output: P1811Ae
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  6.95565580
##  b: -0.05960932
##  k:  0.50841242
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 80, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-7.png)<!-- -->

```
## Verbose output: P1811Be
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  6.55348540
##  b: -0.05640673
##  k:  0.65809189
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 80, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-8.png)<!-- -->

```
## Verbose output: P1811Ce
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  6.85692205
##  b: -0.06779633
##  k:  0.88871951
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 80, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-9.png)<!-- -->

```
## Verbose output: P1812Ae
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  2.9927117
##  b: -0.1494706
##  k:  0.9690993
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 81, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-10.png)<!-- -->

```
## Verbose output: P1812Be
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  3.82955342
##  b: -0.07394128
##  k:  0.51141960
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 75, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-11.png)<!-- -->

```
## Verbose output: P1812De
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Options
##  make.plot      TRUE
##  method(s)      c("ModNegExp", "Spline")
##  nyrs           NULL
##  f              0.5
##  pos.slope      FALSE
##  constrain.nls  never
##  verbose        TRUE
##  return.info    TRUE
##  wt             default
##  span           cv
##  bass           0
##  difference     FALSE
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  No zeros in input series.
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by ModNegExp.
##  Trying to fit nls model...
##  nls coefs
##  a:  3.4011237
##  b: -0.2047775
##  k:  0.6929353
## 
##  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
##  Detrend by spline.
##  Spline parameters
##  nyrs = 75, f = 0.5
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-22-12.png)<!-- -->

```
## $series
## $series$P1809Ae
##      ModNegExp    Spline
## 1898        NA        NA
## 1899        NA        NA
## 1900        NA        NA
## 1901        NA        NA
## 1902        NA        NA
## 1903        NA        NA
## 1904        NA        NA
## 1905        NA        NA
## 1906        NA        NA
## 1907        NA        NA
## 1908        NA        NA
## 1909        NA        NA
## 1910        NA        NA
## 1911        NA        NA
## 1912        NA        NA
## 1913        NA        NA
## 1914        NA        NA
## 1915        NA        NA
## 1916        NA        NA
## 1917        NA        NA
## 1918        NA        NA
## 1919        NA        NA
## 1920        NA        NA
## 1921        NA        NA
## 1922        NA        NA
## 1923        NA        NA
## 1924        NA        NA
## 1925        NA        NA
## 1926        NA        NA
## 1927        NA        NA
## 1928        NA        NA
## 1929        NA        NA
## 1930        NA        NA
## 1931        NA        NA
## 1932        NA        NA
## 1933        NA        NA
## 1934        NA        NA
## 1935        NA        NA
## 1936        NA        NA
## 1937        NA        NA
## 1938        NA        NA
## 1939        NA        NA
## 1940        NA        NA
## 1941        NA        NA
## 1942        NA        NA
## 1943        NA        NA
## 1944        NA        NA
## 1945        NA        NA
## 1946 0.6147067 0.8217168
## 1947 0.7170256 0.9316167
## 1948 0.7114734 0.8992368
## 1949 0.7122666 0.8764860
## 1950 0.8851776 1.0614862
## 1951 0.7915836 0.9259452
## 1952 0.8122060 0.9276988
## 1953 0.9970146 1.1131942
## 1954 0.9589424 1.0478476
## 1955 0.8336216 0.8925620
## 1956 1.3372845 1.4047529
## 1957 0.8082402 0.8340274
## 1958 0.8034811 0.8154984
## 1959 0.8907298 0.8903128
## 1960 1.1524759 1.1358983
## 1961 0.9200771 0.8954370
## 1962 0.9018342 0.8678742
## 1963 1.3245938 1.2623549
## 1964 1.0929882 1.0331765
## 1965 1.2841421 1.2060010
## 1966 0.9613219 0.8984787
## 1967 1.0025668 0.9340850
## 1968 1.1421647 1.0626149
## 1969 1.2714514 1.1832624
## 1970 1.3253869 1.2360458
## 1971 1.0517433 0.9846619
## 1972 1.1469237 1.0798193
## 1973 1.1556486 1.0960036
## 1974 1.5212999 1.4557010
## 1975 1.4602258 1.4118985
## 1976 1.3602864 1.3307565
## 1977 1.0406389 1.0310030
## 1978 0.6749876 0.6775743
## 1979 0.7098871 0.7220227
## 1980 0.6115340 0.6299318
## 1981 0.6551584 0.6829408
## 1982 0.6646764 0.7003955
## 1983 0.5401487 0.5746370
## 1984 0.8320353 0.8924410
## 1985 0.9930487 1.0724633
## 1986 1.0604682 1.1516652
## 1987 1.0636409 1.1601247
## 1988 0.8463123 0.9259598
## 1989 0.7566841 0.8294529
## 1990 0.6456403 0.7081883
## 1991 0.5710823 0.6260789
## 1992 0.9652878 1.0566091
## 1993 1.0747452 1.1736728
## 1994 1.2309997 1.3404211
## 1995 1.1802368 1.2809423
## 1996 1.3428367 1.4522263
## 1997 1.2191022 1.3133276
## 1998 1.5704765 1.6846296
## 1999 1.1199559 1.1954457
## 2000 1.1715120 1.2430239
## 2001 0.7828587 0.8244593
## 2002 0.6313633 0.6586527
## 2003 0.8122060 0.8373199
## 2004 0.5004902 0.5085046
## 2005 0.4227596 0.4220947
## 2006 0.7035417 0.6882668
## 2007 0.4679703 0.4473389
## 2008 0.9573561 0.8920051
## 2009 1.1961002 1.0840180
## 2010 0.8114128 0.7141191
## 2011 0.7241641 0.6181356
## 2012 1.0969540 0.9074004
## 2013 1.2984192 1.0405004
## 2014 2.1042798 1.6337983
## 2015 1.5395428 1.1586844
## 2016 1.5673038 1.1441998
## 2017 1.8932966 1.3417973
## 2018        NA        NA
## 
## $series$P1809Be
##      ModNegExp    Spline
## 1898 0.2310639 0.4171725
## 1899 0.2927443 0.5142423
## 1900 0.3933306 0.6727613
## 1901 0.4236963 0.7061691
## 1902 0.7164406 1.1644363
## 1903 0.5769482 0.9151483
## 1904 0.7449084 1.1540360
## 1905 1.1292242 1.7100373
## 1906 0.9456067 1.4008529
## 1907 0.8042164 1.1664086
## 1908 0.6533369 0.9284009
## 1909 0.7235575 1.0080938
## 1910 1.0343314 1.4138769
## 1911 0.6334094 0.8500424
## 1912 1.0144039 1.3373164
## 1913 0.8141801 1.0550119
## 1914 0.3914328 0.4988102
## 1915 0.5223848 0.6549682
## 1916 0.4118347 0.5082811
## 1917 1.1648090 1.4157402
## 1918 0.4526386 0.5420341
## 1919 0.4013965 0.4737978
## 1920 0.9550959 1.1117684
## 1921 1.0158273 1.1666753
## 1922 0.5223848 0.5922554
## 1923 0.5631887 0.6306607
## 1924 1.1856854 1.3121502
## 1925 1.0850991 1.1874690
## 1926 1.0063380 1.0897241
## 1927 1.4290854 1.5322954
## 1928 1.2924398 1.3731126
## 1929 1.2416722 1.3080227
## 1930 0.9873595 1.0320301
## 1931 0.8806051 0.9138916
## 1932 1.6340539 1.6848267
## 1933 1.0063380 1.0315207
## 1934 0.8934156 0.9109339
## 1935 1.8214671 1.8483788
## 1936 0.8037419 0.8121607
## 1937 0.9536725 0.9600063
## 1938 0.7339957 0.7363536
## 1939 0.7131193 0.7132124
## 1940 0.9038538 0.9014586
## 1941 1.0860480 1.0804392
## 1942 1.0855735 1.0774937
## 1943 1.1899556 1.1786313
## 1944 1.3911283 1.3752424
## 1945 1.6098562 1.5886187
## 1946 0.4630768 0.4561822
## 1947 0.6789579 0.6677034
## 1948 0.3425630 0.3362913
## 1949 0.4317622 0.4230742
## 1950 1.1780940 1.1521366
## 1951 0.7449084 0.7270016
## 1952 1.3759455 1.3399859
## 1953 1.2559061 1.2203489
## 1954 0.6927174 0.6715398
## 1955 0.8042164 0.7777421
## 1956 0.7335213 0.7075899
## 1957 0.7534488 0.7249249
## 1958 0.7041045 0.6756481
## 1959 0.9346940 0.8945094
## 1960 1.2155767 1.1602096
## 1961 1.3863837 1.3197836
## 1962 1.5083209 1.4322615
## 1963 1.5965712 1.5124546
## 1964 1.0253166 0.9691229
## 1965 0.6542858 0.6171273
## 1966 0.4849022 0.4564632
## 1967 0.5636632 0.5296363
## 1968 0.6139564 0.5759392
## 1969 1.0471419 0.9808906
## 1970 0.9237813 0.8643329
## 1971 0.9062262 0.8472160
## 1972 0.9854616 0.9209196
## 1973 1.0751353 1.0048025
## 1974 2.0131506 1.8826706
## 1975 1.8826730 1.7629116
## 1976 2.2257104 2.0882370
## 1977 2.0345014 1.9139486
## 1978 1.6378496 1.5459832
## 1979 1.8020141 1.7077424
## 1980 1.6805513 1.5998948
## 1981 1.2838995 1.2284122
## 1982 1.6573026 1.5941615
## 1983 0.6514390 0.6300932
## 1984 1.0950628 1.0650878
## 1985 1.3659817 1.3358378
## 1986 1.2990823 1.2769837
## 1987 1.3000312 1.2839380
## 1988 0.7937782 0.7871381
## 1989 1.0333825 1.0280361
## 1990 0.4649747 0.4635765
## 1991 0.4863256 0.4853230
## 1992 0.4996105 0.4983566
## 1993 0.4730406 0.4709148
## 1994 0.6243946 0.6193240
## 1995 0.8080121 0.7971360
## 1996 0.5845396 0.5725289
## 1997 0.4943914 0.4798641
## 1998 0.8858242 0.8504469
## 1999 0.4013965 0.3804697
## 2000 0.4929680 0.4604918
## 2001 0.4331856 0.3980812
## 2002 0.6737388 0.6080818
## 2003 0.6224967 0.5509470
## 2004 0.2016472 0.1747655
## 2005 0.1712815 0.1451842
## 2006 0.6044671 0.5005626
## 2007 0.6030437 0.4874473
## 2008 1.0343314 0.8155240
## 2009 1.3669307 1.0507902
## 2010 1.2264893 0.9189781
## 2011 1.2506870 0.9133114
## 2012 1.8309564 1.3031835
## 2013 2.0648671 1.4327495
## 2014 2.0909626 1.4148897
## 2015 2.8766750 1.8991324
## 2016 2.8263818 1.8213932
## 2017 2.6024348 1.6379322
## 2018        NA        NA
## 
## $series$P1809Ce
##      ModNegExp    Spline
## 1898 0.9740381 1.5683845
## 1899 0.7994384 1.1655458
## 1900 1.1971508 1.5919330
## 1901 0.9273387 1.1334650
## 1902 0.9420831 1.0670255
## 1903 1.0621684 1.1241729
## 1904 2.0085059 2.0034190
## 1905 1.1091752 1.0516513
## 1906 0.8682746 0.7891458
## 1907 0.5615456 0.4932325
## 1908 0.8013586 0.6855386
## 1909 0.8927638 0.7492857
## 1910 1.0793069 0.8947335
## 1911 0.7661673 0.6312243
## 1912 1.1126185 0.9160458
## 1913 1.2264724 1.0140502
## 1914 0.6723373 0.5606076
## 1915 0.7661649 0.6465854
## 1916 0.5782920 0.4954261
## 1917 0.6521042 0.5684901
## 1918 0.7020464 0.6239665
## 1919 0.3678325 0.3337640
## 1920 0.5363679 0.4973477
## 1921 0.5597762 0.5307223
## 1922 1.0623959 1.0301470
## 1923 0.7835307 0.7769817
## 1924 0.9375015 0.9504817
## 1925 0.7093926 0.7349532
## 1926 0.7815163 0.8268368
## 1927 1.2835550 1.3856426
## 1928 1.0186018 1.1209709
## 1929 1.7113001 1.9178983
## 1930 1.2324133 1.4050259
## 1931 1.0075832 1.1671209
## 1932 0.9911813 1.1650168
## 1933 0.9946296 1.1846325
## 1934 0.5589489 0.6735988
## 1935 0.7879587 0.9593401
## 1936 0.6621128 0.8131145
## 1937 0.7330784 0.9066023
## 1938 0.9367656 1.1647517
## 1939 0.7639953 0.9534949
## 1940 0.6702711 0.8382938
## 1941 0.6300593 0.7884001
## 1942 0.4631355 0.5789088
## 1943 0.5743693 0.7160952
## 1944 0.5705766 0.7085085
## 1945 0.6033083 0.7451380
## 1946 0.5792738 0.7107390
## 1947 0.6990838 0.8511385
## 1948 0.7807974 0.9423844
## 1949 0.7497408 0.8963011
## 1950 0.8423397 0.9967317
## 1951 0.8073778 0.9450874
## 1952 1.0073180 1.1659531
## 1953 1.2726080 1.4561332
## 1954 1.3162061 1.4884945
## 1955 1.1925481 1.3328596
## 1956 0.9724165 1.0741092
## 1957 0.9335352 1.0191648
## 1958 0.7810686 0.8428978
## 1959 0.9724572 1.0375435
## 1960 1.2673186 1.3371297
## 1961 0.8316603 0.8679798
## 1962 0.9631454 0.9946565
## 1963 1.1265287 1.1515967
## 1964 1.0884125 1.1018051
## 1965 1.3879454 1.3919627
## 1966 1.0425186 1.0362994
## 1967 0.9911734 0.9770266
## 1968 0.8985934 0.8787991
## 1969 1.0300780 0.9999685
## 1970 1.4253066 1.3741745
## 1971 1.4463148 1.3856324
## 1972 1.1856839 1.1293842
## 1973 1.4657681 1.3888587
## 1974 1.4906656 1.4057845
## 1975 1.7046193 1.6007572
## 1976 1.1413409 1.0677467
## 1977 0.7344420 0.6847538
## 1978 0.6589752 0.6125093
## 1979 0.9056050 0.8394010
## 1980 0.8737068 0.8077701
## 1981 0.7982398 0.7362726
## 1982 0.8262484 0.7604694
## 1983 0.7850139 0.7210943
## 1984 1.2821637 1.1756522
## 1985 1.4852250 1.3596527
## 1986 1.6229333 1.4836003
## 1987 1.6167093 1.4760583
## 1988 1.1257837 1.0267008
## 1989 1.0090819 0.9193385
## 1990 1.1771326 1.0714121
## 1991 0.9655133 0.8779577
## 1992 1.0059700 0.9138324
## 1993 1.2541560 1.1380535
## 1994 1.7567520 1.5921975
## 1995 1.2821645 1.1604618
## 1996 1.1646846 1.0524313
## 1997 1.0721012 0.9669104
## 1998 1.4183167 1.2762156
## 1999 1.0440927 0.9369016
## 2000 0.8418095 0.7529095
## 2001 0.8441435 0.7520676
## 2002 0.6099618 0.5409594
## 2003 0.6115178 0.5394861
## 2004 0.4621394 0.4052536
## 2005 0.3298773 0.2873145
## 2006 0.5547229 0.4795198
## 2007 0.6854290 0.5876372
## 2008 1.0635431 0.9037226
## 2009 1.1530145 0.9705028
## 2010 0.8760421 0.7300474
## 2011 0.7336658 0.6050683
## 2012 0.8682619 0.7084187
## 2013 1.2160335 0.9813237
## 2014 1.9847099 1.5839076
## 2015 1.6633907 1.3127237
## 2016 2.1784350 1.7001330
## 2017 2.3908324 1.8453729
## 2018        NA        NA
## 
## $series$P1810Ae
##      ModNegExp    Spline
## 1898 0.5717350 0.8319587
## 1899 1.0811773 1.4701450
## 1900 1.5859083 2.0240395
## 1901 0.4696253 0.5651643
## 1902 1.0295980 1.1739229
## 1903 1.6128136 1.7507347
## 1904 1.4768552 1.5338595
## 1905 1.1832495 1.1816451
## 1906 1.3538534 1.3063928
## 1907 1.2769598 1.1963295
## 1908 0.7060727 0.6452093
## 1909 0.4681925 0.4191428
## 1910 0.4699410 0.4138687
## 1911 0.3808664 0.3312422
## 1912 0.7677592 0.6617500
## 1913 0.8958678 0.7677467
## 1914 0.7408992 0.6331552
## 1915 1.0995150 0.9394229
## 1916 0.7532947 0.6449492
## 1917 1.1559961 0.9937310
## 1918 1.0634919 0.9194175
## 1919 0.6596440 0.5742933
## 1920 0.6695441 0.5876215
## 1921 0.8264150 0.7317098
## 1922 0.6652016 0.5944756
## 1923 0.6000323 0.5413955
## 1924 1.0056441 0.9161721
## 1925 1.2703483 1.1684502
## 1926 0.8198768 0.7611860
## 1927 1.2422270 1.1637060
## 1928 1.0637461 1.0050338
## 1929 1.1472608 1.0926145
## 1930 1.0537801 1.0109897
## 1931 0.6317408 0.6101426
## 1932 1.1084581 1.0769417
## 1933 0.7990256 0.7803511
## 1934 0.7184486 0.7047825
## 1935 1.0716070 1.0551264
## 1936 0.8544093 0.8437972
## 1937 0.8598818 0.8511907
## 1938 0.7270929 0.7209940
## 1939 1.5895910 1.5781536
## 1940 1.1445500 1.1371682
## 1941 1.1357070 1.1288031
## 1942 0.9433563 0.9376832
## 1943 1.0869127 1.0801913
## 1944 0.6796934 0.6752641
## 1945 0.8932687 0.8870732
## 1946 0.9295587 0.9227209
## 1947 1.3541924 1.3437853
## 1948 1.6270602 1.6143216
## 1949 1.5740161 1.5618887
## 1950 1.4914331 1.4806251
## 1951 0.7331534 0.7284550
## 1952 1.1992181 1.1930361
## 1953 0.8397810 0.8368868
## 1954 0.9500729 0.9488919
## 1955 1.5726459 1.5750060
## 1956 1.8713391 1.8803815
## 1957 0.9825591 0.9911885
## 1958 0.7597514 0.7699031
## 1959 1.5773268 1.6066309
## 1960 2.8486831 2.9183186
## 1961 1.1675057 1.2036413
## 1962 0.9227937 0.9578916
## 1963 0.8408210 0.8791731
## 1964 1.0600573 1.1168765
## 1965 0.4332731 0.4600965
## 1966 0.3794506 0.4061817
## 1967 0.6794471 0.7332141
## 1968 0.8948483 0.9735090
## 1969 0.9833357 1.0784248
## 1970 0.7051477 0.7795153
## 1971 0.6513143 0.7256493
## 1972 0.6538912 0.7340852
## 1973 0.9539279 1.0788385
## 1974 0.8423924 0.9594780
## 1975 0.8411211 0.9645466
## 1976 0.7872778 0.9086270
## 1977 1.3296664 1.5439289
## 1978 0.7321563 0.8549366
## 1979 1.1950523 1.4026858
## 1980 1.3027696 1.5362454
## 1981 1.4463905 1.7125653
## 1982 1.2066138 1.4335469
## 1983 0.7885981 0.9393969
## 1984 0.6513974 0.7773296
## 1985 0.8232256 0.9831252
## 1986 1.0360877 1.2369005
## 1987 0.8783687 1.0469723
## 1988 0.5334339 0.6339933
## 1989 0.3269854 0.3869546
## 1990 0.6065269 0.7136024
## 1991 0.3269865 0.3818864
## 1992 0.6001172 0.6946226
## 1993 0.7604058 0.8709146
## 1994 0.5988362 0.6775971
## 1995 0.6385882 0.7127665
## 1996 0.5693442 0.6259139
## 1997 0.4090561 0.4422974
## 1998 0.5693449 0.6046624
## 1999 0.4616313 0.4809470
## 2000 0.4359853 0.4450898
## 2001 0.5449819 0.5446262
## 2002 1.0348247 1.0114723
## 2003 0.8142676 0.7778950
## 2004 0.7052713 0.6581668
## 2005 0.5975573 0.5445110
## 2006 1.2476896 1.1098518
## 2007 1.1425403 0.9919910
## 2008 1.9850198 1.6822187
## 2009 2.4915336 2.0611969
## 2010 1.8285781 1.4770357
## 2011 1.1604933 0.9154899
## 2012 1.1720342 0.9032405
## 2013 1.2092214 0.9106443
## 2014 0.7873404 0.5795920
## 2015 0.9566058 0.6885846
## 2016 1.9401403 1.3661052
## 2017 2.3915147 1.6478953
## 2018 2.2466134 1.5155825
## 
## $series$P1810Be
##      ModNegExp    Spline
## 1898        NA        NA
## 1899        NA        NA
## 1900        NA        NA
## 1901        NA        NA
## 1902        NA        NA
## 1903        NA        NA
## 1904        NA        NA
## 1905        NA        NA
## 1906        NA        NA
## 1907 0.9609490 2.3048863
## 1908 1.1135813 2.0353805
## 1909 0.9929564 1.4181599
## 1910 1.1492538 1.3216830
## 1911 0.5461203 0.5232494
## 1912 0.6355080 0.5260309
## 1913 1.1175902 0.8286124
## 1914 1.2108861 0.8317483
## 1915 1.0318399 0.6762862
## 1916 1.5431227 0.9888836
## 1917 1.8770676 1.1989883
## 1918 1.2474691 0.8059380
## 1919 1.0139236 0.6696770
## 1920 1.0179859 0.6926067
## 1921 0.4894178 0.3447898
## 1922 0.5930899 0.4340739
## 1923 0.5640019 0.4296519
## 1924 0.7856145 0.6234350
## 1925 0.6950204 0.5745237
## 1926 0.5016257 0.4316233
## 1927 0.7726808 0.6911640
## 1928 0.7757843 0.7201381
## 1929 1.2077873 1.1609769
## 1930 1.1053956 1.0975464
## 1931 0.7486703 0.7656738
## 1932 0.6917126 0.7264036
## 1933 0.5558588 0.5973936
## 1934 0.2421550 0.2653961
## 1935 0.5495099 0.6119092
## 1936 0.5455111 0.6149076
## 1937 0.6219658 0.7070690
## 1938 0.8118712 0.9274810
## 1939 0.6702659 0.7668065
## 1940 0.6557887 0.7488697
## 1941 0.6598163 0.7498176
## 1942 0.5037157 0.5680867
## 1943 0.5882066 0.6567427
## 1944 0.7298284 0.8050213
## 1945 1.1860735 1.2902010
## 1946 0.9792760 1.0490612
## 1947 1.0074400 1.0616939
## 1948 0.9833005 1.0186398
## 1949 0.6099362 0.6208253
## 1950 0.8851319 0.8850335
## 1951 0.8915693 0.8758051
## 1952 1.6962350 1.6375177
## 1953 1.7171564 1.6300831
## 1954 1.6439319 1.5357591
## 1955 1.7179612 1.5808837
## 1956 1.9786729 1.7954302
## 1957 1.6696813 1.4956634
## 1958 1.0838847 0.9596385
## 1959 1.4443750 1.2654893
## 1960 1.9062531 1.6548273
## 1961 1.7227893 1.4836876
## 1962 1.7002587 1.4544614
## 1963 1.3655177 1.1616760
## 1964 1.4089697 1.1934138
## 1965 1.2528645 1.0577221
## 1966 0.8473130 0.7137381
## 1967 1.3148238 1.1061500
## 1968 1.2713719 1.0692365
## 1969 1.2190686 1.0258106
## 1970 1.2625205 1.0638437
## 1971 0.7990331 0.6747552
## 1972 1.0589401 0.8968437
## 1973 1.0372141 0.8816274
## 1974 1.4113837 1.2048415
## 1975 1.2327479 1.0575849
## 1976 1.2673485 1.0933768
## 1977 1.1530860 1.0009950
## 1978 0.7185665 0.6280332
## 1979 1.1418207 1.0052968
## 1980 1.0066368 0.8932589
## 1981 1.2657392 1.1326041
## 1982 1.1096340 1.0017495
## 1983 0.8545550 0.7787075
## 1984 1.3526431 1.2447315
## 1985 1.5111622 1.4049500
## 1986 1.4886316 1.3988950
## 1987 1.1723980 1.1140239
## 1988 0.8030564 0.7718526
## 1989 0.6389046 0.6213135
## 1990 0.5713127 0.5622455
## 1991 0.6791379 0.6764860
## 1992 1.0790567 1.0880634
## 1993 1.0959547 1.1188364
## 1994 1.3011445 1.3449698
## 1995 1.0315815 1.0797981
## 1996 1.0766427 1.1412614
## 1997 1.1611326 1.2464411
## 1998 1.5964568 1.7354035
## 1999 0.7982284 0.8785518
## 2000 1.0718148 1.1941351
## 2001 0.6952312 0.7837948
## 2002 0.6928172 0.7899915
## 2003 0.4578548 0.5277157
## 2004 0.3637089 0.4234315
## 2005 0.3283036 0.3857539
## 2006 0.4449801 0.5272316
## 2007 0.3596856 0.4293549
## 2008 0.4522221 0.5433541
## 2009 0.4940648 0.5969934
## 2010 0.6847705 0.8314306
## 2011 0.5391260 0.6572659
## 2012 0.7233945 0.8849343
## 2013 0.7016685 0.8608275
## 2014 0.7523625 0.9252851
## 2015 1.1949286 1.4727431
## 2016 1.7767019 2.1941448
## 2017 1.6004801 1.9803492
## 2018        NA        NA
## 
## $series$P1810Ce
##      ModNegExp    Spline
## 1898 0.7724789 1.1198086
## 1899 0.5043293 0.6851198
## 1900 1.1247579 1.4369168
## 1901 1.1169550 1.3469572
## 1902 1.7792908 2.0334393
## 1903 1.8111535 1.9697547
## 1904 1.3051223 1.3566045
## 1905 1.3553761 1.3524549
## 1906 0.7817662 0.7522122
## 1907 0.5971039 0.5564911
## 1908 0.6939386 0.6292293
## 1909 0.7159158 0.6343708
## 1910 0.8229653 0.7157082
## 1911 0.4907805 0.4206757
## 1912 0.6857852 0.5817356
## 1913 1.0722071 0.9036364
## 1914 0.7806247 0.6560735
## 1915 1.1522202 0.9690863
## 1916 0.8293489 0.7003170
## 1917 0.9341998 0.7943669
## 1918 0.8006383 0.6873932
## 1919 0.4488375 0.3900061
## 1920 0.5546897 0.4887975
## 1921 0.6564756 0.5876731
## 1922 0.9908869 0.9023653
## 1923 0.8479814 0.7864164
## 1924 1.0456962 0.9883526
## 1925 0.6074741 0.5854292
## 1926 0.6800048 0.6682982
## 1927 1.1011408 1.1034786
## 1928 1.2628956 1.2900054
## 1929 1.3947136 1.4512641
## 1930 1.0803771 1.1442069
## 1931 0.7237999 0.7793684
## 1932 0.8112753 0.8869865
## 1933 0.4251780 0.4712880
## 1934 0.3754048 0.4211634
## 1935 0.4176146 0.4733410
## 1936 0.4708729 0.5381805
## 1937 0.6593423 0.7584474
## 1938 1.0048524 1.1611205
## 1939 0.9099665 1.0542711
## 1940 0.9339332 1.0829695
## 1941 0.7822943 0.9063670
## 1942 0.5591776 0.6462828
## 1943 0.8285949 0.9539226
## 1944 0.8595344 0.9843613
## 1945 1.4869213 1.6919611
## 1946 1.0232853 1.1557667
## 1947 1.3991356 1.5672090
## 1948 1.0217610 1.1342033
## 1949 0.6402438 0.7038719
## 1950 0.8883301 0.9667413
## 1951 0.6942312 0.7475840
## 1952 1.6148585 1.7202615
## 1953 1.5941889 1.6797301
## 1954 1.4488927 1.5099139
## 1955 1.0783610 1.1114801
## 1956 1.1093755 1.1310021
## 1957 0.8999036 0.9075503
## 1958 0.6572574 0.6557788
## 1959 0.6928628 0.6840532
## 1960 0.8781822 0.8581074
## 1961 0.9275022 0.8972316
## 1962 0.7875755 0.7544992
## 1963 0.8054412 0.7644505
## 1964 0.9989112 0.9397095
## 1965 1.1255500 1.0500692
## 1966 1.0636489 0.9847055
## 1967 1.3345484 1.2268690
## 1968 1.5369814 1.4041732
## 1969 1.4952051 1.3586299
## 1970 1.6766973 1.5166509
## 1971 1.4406100 1.2983933
## 1972 1.7736650 1.5942933
## 1973 1.8777699 1.6849533
## 1974 1.9859053 1.7805785
## 1975 1.7529619 1.5719053
## 1976 1.6167541 1.4511707
## 1977 0.7660691 0.6888087
## 1978 0.6636777 0.5981922
## 1979 0.8854649 0.8005141
## 1980 0.9653247 0.8758249
## 1981 0.9427637 0.8588145
## 1982 1.0282686 0.9408898
## 1983 0.9782826 0.8994845
## 1984 1.1508900 1.0636566
## 1985 1.4412522 1.3392677
## 1986 1.8340486 1.7139507
## 1987 1.2444879 1.1697979
## 1988 0.8073516 0.7633973
## 1989 0.7210570 0.6858331
## 1990 0.5960457 0.5702208
## 1991 0.6646072 0.6393836
## 1992 0.8283439 0.8011782
## 1993 1.2114690 1.1776541
## 1994 1.3413326 1.3100028
## 1995 0.9775715 0.9588124
## 1996 1.1025947 1.0855286
## 1997 1.0211332 1.0085790
## 1998 0.9501565 0.9409291
## 1999 1.1017969 1.0932004
## 2000 0.8880534 0.8821592
## 2001 0.7493217 0.7446117
## 2002 0.6428529 0.6384758
## 2003 0.4726629 0.4687613
## 2004 0.3702262 0.3662831
## 2005 0.3774860 0.3722014
## 2006 0.7993354 0.7847274
## 2007 0.5396124 0.5269711
## 2008 0.5734899 0.5566370
## 2009 0.6638291 0.6398840
## 2010 0.7485222 0.7160508
## 2011 0.7001268 0.6642770
## 2012 0.7727209 0.7268015
## 2013 0.9937292 0.9262340
## 2014 1.3421802 1.2394138
## 2015 2.3189720 2.1212928
## 2016 2.0382762 1.8469729
## 2017 2.0665077 1.8550215
## 2018        NA        NA
## 
## $series$P1811Ae
##      ModNegExp    Spline
## 1898 0.8643980 1.0643435
## 1899 1.1301417 1.3519996
## 1900 1.0170626 1.1832684
## 1901 0.7990338 0.9049506
## 1902 1.1854290 1.3082940
## 1903 0.9688051 1.0430293
## 1904 1.1054543 1.1622581
## 1905 1.0640436 1.0937144
## 1906 0.9366083 0.9422655
## 1907 0.8120810 0.8005319
## 1908 0.9616563 0.9299541
## 1909 0.9207153 0.8744491
## 1910 1.0912532 1.0190961
## 1911 0.8359527 0.7685519
## 1912 1.1521053 1.0440361
## 1913 1.3477048 1.2052859
## 1914 0.9633071 0.8512969
## 1915 1.1329349 0.9905969
## 1916 1.0921836 0.9460702
## 1917 0.9294427 0.7986382
## 1918 0.8147607 0.6953835
## 1919 0.9657708 0.8197977
## 1920 0.8530843 0.7211759
## 1921 0.8241536 0.6948023
## 1922 1.1321690 0.9531639
## 1923 1.1773483 0.9912427
## 1924 1.2439955 1.0489268
## 1925 1.1643604 0.9847250
## 1926 1.2224766 1.0385694
## 1927 1.2047403 1.0297649
## 1928 1.1705227 1.0082571
## 1929 1.6138989 1.4032178
## 1930 1.7863458 1.5703377
## 1931 1.0996944 0.9790442
## 1932 1.2960089 1.1704579
## 1933 0.5363346 0.4921512
## 1934 0.5812405 0.5427574
## 1935 0.8939404 0.8507266
## 1936 0.6856106 0.6658960
## 1937 0.9579297 0.9508076
## 1938 0.8370315 0.8501117
## 1939 0.9050231 0.9416067
## 1940 0.6999238 0.7467657
## 1941 0.6374687 0.6980761
## 1942 1.0008517 1.1257502
## 1943 0.6752863 0.7806032
## 1944 0.4566485 0.5426856
## 1945 0.4954441 0.6053994
## 1946 0.3385281 0.4252836
## 1947 0.8101721 1.0460420
## 1948 0.5801854 0.7694371
## 1949 0.4514162 0.6144057
## 1950 0.5126165 0.7152701
## 1951 0.5796873 0.8281178
## 1952 0.8124265 1.1863838
## 1953 0.4209896 0.6273128
## 1954 0.4142644 0.6286434
## 1955 0.8727212 1.3458196
## 1956 1.0812325 1.6905601
## 1957 0.7838122 1.2396386
## 1958 0.5970661 0.9528193
## 1959 0.7619932 1.2239170
## 1960 0.8195318 1.3215312
## 1961 0.7903887 1.2763377
## 1962 0.6969662 1.1242649
## 1963 0.8875478 1.4266834
## 1964 0.8027032 1.2827915
## 1965 0.7628964 1.2093863
## 1966 0.6155651 0.9659591
## 1967 0.8138291 1.2616842
## 1968 0.6613006 1.0110303
## 1969 0.6213125 0.9352111
## 1970 0.7892251 1.1678748
## 1971 0.5330027 0.7743916
## 1972 0.6190683 0.8821133
## 1973 0.7816571 1.0913312
## 1974 0.6597149 0.9018465
## 1975 0.9183389 1.2285023
## 1976 1.4936070 1.9545321
## 1977 1.4009475 1.7929439
## 1978 1.4289377 1.7883518
## 1979 1.5245113 1.8657659
## 1980 1.0039407 1.2015287
## 1981 1.0649726 1.2464779
## 1982 1.0448144 1.1959910
## 1983 0.5420997 0.6069202
## 1984 0.5846625 0.6402350
## 1985 0.9209783 0.9864731
## 1986 1.0479764 1.0980341
## 1987 0.9261442 0.9493040
## 1988 0.5861953 0.5878564
## 1989 0.5992979 0.5880584
## 1990 0.5395576 0.5181113
## 1991 0.6890749 0.6476433
## 1992 1.1228659 1.0331937
## 1993 1.5136450 1.3639215
## 1994 1.8118290 1.5993574
## 1995 2.0960605 1.8132741
## 1996 1.7063492 1.4472161
## 1997 1.3432365 1.1173581
## 1998 1.5743288 1.2848888
## 1999 1.3369550 1.0709212
## 2000 1.0297954 0.8098131
## 2001 1.1080548 0.8556436
## 2002 1.2458791 0.9449212
## 2003 1.2976296 0.9668009
## 2004 0.6535641 0.4784209
## 2005 0.5562604 0.4001265
## 2006 1.3779231 0.9740999
## 2007 1.2309707 0.8553738
## 2008 1.6553395 1.1308509
## 2009 1.9761010 1.3274808
## 2010 1.4419187 0.9527072
## 2011 1.5962969 1.0376104
## 2012 1.5802403 1.0107670
## 2013 1.8842658 1.1862744
## 2014 1.6779294 1.0400183
## 2015 1.7296877 1.0557663
## 2016 1.5558483 0.9354203
## 2017 1.8857472 1.1170352
## 2018        NA        NA
## 
## $series$P1811Be
##      ModNegExp    Spline
## 1898 1.1059304 1.3491536
## 1899 0.9679945 1.1499673
## 1900 0.9132095 1.0574125
## 1901 0.9784512 1.1052666
## 1902 1.4452655 1.5941564
## 1903 1.0131429 1.0922373
## 1904 0.9162904 0.9663808
## 1905 0.9653232 0.9969125
## 1906 0.9750908 0.9869417
## 1907 0.6948897 0.6899352
## 1908 0.5578894 0.5438263
## 1909 0.5886820 0.5638691
## 1910 0.8352799 0.7868256
## 1911 0.7406558 0.6867181
## 1912 0.9483898 0.8662398
## 1913 1.0537214 0.9489675
## 1914 0.9220528 0.8195112
## 1915 1.2284290 1.0785446
## 1916 1.0289057 0.8932788
## 1917 1.1280881 0.9694654
## 1918 1.1623478 0.9898736
## 1919 1.1297532 0.9545143
## 1920 1.0988806 0.9222137
## 1921 1.3336790 1.1131930
## 1922 1.6422979 1.3652016
## 1923 1.3068673 1.0834824
## 1924 1.4910287 1.2347284
## 1925 1.3490826 1.1176291
## 1926 1.6193777 1.3442728
## 1927 1.7027067 1.4187041
## 1928 1.5546267 1.3024029
## 1929 1.6590280 1.3999425
## 1930 1.3895181 1.1831414
## 1931 0.8624483 0.7423362
## 1932 0.8302737 0.7236977
## 1933 0.8694251 0.7687802
## 1934 0.6376314 0.5729700
## 1935 1.1587955 1.0600131
## 1936 0.9788068 0.9130328
## 1937 1.1342444 1.0807182
## 1938 1.1569711 1.1278718
## 1939 0.4829783 0.4824891
## 1940 0.8039441 0.8242665
## 1941 1.0357734 1.0914723
## 1942 0.4141792 0.4491858
## 1943 0.5534133 0.6184591
## 1944 0.5104468 0.5884585
## 1945 0.6181399 0.7358330
## 1946 0.4742138 0.5833853
## 1947 0.7266921 0.9245197
## 1948 0.5373987 0.7074107
## 1949 0.5084773 0.6927918
## 1950 0.6215858 0.8767025
## 1951 0.5238662 0.7648338
## 1952 0.6665811 1.0071109
## 1953 0.5200388 0.8126830
## 1954 0.5742504 0.9275285
## 1955 0.5370708 0.8957148
## 1956 0.7344941 1.2632824
## 1957 0.7747879 1.3722044
## 1958 0.6600948 1.2017117
## 1959 0.7658707 1.4302799
## 1960 0.9507396 1.8171487
## 1961 0.6081135 1.1864348
## 1962 0.5135364 1.0197632
## 1963 0.5695334 1.1474442
## 1964 0.5768954 1.1751627
## 1965 0.4239817 0.8700510
## 1966 0.3473082 0.7152200
## 1967 0.3505550 0.7215808
## 1968 0.3549661 0.7274009
## 1969 0.3566852 0.7247558
## 1970 0.3595729 0.7216263
## 1971 0.3070056 0.6062554
## 1972 0.4234068 0.8198001
## 1973 0.4517601 0.8548098
## 1974 0.4843634 0.8929856
## 1975 0.5172133 0.9266024
## 1976 0.5203230 0.9037127
## 1977 0.4945291 0.8310177
## 1978 0.3498409 0.5678316
## 1979 0.4693228 0.7347828
## 1980 0.5898762 0.8898762
## 1981 0.6513241 0.9460603
## 1982 0.6233163 0.8713112
## 1983 0.4779058 0.6427587
## 1984 0.6595579 0.8534836
## 1985 1.1436657 1.4241775
## 1986 2.5493111 3.0561687
## 1987 1.4849943 1.7147293
## 1988 1.0333702 1.1499993
## 1989 1.2481480 1.3395036
## 1990 1.0092037 1.0451108
## 1991 0.8889130 0.8888264
## 1992 1.0146004 0.9801506
## 1993 1.0476549 0.9784080
## 1994 1.6684896 1.5072706
## 1995 1.4923876 1.3048901
## 1996 1.5497061 1.3122454
## 1997 1.2152168 0.9970737
## 1998 1.0601635 0.8432786
## 1999 0.9677739 0.7466162
## 2000 1.0107157 0.7565990
## 2001 1.0078915 0.7323923
## 2002 0.9413273 0.6642631
## 2003 0.8166844 0.5598869
## 2004 0.8786388 0.5854443
## 2005 0.8827337 0.5719103
## 2006 1.8092628 1.1403370
## 2007 1.3555205 0.8315735
## 2008 2.0533763 1.2267978
## 2009 2.7182485 1.5825823
## 2010 1.8662176 1.0594622
## 2011 2.1849827 1.2102995
## 2012 2.6299410 1.4222870
## 2013 2.6006390 1.3740033
## 2014 2.4107559 1.2450506
## 2015 2.1619706 1.0920782
## 2016 1.4007763 0.6924186
## 2017 1.5579407 0.7539716
## 2018        NA        NA
## 
## $series$P1811Ce
##      ModNegExp    Spline
## 1898 0.9748943 1.2263268
## 1899 0.9217338 1.1213103
## 1900 0.8771228 1.0331665
## 1901 0.9998443 1.1417507
## 1902 1.2353129 1.3693217
## 1903 1.0995593 1.1847227
## 1904 0.9383574 0.9840798
## 1905 1.1021273 1.1265839
## 1906 1.1998755 1.1971628
## 1907 0.9060253 0.8836187
## 1908 1.0667726 1.0184256
## 1909 0.8520336 0.7973957
## 1910 1.0008018 0.9194997
## 1911 0.7838364 0.7080134
## 1912 0.8548895 0.7602631
## 1913 1.0058369 0.8819521
## 1914 0.8099868 0.7012715
## 1915 0.9905872 0.8480489
## 1916 0.8281284 0.7020668
## 1917 0.9948016 0.8363866
## 1918 0.8889852 0.7423337
## 1919 0.7110146 0.5905682
## 1920 0.9302355 0.7697259
## 1921 1.2038931 0.9939508
## 1922 1.8703202 1.5432281
## 1923 1.3112332 1.0830688
## 1924 1.2257861 1.0152964
## 1925 1.2050848 1.0026522
## 1926 1.2760269 1.0683435
## 1927 1.2840323 1.0837263
## 1928 1.1864649 1.0112862
## 1929 1.7469764 1.5065067
## 1930 1.6233864 1.4189389
## 1931 1.2049293 1.0694212
## 1932 1.2298154 1.1103125
## 1933 0.8983997 0.8265014
## 1934 0.6973886 0.6548466
## 1935 0.6686793 0.6418935
## 1936 0.6242474 0.6135341
## 1937 0.7871365 0.7932180
## 1938 0.8696897 0.8998263
## 1939 1.0144843 1.0790699
## 1940 0.9235879 1.0111467
## 1941 0.8236610 0.9291723
## 1942 1.1539873 1.3427448
## 1943 0.7643153 0.9180991
## 1944 0.5690921 0.7062217
## 1945 0.5227702 0.6705913
## 1946 0.4753034 0.6304946
## 1947 0.5768374 0.7914584
## 1948 1.1414388 1.6200137
## 1949 0.4181240 0.6137797
## 1950 0.6386019 0.9692635
## 1951 0.6957821 1.0913453
## 1952 1.2967111 2.1003428
## 1953 0.3347281 0.5593426
## 1954 0.4813335 0.8287748
## 1955 0.6626786 1.1739456
## 1956 0.6911099 1.2574239
## 1957 0.5407135 1.0083475
## 1958 0.5528927 1.0543726
## 1959 0.5790979 1.1264292
## 1960 0.4956934 0.9807208
## 1961 0.5111416 1.0255039
## 1962 0.5008552 1.0157108
## 1963 0.5036945 1.0289981
## 1964 0.4949393 1.0149941
## 1965 0.4085906 0.8381174
## 1966 0.2225778 0.4550200
## 1967 0.3585376 0.7278681
## 1968 0.3483725 0.6998516
## 1969 0.3943695 0.7813483
## 1970 0.4299437 0.8374296
## 1971 0.3414885 0.6519602
## 1972 0.3436545 0.6413529
## 1973 0.5019467 0.9134910
## 1974 0.5854489 1.0367528
## 1975 0.6303096 1.0841127
## 1976 0.8164281 1.3617264
## 1977 0.8520466 1.3763173
## 1978 0.6466871 1.0105729
## 1979 0.7518068 1.1355961
## 1980 0.7411887 1.0814323
## 1981 0.8817916 1.2421448
## 1982 0.8601858 1.1694521
## 1983 0.6403655 0.8400521
## 1984 0.6655383 0.8423488
## 1985 1.1673805 1.4255064
## 1986 1.3566812 1.5984910
## 1987 1.2875066 1.4639478
## 1988 0.9024805 0.9904754
## 1989 0.6806143 0.7211613
## 1990 0.5159516 0.5279216
## 1991 0.8695882 0.8594451
## 1992 0.9192374 0.8778223
## 1993 1.2247735 1.1304621
## 1994 1.2134413 1.0829351
## 1995 1.1908932 1.0280492
## 1996 1.1671454 0.9750035
## 1997 1.1098547 0.8975878
## 1998 1.2198622 0.9555354
## 1999 1.5019116 1.1400061
## 2000 1.7864339 1.3145691
## 2001 1.4240008 1.0163639
## 2002 1.5308527 1.0602821
## 2003 1.5985970 1.0749268
## 2004 0.7352541 0.4802059
## 2005 0.6280431 0.3985877
## 2006 1.1254782 0.6944053
## 2007 1.1739979 0.7045187
## 2008 1.6483328 0.9625906
## 2009 2.1834293 1.2414989
## 2010 2.0685061 1.1458520
## 2011 2.1116105 1.1402876
## 2012 3.1047448 1.6354083
## 2013 2.5747350 1.3237481
## 2014 2.5864550 1.2987234
## 2015 2.4174546 1.1862149
## 2016 1.8476341 0.8864478
## 2017 1.4751976 0.6923750
## 2018        NA        NA
## 
## $series$P1812Ae
##      ModNegExp    Spline
## 1898 1.0266998 1.5584049
## 1899 0.9176719 1.2881042
## 1900 0.8054521 1.0512966
## 1901 0.7009517 0.8556780
## 1902 1.1493803 1.3202007
## 1903 1.5239417 1.6572894
## 1904 1.1434226 1.1847999
## 1905 1.1945713 1.1869504
## 1906 1.3021576 1.2486092
## 1907 1.0454714 0.9734886
## 1908 1.3275541 1.2077019
## 1909 1.2297674 1.0993671
## 1910 0.9278700 0.8196031
## 1911 0.6702521 0.5879920
## 1912 0.8414633 0.7365847
## 1913 1.0081249 0.8842985
## 1914 0.8490417 0.7491164
## 1915 1.0450864 0.9305398
## 1916 0.8724075 0.7860947
## 1917 0.6546492 0.5983111
## 1918 0.7053296 0.6550038
## 1919 0.4811370 0.4545778
## 1920 0.6073585 0.5842747
## 1921 0.6768595 0.6632058
## 1922 0.7545038 0.7529139
## 1923 0.7239084 0.7353259
## 1924 0.5078327 0.5246155
## 1925 0.3242507 0.3402392
## 1926 0.2935566 0.3123935
## 1927 0.3071163 0.3308496
## 1928 0.3816917 0.4154142
## 1929 0.5321132 0.5838141
## 1930 0.6228099 0.6873082
## 1931 0.4930764 0.5460656
## 1932 0.6019710 0.6675067
## 1933 0.7172826 0.7946232
## 1934 0.4556745 0.5032710
## 1935 0.6851719 0.7529514
## 1936 0.5951542 0.6495823
## 1937 0.8528927 0.9230761
## 1938 0.8866118 0.9501856
## 1939 1.1346867 1.2027403
## 1940 0.9066283 0.9495796
## 1941 0.9935615 1.0274984
## 1942 0.6003982 0.6127436
## 1943 1.0718073 1.0791114
## 1944 1.0012754 0.9943949
## 1945 1.6707997 1.6368770
## 1946 1.4036021 1.3568489
## 1947 1.6512174 1.5756690
## 1948 2.0853877 1.9654241
## 1949 1.1232948 1.0462941
## 1950 1.2069866 1.1119059
## 1951 1.3752095 1.2539612
## 1952 1.8548223 1.6754754
## 1953 1.9952722 1.7871095
## 1954 1.9356577 1.7206636
## 1955 1.8275326 1.6138289
## 1956 1.4728604 1.2932328
## 1957 1.1738245 1.0257050
## 1958 1.0985864 0.9561354
## 1959 1.3410606 1.1634337
## 1960 1.4391194 1.2454378
## 1961 1.0368211 0.8957055
## 1962 1.0079648 0.8698127
## 1963 1.0853699 0.9361378
## 1964 0.9770611 0.8427725
## 1965 0.9450951 0.8156806
## 1966 0.6727208 0.5812348
## 1967 0.9771098 0.8455497
## 1968 1.3723041 1.1899484
## 1969 1.3393004 1.1642322
## 1970 1.2918485 1.1263044
## 1971 0.7542721 0.6598545
## 1972 1.0700211 0.9396711
## 1973 1.2144861 1.0710861
## 1974 1.2712442 1.1264012
## 1975 1.2423576 1.1064338
## 1976 1.3961097 1.2502364
## 1977 1.3249154 1.1935164
## 1978 0.9689245 0.8783407
## 1979 1.0267115 0.9369329
## 1980 1.4487497 1.3313253
## 1981 1.1618910 1.0755247
## 1982 0.9183700 0.8565567
## 1983 0.9194030 0.8642338
## 1984 1.0669627 1.0109981
## 1985 1.3342206 1.2746183
## 1986 1.4755894 1.4214479
## 1987 1.3383502 1.3001426
## 1988 1.0153720 0.9947631
## 1989 0.6624686 0.6545158
## 1990 0.4726025 0.4708328
## 1991 0.6934257 0.6964901
## 1992 0.8265390 0.8368163
## 1993 1.2423885 1.2675561
## 1994 1.0525221 1.0818331
## 1995 0.9895774 1.0243696
## 1996 0.7687542 0.8011450
## 1997 0.8616240 0.9036013
## 1998 1.3404188 1.4139559
## 1999 0.8688474 0.9214213
## 2000 0.9235374 0.9841248
## 2001 0.7243836 0.7751486
## 2002 0.6139719 0.6593325
## 2003 0.6088125 0.6556634
## 2004 0.4055311 0.4376763
## 2005 0.3529049 0.3814197
## 2006 0.6366735 0.6886009
## 2007 0.8471782 0.9162909
## 2008 1.1237237 1.2146395
## 2009 0.6779490 0.7319081
## 2010 0.5479314 0.5904995
## 2011 0.6552475 0.7045605
## 2012 1.0597468 1.1364480
## 2013 0.8647204 0.9244976
## 2014 0.9431438 1.0050116
## 2015 1.0845121 1.1516055
## 2016 1.4559911 1.5404552
## 2017 1.5447333 1.6283144
## 2018 1.5870406 1.6667138
## 
## $series$P1812Be
##      ModNegExp    Spline
## 1898        NA        NA
## 1899        NA        NA
## 1900        NA        NA
## 1901        NA        NA
## 1902        NA        NA
## 1903        NA        NA
## 1904        NA        NA
## 1905        NA        NA
## 1906 0.9068280 1.1335747
## 1907 1.0884686 1.3127983
## 1908 0.9968964 1.1617828
## 1909 0.8921391 1.0061517
## 1910 0.9818204 1.0732644
## 1911 0.4516959 0.4793768
## 1912 1.1200346 1.1559967
## 1913 0.8297227 0.8343035
## 1914 0.8270398 0.8116903
## 1915 1.6767637 1.6093671
## 1916 1.6511846 1.5530571
## 1917 1.6520560 1.5259803
## 1918 1.3593694 1.2357783
## 1919 1.3689290 1.2274947
## 1920 1.1399782 1.0104797
## 1921 0.9759201 0.8570070
## 1922 0.8826011 0.7694883
## 1923 0.6571282 0.5699794
## 1924 0.8076216 0.6983347
## 1925 0.6494751 0.5609303
## 1926 0.3759487 0.3249193
## 1927 0.5117795 0.4434095
## 1928 0.7599778 0.6612133
## 1929 1.3207192 1.1558050
## 1930 0.5993975 0.5284543
## 1931 0.5077128 0.4516238
## 1932 2.1074981 1.8941004
## 1933 0.8607488 0.7826412
## 1934 1.1072304 1.0197473
## 1935 1.1884785 1.1098563
## 1936 0.6478324 0.6139544
## 1937 0.6809827 0.6553881
## 1938 0.4010952 0.3921924
## 1939 0.4918449 0.4887405
## 1940 0.8019478 0.8098737
## 1941 0.9296529 0.9540143
## 1942 0.4225190 0.4404604
## 1943 0.4595494 0.4864158
## 1944 0.5622974 0.6039223
## 1945 0.9601177 1.0455752
## 1946 0.7756921 0.8558127
## 1947 1.0000206 1.1168022
## 1948 2.0663199 2.3337272
## 1949 0.8128588 0.9275832
## 1950 0.8830956 1.0172165
## 1951 0.5977544 0.6943311
## 1952 0.9143421 1.0699368
## 1953 1.0265285 1.2089376
## 1954 1.0445430 1.2369139
## 1955 1.1939746 1.4203941
## 1956 1.2191257 1.4558250
## 1957 1.0854097 1.3000858
## 1958 0.9430001 1.1321398
## 1959 1.2782075 1.5371371
## 1960 1.0519385 1.2663614
## 1961 0.6551893 0.7891123
## 1962 1.1408237 1.3739075
## 1963 0.9131575 1.0990859
## 1964 1.1638052 1.3992933
## 1965 1.1028245 1.3239961
## 1966 0.7732315 0.9265303
## 1967 1.6693294 1.9956511
## 1968 0.8653842 1.0317375
## 1969 0.9006772 1.0704398
## 1970 1.1018135 1.3047761
## 1971 0.7141415 0.8422384
## 1972 0.6778895 0.7957996
## 1973 0.5964471 0.6965759
## 1974 0.7237406 0.8403902
## 1975 0.6790966 0.7835744
## 1976 0.9689218 1.1103033
## 1977 0.9658808 1.0986084
## 1978 0.6902999 0.7789230
## 1979 1.1355018 1.2704596
## 1980 0.7029242 0.7794489
## 1981 0.9689576 1.0643607
## 1982 1.1882125 1.2923891
## 1983 0.6897240 0.7425239
## 1984 0.8917909 0.9498657
## 1985 0.8567303 0.9024976
## 1986 1.1765501 1.2253678
## 1987 0.7341482 0.7557157
## 1988 0.8177862 0.8317807
## 1989 0.6954265 0.6987228
## 1990 0.8967174 0.8898203
## 1991 0.9439257 0.9249303
## 1992 1.1051501 1.0692353
## 1993 1.3091286 1.2505373
## 1994 1.1882407 1.1206891
## 1995 0.9838253 0.9161920
## 1996 0.7403097 0.6807780
## 1997 0.5332829 0.4843113
## 1998 1.1079406 0.9938739
## 1999 0.8251002 0.7312585
## 2000 1.3150037 1.1517883
## 2001 2.5088199 2.1725054
## 2002 1.4017447 1.2005930
## 2003 1.4489949 1.2280784
## 2004 1.1129381 0.9338103
## 2005 0.9478872 0.7877069
## 2006 1.8126722 1.4925757
## 2007 0.9893845 0.8075676
## 2008 1.9793270 1.6021713
## 2009 1.4049888 1.1282763
## 2010 1.2435520 0.9910970
## 2011 1.3627576 1.0782520
## 2012 1.4995432 1.1782339
## 2013 1.3691608 1.0685618
## 2014 1.0768001 0.8348967
## 2015 0.8721208 0.6718681
## 2016 0.9112843 0.6976052
## 2017 1.0402250 0.7913314
## 2018        NA        NA
## 
## $series$P1812De
##      ModNegExp    Spline
## 1898        NA        NA
## 1899        NA        NA
## 1900        NA        NA
## 1901        NA        NA
## 1902        NA        NA
## 1903        NA        NA
## 1904        NA        NA
## 1905 0.9462316 1.6765831
## 1906 1.1944713 1.8579931
## 1907 0.9301410 1.2809025
## 1908 0.8721707 1.0731548
## 1909 0.9845375 1.0931467
## 1910 1.0447846 1.0577718
## 1911 0.8590099 0.8016492
## 1912 0.9417513 0.8190773
## 1913 0.9963652 0.8165373
## 1914 0.9754772 0.7613741
## 1915 1.3089041 0.9830244
## 1916 1.2079812 0.8813785
## 1917 0.9082731 0.6495138
## 1918 0.9533191 0.6734611
## 1919 0.8335714 0.5857859
## 1920 0.9447630 0.6644221
## 1921 1.0719826 0.7582853
## 1922 1.4019371 1.0016436
## 1923 1.0781477 0.7806619
## 1924 1.1580245 0.8519902
## 1925 0.8158919 0.6111063
## 1926 0.7433001 0.5675261
## 1927 1.1567720 0.9010449
## 1928 1.1840197 0.9411900
## 1929 1.5506007 1.2577987
## 1930 1.4404385 1.1918575
## 1931 1.0998846 0.9276601
## 1932 1.0469680 0.8992132
## 1933 0.8890153 0.7766034
## 1934 0.4955443 0.4396642
## 1935 0.7511938 0.6758661
## 1936 0.7265845 0.6618238
## 1937 1.1077832 1.0197956
## 1938 1.1319311 1.0513062
## 1939 1.0710807 1.0019290
## 1940 0.8560248 0.8051484
## 1941 0.7413513 0.6999730
## 1942 0.4291754 0.4061506
## 1943 0.7434173 0.7041344
## 1944 0.8474114 0.8022817
## 1945 1.0148431 0.9593005
## 1946 1.1145377 1.0509172
## 1947 1.2646998 1.1886598
## 1948 1.3441969 1.2586093
## 1949 1.0212414 0.9522591
## 1950 1.4555449 1.3513447
## 1951 1.0704598 0.9894915
## 1952 2.0631393 1.8990017
## 1953 2.1541383 1.9748796
## 1954 2.1195950 1.9361984
## 1955 1.7791324 1.6200135
## 1956 1.2784696 1.1609207
## 1957 1.1933603 1.0811012
## 1958 1.0043450 0.9080909
## 1959 1.0996004 0.9926334
## 1960 1.0880687 0.9809851
## 1961 0.8182239 0.7369940
## 1962 0.9178033 0.8261355
## 1963 0.8918333 0.8024440
## 1964 1.2165362 1.0944712
## 1965 1.0000749 0.8998738
## 1966 0.5714733 0.5144450
## 1967 0.9207096 0.8294480
## 1968 1.2771627 1.1518058
## 1969 1.3608663 1.2290666
## 1970 1.3262333 1.2000033
## 1971 0.9697822 0.8794799
## 1972 0.9683401 0.8805693
## 1973 1.1371872 1.0374236
## 1974 1.1804819 1.0809085
## 1975 1.1487336 1.0562910
## 1976 1.2714004 1.1746864
## 1977 1.2252206 1.1380993
## 1978 0.7648612 0.7147157
## 1979 1.2829467 1.2067281
## 1980 1.2540842 1.1881000
## 1981 1.1299748 1.0789571
## 1982 0.8240302 0.7935541
## 1983 0.8254735 0.8022837
## 1984 1.1458496 1.1247251
## 1985 1.5730179 1.5604918
## 1986 1.6495042 1.6550697
## 1987 1.2526419 1.2721887
## 1988 0.7951679 0.8180135
## 1989 0.7475444 0.7794958
## 1990 0.7489876 0.7921530
## 1991 0.8399051 0.9015578
## 1992 0.9337090 1.0178112
## 1993 1.2223362 1.3539306
## 1994 1.0087521 1.1360321
## 1995 0.9005169 1.0316570
## 1996 0.7331131 0.8548124
## 1997 1.1458500 1.3604525
## 1998 0.8702111 1.0524826
## 1999 0.7792935 0.9604573
## 2000 0.7172386 0.9010482
## 2001 0.7186818 0.9204834
## 2002 0.6566269 0.8575249
## 2003 0.4127369 0.5496284
## 2004 0.2741959 0.3723149
## 2005 0.2135841 0.2956866
## 2006 0.4618036 0.6517447
## 2007 0.5036545 0.7245251
## 2008 0.8557797 1.2546808
## 2009 0.6407524 0.9573584
## 2010 0.6263211 0.9536034
## 2011 0.4271683 0.6627373
## 2012 0.5512780 0.8715351
## 2013 0.5339604 0.8602345
## 2014 0.6277642 1.0307348
## 2015 0.6725014 1.1255551
## 2016 0.9034032 1.5416747
## 2017 1.0708070 1.8638312
## 2018        NA        NA
## 
## 
## $curves
## $curves[[1]]
##     ModNegExp    Spline
## 1          NA        NA
## 2          NA        NA
## 3          NA        NA
## 4          NA        NA
## 5          NA        NA
## 6          NA        NA
## 7          NA        NA
## 8          NA        NA
## 9          NA        NA
## 10         NA        NA
## 11         NA        NA
## 12         NA        NA
## 13         NA        NA
## 14         NA        NA
## 15         NA        NA
## 16         NA        NA
## 17         NA        NA
## 18         NA        NA
## 19         NA        NA
## 20         NA        NA
## 21         NA        NA
## 22         NA        NA
## 23         NA        NA
## 24         NA        NA
## 25         NA        NA
## 26         NA        NA
## 27         NA        NA
## 28         NA        NA
## 29         NA        NA
## 30         NA        NA
## 31         NA        NA
## 32         NA        NA
## 33         NA        NA
## 34         NA        NA
## 35         NA        NA
## 36         NA        NA
## 37         NA        NA
## 38         NA        NA
## 39         NA        NA
## 40         NA        NA
## 41         NA        NA
## 42         NA        NA
## 43         NA        NA
## 44         NA        NA
## 45         NA        NA
## 46         NA        NA
## 47         NA        NA
## 48         NA        NA
## 49   1.260764 0.9431473
## 50   1.260764 0.9703562
## 51   1.260764 0.9975125
## 52   1.260764 1.0245457
## 53   1.260764 1.0513561
## 54   1.260764 1.0778176
## 55   1.260764 1.1038066
## 56   1.260764 1.1291830
## 57   1.260764 1.1537937
## 58   1.260764 1.1775093
## 59   1.260764 1.2002111
## 60   1.260764 1.2217823
## 61   1.260764 1.2421851
## 62   1.260764 1.2613545
## 63   1.260764 1.2791638
## 64   1.260764 1.2954568
## 65   1.260764 1.3100977
## 66   1.260764 1.3229243
## 67   1.260764 1.3337509
## 68   1.260764 1.3424533
## 69   1.260764 1.3489468
## 70   1.260764 1.3531958
## 71   1.260764 1.3551476
## 72   1.260764 1.3547291
## 73   1.260764 1.3518917
## 74   1.260764 1.3466552
## 75   1.260764 1.3391129
## 76   1.260764 1.3293752
## 77   1.260764 1.3175783
## 78   1.260764 1.3039181
## 79   1.260764 1.2887406
## 80   1.260764 1.2725472
## 81   1.260764 1.2559508
## 82   1.260764 1.2395733
## 83   1.260764 1.2239420
## 84   1.260764 1.2094753
## 85   1.260764 1.1964669
## 86   1.260764 1.1850959
## 87   1.260764 1.1754279
## 88   1.260764 1.1674059
## 89   1.260764 1.1609277
## 90   1.260764 1.1559102
## 91   1.260764 1.1523178
## 92   1.260764 1.1501557
## 93   1.260764 1.1494117
## 94   1.260764 1.1500149
## 95   1.260764 1.1517977
## 96   1.260764 1.1544956
## 97   1.260764 1.1578451
## 98   1.260764 1.1616448
## 99   1.260764 1.1657962
## 100  1.260764 1.1703096
## 101  1.260764 1.1753325
## 102  1.260764 1.1811494
## 103  1.260764 1.1882313
## 104  1.260764 1.1971483
## 105  1.260764 1.2085276
## 106  1.260764 1.2229495
## 107  1.260764 1.2408934
## 108  1.260764 1.2627499
## 109  1.260764 1.2887444
## 110  1.260764 1.3189105
## 111  1.260764 1.3531314
## 112  1.260764 1.3911208
## 113  1.260764 1.4325341
## 114  1.260764 1.4770223
## 115  1.260764 1.5241342
## 116  1.260764 1.5732814
## 117  1.260764 1.6238235
## 118  1.260764 1.6751757
## 119  1.260764 1.7269711
## 120  1.260764 1.7789572
## 121        NA        NA
## 
## $curves[[2]]
##     ModNegExp   Spline
## 1    2.107642 1.167383
## 2    2.107642 1.199824
## 3    2.107642 1.232235
## 4    2.107642 1.264570
## 5    2.107642 1.296765
## 6    2.107642 1.328746
## 7    2.107642 1.360443
## 8    2.107642 1.391783
## 9    2.107642 1.422705
## 10   2.107642 1.453179
## 11   2.107642 1.483195
## 12   2.107642 1.512756
## 13   2.107642 1.541860
## 14   2.107642 1.570510
## 15   2.107642 1.598724
## 16   2.107642 1.626522
## 17   2.107642 1.653936
## 18   2.107642 1.680998
## 19   2.107642 1.707716
## 20   2.107642 1.734075
## 21   2.107642 1.760037
## 22   2.107642 1.785572
## 23   2.107642 1.810629
## 24   2.107642 1.835129
## 25   2.107642 1.858995
## 26   2.107642 1.882153
## 27   2.107642 1.904508
## 28   2.107642 1.925945
## 29   2.107642 1.946364
## 30   2.107642 1.965678
## 31   2.107642 1.983814
## 32   2.107642 2.000730
## 33   2.107642 2.016414
## 34   2.107642 2.030875
## 35   2.107642 2.044127
## 36   2.107642 2.056188
## 37   2.107642 2.067109
## 38   2.107642 2.076955
## 39   2.107642 2.085794
## 40   2.107642 2.093736
## 41   2.107642 2.100893
## 42   2.107642 2.107367
## 43   2.107642 2.113242
## 44   2.107642 2.118583
## 45   2.107642 2.123446
## 46   2.107642 2.127892
## 47   2.107642 2.131988
## 48   2.107642 2.135818
## 49   2.107642 2.139496
## 50   2.107642 2.143167
## 51   2.107642 2.146948
## 52   2.107642 2.150923
## 53   2.107642 2.155126
## 54   2.107642 2.159555
## 55   2.107642 2.164202
## 56   2.107642 2.169052
## 57   2.107642 2.174108
## 58   2.107642 2.179386
## 59   2.107642 2.184881
## 60   2.107642 2.190572
## 61   2.107642 2.196410
## 62   2.107642 2.202324
## 63   2.107642 2.208222
## 64   2.107642 2.213999
## 65   2.107642 2.219567
## 66   2.107642 2.224860
## 67   2.107642 2.229851
## 68   2.107642 2.234547
## 69   2.107642 2.238954
## 70   2.107642 2.243049
## 71   2.107642 2.246765
## 72   2.107642 2.249996
## 73   2.107642 2.252604
## 74   2.107642 2.254443
## 75   2.107642 2.255354
## 76   2.107642 2.255170
## 77   2.107642 2.253713
## 78   2.107642 2.250822
## 79   2.107642 2.246392
## 80   2.107642 2.240395
## 81   2.107642 2.232883
## 82   2.107642 2.223989
## 83   2.107642 2.213896
## 84   2.107642 2.202844
## 85   2.107642 2.191121
## 86   2.107642 2.179043
## 87   2.107642 2.166958
## 88   2.107642 2.155202
## 89   2.107642 2.144115
## 90   2.107642 2.134059
## 91   2.107642 2.125421
## 92   2.107642 2.118603
## 93   2.107642 2.113999
## 94   2.107642 2.111996
## 95   2.107642 2.112945
## 96   2.107642 2.117156
## 97   2.107642 2.124897
## 98   2.107642 2.136398
## 99   2.107642 2.151856
## 100  2.107642 2.171448
## 101  2.107642 2.195316
## 102  2.107642 2.223567
## 103  2.107642 2.256284
## 104  2.107642 2.293502
## 105  2.107642 2.335212
## 106  2.107642 2.381354
## 107  2.107642 2.431830
## 108  2.107642 2.486496
## 109  2.107642 2.545136
## 110  2.107642 2.607461
## 111  2.107642 2.673128
## 112  2.107642 2.741746
## 113  2.107642 2.812907
## 114  2.107642 2.886201
## 115  2.107642 2.961210
## 116  2.107642 3.037516
## 117  2.107642 3.114731
## 118  2.107642 3.192510
## 119  2.107642 3.270573
## 120  2.107642 3.348734
## 121        NA       NA
## 
## $curves[[3]]
##     ModNegExp   Spline
## 1    4.971058 3.087253
## 2    4.374321 3.000311
## 3    3.874198 2.913439
## 4    3.455048 2.826731
## 5    3.103760 2.740328
## 6    2.809347 2.654396
## 7    2.562601 2.569108
## 8    2.355805 2.484664
## 9    2.182489 2.401331
## 10   2.037234 2.319393
## 11   1.915497 2.239115
## 12   1.813470 2.160725
## 13   1.727961 2.084420
## 14   1.656296 2.010379
## 15   1.596234 1.938768
## 16   1.545897 1.869730
## 17   1.503709 1.803400
## 18   1.468352 1.739909
## 19   1.438719 1.679363
## 20   1.413884 1.621840
## 21   1.393070 1.567392
## 22   1.375626 1.516041
## 23   1.361006 1.467786
## 24   1.348753 1.422589
## 25   1.338484 1.380386
## 26   1.329878 1.341087
## 27   1.322665 1.304602
## 28   1.316619 1.270829
## 29   1.311553 1.239664
## 30   1.307307 1.210991
## 31   1.303748 1.184687
## 32   1.300765 1.160645
## 33   1.298266 1.138769
## 34   1.296171 1.118993
## 35   1.294415 1.101272
## 36   1.292944 1.085569
## 37   1.291710 1.071855
## 38   1.290677 1.060104
## 39   1.289810 1.050283
## 40   1.289084 1.042353
## 41   1.288476 1.036272
## 42   1.287966 1.031993
## 43   1.287539 1.029472
## 44   1.287181 1.028666
## 45   1.286880 1.029523
## 46   1.286629 1.031986
## 47   1.286418 1.035979
## 48   1.286241 1.041418
## 49   1.286093 1.048205
## 50   1.285969 1.056232
## 51   1.285865 1.065383
## 52   1.285778 1.075531
## 53   1.285705 1.086551
## 54   1.285643 1.098311
## 55   1.285592 1.110679
## 56   1.285549 1.123524
## 57   1.285513 1.136719
## 58   1.285483 1.150159
## 59   1.285457 1.163755
## 60   1.285436 1.177435
## 61   1.285418 1.191129
## 62   1.285404 1.204769
## 63   1.285391 1.218281
## 64   1.285381 1.231596
## 65   1.285372 1.244651
## 66   1.285364 1.257385
## 67   1.285358 1.269735
## 68   1.285353 1.281644
## 69   1.285349 1.293063
## 70   1.285345 1.303956
## 71   1.285342 1.314294
## 72   1.285340 1.324042
## 73   1.285337 1.333164
## 74   1.285336 1.341626
## 75   1.285334 1.349408
## 76   1.285333 1.356510
## 77   1.285332 1.362940
## 78   1.285331 1.368727
## 79   1.285330 1.373921
## 80   1.285330 1.378598
## 81   1.285329 1.382836
## 82   1.285329 1.386703
## 83   1.285328 1.390247
## 84   1.285328 1.393506
## 85   1.285328 1.396506
## 86   1.285328 1.399262
## 87   1.285327 1.401775
## 88   1.285327 1.404035
## 89   1.285327 1.406039
## 90   1.285327 1.407803
## 91   1.285327 1.409369
## 92   1.285327 1.410797
## 93   1.285327 1.412155
## 94   1.285327 1.413508
## 95   1.285327 1.414920
## 96   1.285327 1.416454
## 97   1.285327 1.418166
## 98   1.285327 1.420124
## 99   1.285326 1.422421
## 100  1.285326 1.425158
## 101  1.285326 1.428442
## 102  1.285326 1.432381
## 103  1.285326 1.437092
## 104  1.285326 1.442689
## 105  1.285326 1.449277
## 106  1.285326 1.456942
## 107  1.285326 1.465749
## 108  1.285326 1.475735
## 109  1.285326 1.486904
## 110  1.285326 1.499224
## 111  1.285326 1.512632
## 112  1.285326 1.527044
## 113  1.285326 1.542366
## 114  1.285326 1.558502
## 115  1.285326 1.575340
## 116  1.285326 1.592747
## 117  1.285326 1.610574
## 118  1.285326 1.628675
## 119  1.285326 1.646930
## 120  1.285326 1.665246
## 121        NA       NA
## 
## $curves[[4]]
##     ModNegExp    Spline
## 1   2.7057990 1.8594673
## 2   2.4667556 1.8141068
## 3   2.2573814 1.7687401
## 4   2.0739940 1.7233927
## 5   1.9133681 1.6781341
## 6   1.7726785 1.6330287
## 7   1.6494508 1.5881506
## 8   1.5415177 1.5436107
## 9   1.4469809 1.4995490
## 10  1.3641777 1.4561206
## 11  1.2916517 1.4134948
## 12  1.2281274 1.3718476
## 13  1.1724875 1.3313400
## 14  1.1237535 1.2921058
## 15  1.0810682 1.2542501
## 16  1.0436808 1.2178497
## 17  1.0109338 1.1829644
## 18  0.9822512 1.1496420
## 19  0.9571287 1.1179175
## 20  0.9351242 1.0878195
## 21  0.9158509 1.0593664
## 22  0.8989697 1.0325734
## 23  0.8841838 1.0074512
## 24  0.8712330 0.9839967
## 25  0.8598897 0.9621925
## 26  0.8499542 0.9420101
## 27  0.8412519 0.9234073
## 28  0.8336297 0.9063287
## 29  0.8269535 0.8907153
## 30  0.8211060 0.8765101
## 31  0.8159842 0.8636526
## 32  0.8114981 0.8520846
## 33  0.8075689 0.8417494
## 34  0.8041273 0.8325923
## 35  0.8011128 0.8245572
## 36  0.7984726 0.8175807
## 37  0.7961600 0.8115979
## 38  0.7941344 0.8065385
## 39  0.7923603 0.8023255
## 40  0.7908063 0.7988809
## 41  0.7894452 0.7961231
## 42  0.7882531 0.7939658
## 43  0.7872089 0.7923190
## 44  0.7862943 0.7911034
## 45  0.7854933 0.7902456
## 46  0.7847916 0.7896750
## 47  0.7841771 0.7893208
## 48  0.7836388 0.7891119
## 49  0.7831673 0.7889710
## 50  0.7827544 0.7888165
## 51  0.7823927 0.7885665
## 52  0.7820759 0.7881484
## 53  0.7817984 0.7875052
## 54  0.7815553 0.7865963
## 55  0.7813425 0.7853912
## 56  0.7811560 0.7838575
## 57  0.7809927 0.7819647
## 58  0.7808497 0.7796796
## 59  0.7807244 0.7769700
## 60  0.7806146 0.7738185
## 61  0.7805185 0.7702268
## 62  0.7804343 0.7661996
## 63  0.7803606 0.7617400
## 64  0.7802960 0.7568700
## 65  0.7802394 0.7516508
## 66  0.7801899 0.7461557
## 67  0.7801465 0.7404579
## 68  0.7801085 0.7346285
## 69  0.7800752 0.7287379
## 70  0.7800460 0.7228448
## 71  0.7800205 0.7169939
## 72  0.7799981 0.7112225
## 73  0.7799785 0.7055666
## 74  0.7799613 0.7000627
## 75  0.7799463 0.6947423
## 76  0.7799331 0.6896306
## 77  0.7799216 0.6847473
## 78  0.7799115 0.6801123
## 79  0.7799026 0.6757448
## 80  0.7798949 0.6716631
## 81  0.7798881 0.6678858
## 82  0.7798822 0.6644396
## 83  0.7798770 0.6613527
## 84  0.7798724 0.6586610
## 85  0.7798684 0.6564138
## 86  0.7798649 0.6546754
## 87  0.7798618 0.6535194
## 88  0.7798591 0.6530196
## 89  0.7798568 0.6532458
## 90  0.7798547 0.6542675
## 91  0.7798529 0.6561584
## 92  0.7798514 0.6589920
## 93  0.7798500 0.6628341
## 94  0.7798488 0.6677378
## 95  0.7798477 0.6737472
## 96  0.7798468 0.6808934
## 97  0.7798460 0.6892001
## 98  0.7798452 0.6986860
## 99  0.7798446 0.7093627
## 100 0.7798441 0.7212342
## 101 0.7798436 0.7342941
## 102 0.7798432 0.7485233
## 103 0.7798428 0.7638907
## 104 0.7798425 0.7803517
## 105 0.7798422 0.7978469
## 106 0.7798419 0.8163056
## 107 0.7798417 0.8356544
## 108 0.7798415 0.8558137
## 109 0.7798414 0.8766936
## 110 0.7798412 0.8981936
## 111 0.7798411 0.9202133
## 112 0.7798410 0.9426562
## 113 0.7798409 0.9654472
## 114 0.7798408 0.9885418
## 115 0.7798407 1.0119121
## 116 0.7798407 1.0355305
## 117 0.7798406 1.0593660
## 118 0.7798406 1.0833818
## 119 0.7798405 1.1075282
## 120 0.7798405 1.1317467
## 121 0.7798404 1.1559912
## 
## $curves[[5]]
##     ModNegExp   Spline
## 1          NA       NA
## 2          NA       NA
## 3          NA       NA
## 4          NA       NA
## 5          NA       NA
## 6          NA       NA
## 7          NA       NA
## 8          NA       NA
## 9          NA       NA
## 10   8.009790 3.339427
## 11   5.881923 3.218071
## 12   4.423155 3.096971
## 13   3.423091 2.976508
## 14   2.737492 2.857146
## 15   2.267477 2.739383
## 16   1.945257 2.623663
## 17   1.724357 2.510375
## 18   1.572918 2.399872
## 19   1.469099 2.292484
## 20   1.397925 2.188512
## 21   1.349132 2.088250
## 22   1.315681 1.992005
## 23   1.292749 1.900068
## 24   1.277028 1.812699
## 25   1.266250 1.730120
## 26   1.258861 1.652501
## 27   1.253796 1.579956
## 28   1.250323 1.512557
## 29   1.247942 1.450339
## 30   1.246310 1.393302
## 31   1.245191 1.341409
## 32   1.244424 1.294599
## 33   1.243899 1.252794
## 34   1.243538 1.215922
## 35   1.243291 1.183915
## 36   1.243121 1.156691
## 37   1.243005 1.134154
## 38   1.242926 1.116179
## 39   1.242871 1.102605
## 40   1.242834 1.093245
## 41   1.242808 1.087893
## 42   1.242790 1.086324
## 43   1.242778 1.088307
## 44   1.242770 1.093599
## 45   1.242764 1.101944
## 46   1.242761 1.113069
## 47   1.242758 1.126678
## 48   1.242756 1.142458
## 49   1.242755 1.160085
## 50   1.242754 1.179248
## 51   1.242753 1.199639
## 52   1.242753 1.220955
## 53   1.242753 1.242891
## 54   1.242752 1.265122
## 55   1.242752 1.287314
## 56   1.242752 1.309136
## 57   1.242752 1.330287
## 58   1.242752 1.350510
## 59   1.242752 1.369588
## 60   1.242752 1.387344
## 61   1.242752 1.403654
## 62   1.242752 1.418424
## 63   1.242752 1.431569
## 64   1.242752 1.443026
## 65   1.242752 1.452771
## 66   1.242752 1.460820
## 67   1.242752 1.467219
## 68   1.242752 1.472031
## 69   1.242752 1.475331
## 70   1.242752 1.477196
## 71   1.242752 1.477690
## 72   1.242752 1.476881
## 73   1.242752 1.474841
## 74   1.242752 1.471645
## 75   1.242752 1.467368
## 76   1.242752 1.462069
## 77   1.242752 1.455793
## 78   1.242752 1.448583
## 79   1.242752 1.440491
## 80   1.242752 1.431576
## 81   1.242752 1.421899
## 82   1.242752 1.411523
## 83   1.242752 1.400490
## 84   1.242752 1.388835
## 85   1.242752 1.376592
## 86   1.242752 1.363798
## 87   1.242752 1.350492
## 88   1.242752 1.336702
## 89   1.242752 1.322472
## 90   1.242752 1.307871
## 91   1.242752 1.292993
## 92   1.242752 1.277938
## 93   1.242752 1.262794
## 94   1.242752 1.247624
## 95   1.242752 1.232465
## 96   1.242752 1.217336
## 97   1.242752 1.202257
## 98   1.242752 1.187259
## 99   1.242752 1.172387
## 100  1.242752 1.157696
## 101  1.242752 1.143250
## 102  1.242752 1.129131
## 103  1.242752 1.115452
## 104  1.242752 1.102329
## 105  1.242752 1.089885
## 106  1.242752 1.078232
## 107  1.242752 1.067469
## 108  1.242752 1.057669
## 109  1.242752 1.048875
## 110  1.242752 1.041097
## 111  1.242752 1.034316
## 112  1.242752 1.028487
## 113  1.242752 1.023537
## 114  1.242752 1.019374
## 115  1.242752 1.015895
## 116  1.242752 1.012979
## 117  1.242752 1.010499
## 118  1.242752 1.008323
## 119  1.242752 1.006315
## 120  1.242752 1.004368
## 121        NA       NA
## 
## $curves[[6]]
##     ModNegExp   Spline
## 1    5.978157 4.123919
## 2    5.436924 4.002220
## 3    4.957512 3.880531
## 4    4.532859 3.758842
## 5    4.156712 3.637187
## 6    3.823530 3.515666
## 7    3.528405 3.394505
## 8    3.266990 3.274046
## 9    3.035434 3.154695
## 10   2.830328 3.036886
## 11   2.648649 2.921034
## 12   2.487723 2.807506
## 13   2.345178 2.696630
## 14   2.218915 2.588692
## 15   2.107074 2.483946
## 16   2.008008 2.382595
## 17   1.920257 2.284805
## 18   1.842530 2.190723
## 19   1.773681 2.100477
## 20   1.712696 2.014183
## 21   1.658677 1.931936
## 22   1.610828 1.853817
## 23   1.568444 1.779878
## 24   1.530902 1.710135
## 25   1.497648 1.644567
## 26   1.468193 1.583131
## 27   1.442101 1.525771
## 28   1.418991 1.472424
## 29   1.398519 1.423017
## 30   1.380387 1.377462
## 31   1.364325 1.335653
## 32   1.350098 1.297490
## 33   1.337496 1.262884
## 34   1.326333 1.231767
## 35   1.316446 1.204077
## 36   1.307688 1.179746
## 37   1.299930 1.158695
## 38   1.293058 1.140826
## 39   1.286972 1.126016
## 40   1.281580 1.114118
## 41   1.276804 1.104967
## 42   1.272574 1.098389
## 43   1.268827 1.094214
## 44   1.265508 1.092273
## 45   1.262568 1.092401
## 46   1.259964 1.094428
## 47   1.257658 1.098174
## 48   1.255615 1.103453
## 49   1.253805 1.110086
## 50   1.252202 1.117911
## 51   1.250782 1.126782
## 52   1.249524 1.136570
## 53   1.248410 1.147153
## 54   1.247423 1.158398
## 55   1.246549 1.170171
## 56   1.245775 1.182333
## 57   1.245089 1.194770
## 58   1.244481 1.207399
## 59   1.243943 1.220157
## 60   1.243466 1.232990
## 61   1.243044 1.245847
## 62   1.242670 1.258674
## 63   1.242339 1.271403
## 64   1.242046 1.283949
## 65   1.241786 1.296224
## 66   1.241556 1.308129
## 67   1.241352 1.319557
## 68   1.241171 1.330388
## 69   1.241011 1.340502
## 70   1.240869 1.349777
## 71   1.240744 1.358095
## 72   1.240633 1.365346
## 73   1.240534 1.371443
## 74   1.240447 1.376316
## 75   1.240369 1.379922
## 76   1.240301 1.382234
## 77   1.240240 1.383258
## 78   1.240187 1.383035
## 79   1.240139 1.381643
## 80   1.240097 1.379193
## 81   1.240060 1.375812
## 82   1.240027 1.371619
## 83   1.239997 1.366712
## 84   1.239971 1.361179
## 85   1.239948 1.355100
## 86   1.239928 1.348550
## 87   1.239910 1.341598
## 88   1.239894 1.334311
## 89   1.239880 1.326759
## 90   1.239867 1.319031
## 91   1.239856 1.311244
## 92   1.239846 1.303524
## 93   1.239838 1.295989
## 94   1.239830 1.288741
## 95   1.239823 1.281862
## 96   1.239817 1.275417
## 97   1.239812 1.269463
## 98   1.239807 1.264064
## 99   1.239803 1.259294
## 100  1.239799 1.255231
## 101  1.239796 1.251954
## 102  1.239793 1.249542
## 103  1.239790 1.248074
## 104  1.239788 1.247630
## 105  1.239786 1.248285
## 106  1.239784 1.250103
## 107  1.239783 1.253129
## 108  1.239781 1.257384
## 109  1.239780 1.262859
## 110  1.239779 1.269519
## 111  1.239778 1.277314
## 112  1.239777 1.286171
## 113  1.239776 1.295997
## 114  1.239775 1.306684
## 115  1.239775 1.318104
## 116  1.239774 1.330117
## 117  1.239774 1.342570
## 118  1.239773 1.355306
## 119  1.239773 1.368185
## 120  1.239773 1.381116
## 121        NA       NA
## 
## $curves[[7]]
##     ModNegExp    Spline
## 1   7.0615621 5.7349908
## 2   6.6823479 5.5858003
## 3   6.3250779 5.4366363
## 4   5.9884822 5.2875815
## 5   5.6713645 5.1387532
## 6   5.3725976 4.9902722
## 7   5.0911195 4.8422979
## 8   4.8259299 4.6950098
## 9   4.5760861 4.5486119
## 10  4.3407001 4.4033223
## 11  4.1189354 4.2593500
## 12  3.9100036 4.1168779
## 13  3.7131621 3.9760726
## 14  3.5277114 3.8370862
## 15  3.3529922 3.7000638
## 16  3.1883836 3.5651293
## 17  3.0333004 3.4324101
## 18  2.8871916 3.3020496
## 19  2.7495377 3.1741830
## 20  2.6198494 3.0489402
## 21  2.4976659 2.9264428
## 22  2.3825529 2.8067898
## 23  2.2741011 2.6900510
## 24  2.1719252 2.5762723
## 25  2.0756619 2.4654731
## 26  1.9849692 2.3576467
## 27  1.8995246 2.2527788
## 28  1.8190244 2.1508544
## 29  1.7431827 2.0518609
## 30  1.6717296 1.9557861
## 31  1.6044114 1.8626201
## 32  1.5409887 1.7723549
## 33  1.4812362 1.6849878
## 34  1.4249413 1.6005406
## 35  1.3719041 1.5190636
## 36  1.3219360 1.4406140
## 37  1.2748595 1.3652509
## 38  1.2305071 1.2930124
## 39  1.1887214 1.2239148
## 40  1.1493536 1.1579630
## 41  1.1122640 1.0951502
## 42  1.0773206 1.0354642
## 43  1.0443993 0.9788880
## 44  1.0133831 0.9254005
## 45  0.9841618 0.8749721
## 46  0.9566313 0.8275652
## 47  0.9306941 0.7831422
## 48  0.9062577 0.7416592
## 49  0.8832354 0.7030603
## 50  0.8615453 0.6672772
## 51  0.8411104 0.6342299
## 52  0.8218580 0.6038355
## 53  0.8037197 0.5760062
## 54  0.7866311 0.5506463
## 55  0.7705312 0.5276539
## 56  0.7553631 0.5069242
## 57  0.7410727 0.4883532
## 58  0.7276092 0.4718314
## 59  0.7149249 0.4572449
## 60  0.7029745 0.4444844
## 61  0.6917157 0.4334505
## 62  0.6811084 0.4240484
## 63  0.6711149 0.4161839
## 64  0.6616997 0.4097662
## 65  0.6528294 0.4047089
## 66  0.6444724 0.4009299
## 67  0.6365989 0.3983500
## 68  0.6291811 0.3968955
## 69  0.6221925 0.3964971
## 70  0.6156083 0.3970883
## 71  0.6094052 0.3986033
## 72  0.6035610 0.4009790
## 73  0.5980550 0.4041529
## 74  0.5928676 0.4080623
## 75  0.5879804 0.4126454
## 76  0.5833760 0.4178383
## 77  0.5790380 0.4235754
## 78  0.5749511 0.4297916
## 79  0.5711007 0.4364216
## 80  0.5674731 0.4434048
## 81  0.5640554 0.4506943
## 82  0.5608355 0.4582568
## 83  0.5578019 0.4660729
## 84  0.5549438 0.4741360
## 85  0.5522512 0.4824451
## 86  0.5497143 0.4910036
## 87  0.5473243 0.4998165
## 88  0.5450726 0.5088836
## 89  0.5429511 0.5181989
## 90  0.5409525 0.5277551
## 91  0.5390695 0.5375463
## 92  0.5372954 0.5475646
## 93  0.5356240 0.5577952
## 94  0.5340494 0.5682141
## 95  0.5325658 0.5787879
## 96  0.5311681 0.5894767
## 97  0.5298513 0.6002411
## 98  0.5286107 0.6110494
## 99  0.5274419 0.6218836
## 100 0.5263407 0.6327425
## 101 0.5253032 0.6436355
## 102 0.5243258 0.6545766
## 103 0.5234049 0.6655856
## 104 0.5225374 0.6766836
## 105 0.5217200 0.6878881
## 106 0.5209499 0.6992132
## 107 0.5202244 0.7106713
## 108 0.5195409 0.7222716
## 109 0.5188969 0.7340109
## 110 0.5182902 0.7458727
## 111 0.5177186 0.7578364
## 112 0.5171801 0.7698793
## 113 0.5166727 0.7819822
## 114 0.5161947 0.7941324
## 115 0.5157444 0.8063184
## 116 0.5153201 0.8185290
## 117 0.5149203 0.8307546
## 118 0.5145437 0.8429896
## 119 0.5141889 0.8552305
## 120 0.5138547 0.8674749
## 121        NA        NA
## 
## $curves[[8]]
##     ModNegExp    Spline
## 1   6.8521490 5.6168549
## 2   6.5124337 5.4818949
## 3   6.1913501 5.3470147
## 4   5.8878766 5.2123170
## 5   5.6010471 5.0779209
## 6   5.3299489 4.9439805
## 7   5.0737192 4.8107328
## 8   4.8315425 4.6784445
## 9   4.6026481 4.5473811
## 10  4.3863075 4.4178061
## 11  4.1818321 4.2899729
## 12  3.9885713 4.1640873
## 13  3.8059099 4.0402852
## 14  3.6332667 3.9186388
## 15  3.4700922 3.7991788
## 16  3.3158670 3.6818962
## 17  3.1701003 3.5667604
## 18  3.0323283 3.4537282
## 19  2.9021124 3.3427412
## 20  2.7790383 3.2337411
## 21  2.6627142 3.1266617
## 22  2.5527700 3.0214320
## 23  2.4488557 2.9179787
## 24  2.3506406 2.8162231
## 25  2.2578121 2.7160823
## 26  2.1700749 2.6174859
## 27  2.0871496 2.5203924
## 28  2.0087724 2.4247758
## 29  1.9346939 2.3306281
## 30  1.8646782 2.2379578
## 31  1.7985025 2.1468011
## 32  1.7359563 2.0572273
## 33  1.6768404 1.9693335
## 34  1.6209668 1.8832438
## 35  1.5681575 1.7990938
## 36  1.5182446 1.7170058
## 37  1.4710693 1.6370839
## 38  1.4264812 1.5594147
## 39  1.3843386 1.4840650
## 40  1.3445074 1.4110987
## 41  1.3068606 1.3405778
## 42  1.2712787 1.2725675
## 43  1.2376482 1.2071339
## 44  1.2058622 1.1443259
## 45  1.1758196 1.0841838
## 46  1.1474246 1.0267453
## 47  1.1205870 0.9720311
## 48  1.0952213 0.9200458
## 49  1.0712468 0.8707795
## 50  1.0485871 0.8242117
## 51  1.0271703 0.7803105
## 52  1.0069280 0.7390388
## 53  0.9877960 0.7003516
## 54  0.9697132 0.6641966
## 55  0.9526223 0.6305165
## 56  0.9364686 0.5992497
## 57  0.9212010 0.5703329
## 58  0.9067706 0.5436999
## 59  0.8931318 0.5192822
## 60  0.8802409 0.4970105
## 61  0.8680571 0.4768199
## 62  0.8565415 0.4586515
## 63  0.8456574 0.4424514
## 64  0.8353703 0.4281735
## 65  0.8256474 0.4157828
## 66  0.8164578 0.4052485
## 67  0.8077721 0.3965408
## 68  0.7995629 0.3896323
## 69  0.7918038 0.3844971
## 70  0.7844704 0.3811077
## 71  0.7775391 0.3794331
## 72  0.7709880 0.3794382
## 73  0.7647961 0.3810836
## 74  0.7589439 0.3843265
## 75  0.7534126 0.3891193
## 76  0.7481847 0.3954096
## 77  0.7432436 0.4031420
## 78  0.7385734 0.4122588
## 79  0.7341593 0.4227007
## 80  0.7299874 0.4344071
## 81  0.7260443 0.4473157
## 82  0.7223174 0.4613608
## 83  0.7187949 0.4764708
## 84  0.7154656 0.4925690
## 85  0.7123190 0.5095768
## 86  0.7093449 0.5274141
## 87  0.7065339 0.5459976
## 88  0.7038770 0.5652385
## 89  0.7013659 0.5850462
## 90  0.6989926 0.6053434
## 91  0.6967493 0.6260873
## 92  0.6946292 0.6472547
## 93  0.6926253 0.6688286
## 94  0.6907313 0.6907986
## 95  0.6889412 0.7131557
## 96  0.6872492 0.7358893
## 97  0.6856501 0.7589878
## 98  0.6841386 0.7824414
## 99  0.6827101 0.8062516
## 100 0.6813599 0.8304301
## 101 0.6800837 0.8549962
## 102 0.6788776 0.8799702
## 103 0.6777376 0.9053673
## 104 0.6766601 0.9311949
## 105 0.6756417 0.9574519
## 106 0.6746792 0.9841274
## 107 0.6737695 1.0111979
## 108 0.6729096 1.0386244
## 109 0.6720969 1.0663515
## 110 0.6713288 1.0943110
## 111 0.6706028 1.1224343
## 112 0.6699167 1.1506510
## 113 0.6692681 1.1789000
## 114 0.6686552 1.2071392
## 115 0.6680758 1.2353344
## 116 0.6675283 1.2634613
## 117 0.6670107 1.2915137
## 118 0.6665216 1.3195026
## 119 0.6660592 1.3474508
## 120 0.6656223 1.3753834
## 121        NA        NA
## 
## $curves[[9]]
##     ModNegExp    Spline
## 1   7.2961756 5.8002485
## 2   6.8761718 5.6523157
## 3   6.4836990 5.5044373
## 4   6.1169524 5.3566860
## 5   5.7742458 5.2091485
## 6   5.4540033 5.0619441
## 7   5.1547525 4.9152516
## 8   4.8751173 4.7692851
## 9   4.6138120 4.6242668
## 10  4.3696351 4.4804392
## 11  4.1414638 4.3380686
## 12  3.9282489 4.1974143
## 13  3.7290101 4.0587290
## 14  3.5428313 3.9222421
## 15  3.3688563 3.7881623
## 16  3.2062852 3.6566612
## 17  3.0543706 3.5278775
## 18  2.9124138 3.4019264
## 19  2.7797622 3.2788905
## 20  2.6558059 3.1588262
## 21  2.5399747 3.0417587
## 22  2.4317362 2.9276889
## 23  2.3305927 2.8165870
## 24  2.2360790 2.7083836
## 25  2.1477606 2.6029853
## 26  2.0652314 2.5003028
## 27  1.9881120 2.4002843
## 28  1.9160477 2.3028923
## 29  1.8487071 2.2080914
## 30  1.7857807 2.1158479
## 31  1.7269790 2.0261327
## 32  1.6720317 1.9389227
## 33  1.6206862 1.8542024
## 34  1.5727064 1.7719865
## 35  1.5278716 1.6923163
## 36  1.4859756 1.6152424
## 37  1.4468260 1.5408189
## 38  1.4102425 1.4690910
## 39  1.3760571 1.4000851
## 40  1.3441125 1.3338074
## 41  1.3142618 1.2702452
## 42  1.2863679 1.2093749
## 43  1.2603023 1.1511682
## 44  1.2359454 1.0955987
## 45  1.2131850 1.0426404
## 46  1.1919165 0.9922676
## 47  1.1720422 0.9444626
## 48  1.1534706 0.8992064
## 49  1.1361164 0.8564704
## 50  1.1198997 0.8162147
## 51  1.1047460 0.7783885
## 52  1.0905856 0.7429376
## 53  1.0773535 0.7098173
## 54  1.0649887 0.6789785
## 55  1.0534344 0.6503700
## 56  1.0426374 0.6239468
## 57  1.0325482 0.5996804
## 58  1.0231204 0.5775395
## 59  1.0143105 0.5574890
## 60  1.0060781 0.5394965
## 61  0.9983853 0.5235341
## 62  0.9911968 0.5095749
## 63  0.9844795 0.4975932
## 64  0.9782025 0.4875652
## 65  0.9723370 0.4794672
## 66  0.9668559 0.4732759
## 67  0.9617341 0.4689682
## 68  0.9569481 0.4665217
## 69  0.9524758 0.4659136
## 70  0.9482966 0.4671176
## 71  0.9443913 0.4700997
## 72  0.9407421 0.4748202
## 73  0.9373321 0.4812345
## 74  0.9341455 0.4892937
## 75  0.9311679 0.4989453
## 76  0.9283854 0.5101309
## 77  0.9257854 0.5227862
## 78  0.9233557 0.5368446
## 79  0.9210854 0.5522402
## 80  0.9189638 0.5689095
## 81  0.9169813 0.5867959
## 82  0.9151288 0.6058492
## 83  0.9133977 0.6260216
## 84  0.9117800 0.6472676
## 85  0.9102684 0.6695443
## 86  0.9088559 0.6928142
## 87  0.9075360 0.7170426
## 88  0.9063026 0.7421924
## 89  0.9051500 0.7682245
## 90  0.9040730 0.7951103
## 91  0.9030666 0.8228372
## 92  0.9021262 0.8514046
## 93  0.9012474 0.8808127
## 94  0.9004262 0.9110530
## 95  0.8996588 0.9421041
## 96  0.8989418 0.9739380
## 97  0.8982717 1.0065238
## 98  0.8976456 1.0398335
## 99  0.8970605 1.0738423
## 100 0.8965137 1.1085266
## 101 0.8960028 1.1438613
## 102 0.8955254 1.1798183
## 103 0.8950793 1.2163682
## 104 0.8946624 1.2534880
## 105 0.8942728 1.2911658
## 106 0.8939088 1.3293928
## 107 0.8935687 1.3681631
## 108 0.8932508 1.4074694
## 109 0.8929538 1.4472815
## 110 0.8926762 1.4875404
## 111 0.8924169 1.5281678
## 112 0.8921745 1.5690710
## 113 0.8919480 1.6101556
## 114 0.8917364 1.6513378
## 115 0.8915387 1.6925437
## 116 0.8913539 1.7337135
## 117 0.8911812 1.7748199
## 118 0.8910198 1.8158598
## 119 0.8908690 1.8568494
## 120 0.8907281 1.8978154
## 121        NA        NA
## 
## $curves[[10]]
##     ModNegExp    Spline
## 1   3.5463143 2.3363633
## 2   3.1885035 2.2715554
## 3   2.8803698 2.2067987
## 4   2.6150162 2.1421609
## 5   2.3865033 2.0777144
## 6   2.1897163 2.0135289
## 7   2.0202505 1.9496963
## 8   1.8743126 1.8863468
## 9   1.7486362 1.8236290
## 10  1.6404083 1.7617053
## 11  1.5472063 1.7007508
## 12  1.4669441 1.6409441
## 13  1.3978252 1.5824734
## 14  1.3383025 1.5255309
## 15  1.2870437 1.4702994
## 16  1.2429015 1.4169423
## 17  1.2048879 1.3656089
## 18  1.1721519 1.3164401
## 19  1.1439608 1.2695671
## 20  1.1196837 1.2251151
## 21  1.0987771 1.1831992
## 22  1.0807731 1.1439186
## 23  1.0652688 1.1073559
## 24  1.0519170 1.0735732
## 25  1.0404189 1.0426160
## 26  1.0305171 1.0145162
## 27  1.0219901 0.9892961
## 28  1.0146469 0.9669668
## 29  1.0083233 0.9475229
## 30  1.0028776 0.9309366
## 31  0.9981879 0.9171569
## 32  0.9941494 0.9061104
## 33  0.9906715 0.8977050
## 34  0.9876765 0.8918343
## 35  0.9850973 0.8883806
## 36  0.9828762 0.8872129
## 37  0.9809635 0.8881895
## 38  0.9793163 0.8911598
## 39  0.9778978 0.8959603
## 40  0.9766762 0.9024175
## 41  0.9756243 0.9103485
## 42  0.9747184 0.9195667
## 43  0.9739382 0.9298852
## 44  0.9732664 0.9411207
## 45  0.9726879 0.9530903
## 46  0.9721896 0.9656093
## 47  0.9717606 0.9784845
## 48  0.9713911 0.9915223
## 49  0.9710729 1.0045333
## 50  0.9707989 1.0173457
## 51  0.9705629 1.0298032
## 52  0.9703597 1.0417721
## 53  0.9701847 1.0531467
## 54  0.9700340 1.0638288
## 55  0.9699043 1.0737251
## 56  0.9697925 1.0827540
## 57  0.9696962 1.0908582
## 58  0.9696134 1.0980099
## 59  0.9695420 1.1042096
## 60  0.9694805 1.1094808
## 61  0.9694276 1.1138590
## 62  0.9693820 1.1173821
## 63  0.9693428 1.1200880
## 64  0.9693090 1.1220205
## 65  0.9692798 1.1232304
## 66  0.9692548 1.1237662
## 67  0.9692332 1.1236722
## 68  0.9692146 1.1229886
## 69  0.9691986 1.1217499
## 70  0.9691848 1.1199815
## 71  0.9691729 1.1176955
## 72  0.9691627 1.1148979
## 73  0.9691539 1.1116001
## 74  0.9691463 1.1078199
## 75  0.9691398 1.1035776
## 76  0.9691342 1.0988846
## 77  0.9691293 1.0937488
## 78  0.9691251 1.0881808
## 79  0.9691216 1.0821953
## 80  0.9691185 1.0758126
## 81  0.9691158 1.0690612
## 82  0.9691135 1.0619757
## 83  0.9691115 1.0545883
## 84  0.9691098 1.0469309
## 85  0.9691084 1.0390439
## 86  0.9691071 1.0309710
## 87  0.9691060 1.0227517
## 88  0.9691051 1.0144213
## 89  0.9691043 1.0060165
## 90  0.9691036 0.9975829
## 91  0.9691030 0.9891802
## 92  0.9691025 0.9808777
## 93  0.9691020 0.9727444
## 94  0.9691016 0.9648378
## 95  0.9691013 0.9571993
## 96  0.9691010 0.9498594
## 97  0.9691008 0.9428441
## 98  0.9691006 0.9361855
## 99  0.9691004 0.9299190
## 100 0.9691002 0.9240801
## 101 0.9691001 0.9186991
## 102 0.9691000 0.9138057
## 103 0.9690999 0.9094375
## 104 0.9690998 0.9056328
## 105 0.9690997 0.9024278
## 106 0.9690997 0.8998519
## 107 0.9690996 0.8979239
## 108 0.9690996 0.8966501
## 109 0.9690995 0.8960198
## 110 0.9690995 0.8960037
## 111 0.9690995 0.8965623
## 112 0.9690994 0.8976537
## 113 0.9690994 0.8992386
## 114 0.9690994 0.9012710
## 115 0.9690994 0.9036929
## 116 0.9690994 0.9064383
## 117 0.9690994 0.9094423
## 118 0.9690993 0.9126389
## 119 0.9690993 0.9159630
## 120 0.9690993 0.9193556
## 121 0.9690993 0.9227739
## 
## $curves[[11]]
##     ModNegExp    Spline
## 1          NA        NA
## 2          NA        NA
## 3          NA        NA
## 4          NA        NA
## 5          NA        NA
## 6          NA        NA
## 7          NA        NA
## 8          NA        NA
## 9   4.0680263 3.2543070
## 10  3.8145335 3.1627098
## 11  3.5791081 3.0711420
## 12  3.3604624 2.9796699
## 13  3.1574004 2.8883842
## 14  2.9688114 2.7973818
## 15  2.7936638 2.7067551
## 16  2.6309996 2.6165538
## 17  2.4799292 2.5268257
## 18  2.3396261 2.4376042
## 19  2.2093229 2.3489156
## 20  2.0883069 2.2608417
## 21  1.9759162 2.1735290
## 22  1.8715360 2.0871780
## 23  1.7745954 2.0020194
## 24  1.6845641 1.9183041
## 25  1.6009497 1.8362853
## 26  1.5232947 1.7562039
## 27  1.4511746 1.6782783
## 28  1.3841947 1.6026948
## 29  1.3219887 1.5296107
## 30  1.2642164 1.4591479
## 31  1.2105617 1.3913816
## 32  1.1607312 1.3263483
## 33  1.1144524 1.2640640
## 34  1.0714719 1.2045425
## 35  1.0315549 1.1477744
## 36  0.9944829 1.0937324
## 37  0.9600531 1.0424152
## 38  0.9280773 0.9938223
## 39  0.8983806 0.9479531
## 40  0.8708004 0.9048075
## 41  0.8451860 0.8643717
## 42  0.8213972 0.8266146
## 43  0.7993039 0.7914815
## 44  0.7787853 0.7588984
## 45  0.7597292 0.7287828
## 46  0.7420312 0.7010463
## 47  0.7255946 0.6755836
## 48  0.7103296 0.6522726
## 49  0.6961525 0.6309792
## 50  0.6829859 0.6115676
## 51  0.6707577 0.5938997
## 52  0.6594011 0.5778458
## 53  0.6488539 0.5633019
## 54  0.6390584 0.5501698
## 55  0.6299611 0.5383495
## 56  0.6215122 0.5277361
## 57  0.6136655 0.5182252
## 58  0.6063781 0.5097177
## 59  0.5996100 0.5021208
## 60  0.5933244 0.4953519
## 61  0.5874867 0.4893388
## 62  0.5820651 0.4840167
## 63  0.5770299 0.4793260
## 64  0.5723536 0.4752176
## 65  0.5680106 0.4716475
## 66  0.5639772 0.4685712
## 67  0.5602312 0.4659495
## 68  0.5567522 0.4637476
## 69  0.5535212 0.4619385
## 70  0.5505205 0.4605013
## 71  0.5477336 0.4594192
## 72  0.5451454 0.4586900
## 73  0.5427416 0.4583162
## 74  0.5405092 0.4583026
## 75  0.5384358 0.4586582
## 76  0.5365103 0.4593900
## 77  0.5347220 0.4605004
## 78  0.5330611 0.4619855
## 79  0.5315187 0.4638372
## 80  0.5300861 0.4660441
## 81  0.5287557 0.4685958
## 82  0.5275201 0.4714829
## 83  0.5263725 0.4746944
## 84  0.5253068 0.4782213
## 85  0.5243170 0.4820530
## 86  0.5233977 0.4861797
## 87  0.5225440 0.4905957
## 88  0.5217511 0.4952922
## 89  0.5210148 0.5002580
## 90  0.5203309 0.5054811
## 91  0.5196957 0.5109520
## 92  0.5191059 0.5166569
## 93  0.5185580 0.5225774
## 94  0.5180493 0.5286885
## 95  0.5175767 0.5349618
## 96  0.5171379 0.5413673
## 97  0.5167303 0.5478772
## 98  0.5163518 0.5544689
## 99  0.5160003 0.5611227
## 100 0.5156738 0.5678166
## 101 0.5153706 0.5745196
## 102 0.5150890 0.5811898
## 103 0.5148275 0.5877816
## 104 0.5145846 0.5942448
## 105 0.5143590 0.6005366
## 106 0.5141495 0.6066388
## 107 0.5139549 0.6125441
## 108 0.5137742 0.6182503
## 109 0.5136064 0.6237539
## 110 0.5134505 0.6290495
## 111 0.5133058 0.6341394
## 112 0.5131713 0.6390279
## 113 0.5130465 0.6437311
## 114 0.5129305 0.6482714
## 115 0.5128228 0.6526718
## 116 0.5127228 0.6569578
## 117 0.5126299 0.6611596
## 118 0.5125437 0.6653092
## 119 0.5124636 0.6694331
## 120 0.5123892 0.6735484
## 121        NA        NA
## 
## $curves[[12]]
##     ModNegExp    Spline
## 1          NA        NA
## 2          NA        NA
## 3          NA        NA
## 4          NA        NA
## 5          NA        NA
## 6          NA        NA
## 7          NA        NA
## 8   3.4642681 1.9551671
## 9   2.9510964 1.8972084
## 10  2.5329494 1.8393282
## 11  2.1922314 1.7816628
## 12  1.9146046 1.7243797
## 13  1.6883863 1.6676564
## 14  1.5040572 1.6116776
## 15  1.3538606 1.5566296
## 16  1.2314761 1.5026870
## 17  1.1317537 1.4500099
## 18  1.0504971 1.3987446
## 19  0.9842869 1.3490232
## 20  0.9303369 1.3009731
## 21  0.8863769 1.2547125
## 22  0.8505570 1.2103398
## 23  0.8213700 1.1679322
## 24  0.7975876 1.1275439
## 25  0.7782090 1.0892098
## 26  0.7624187 1.0529526
## 27  0.7495524 1.0187910
## 28  0.7390685 0.9867350
## 29  0.7305260 0.9567843
## 30  0.7235653 0.9289216
## 31  0.7178935 0.9031120
## 32  0.7132719 0.8793140
## 33  0.7095062 0.8574850
## 34  0.7064377 0.8375913
## 35  0.7039375 0.8196054
## 36  0.7019002 0.8034989
## 37  0.7002402 0.7892386
## 38  0.6988875 0.7767811
## 39  0.6977853 0.7660649
## 40  0.6968873 0.7570144
## 41  0.6961555 0.7495438
## 42  0.6955592 0.7435657
## 43  0.6950733 0.7389942
## 44  0.6946774 0.7357427
## 45  0.6943548 0.7337179
## 46  0.6940920 0.7328146
## 47  0.6938778 0.7329097
## 48  0.6937033 0.7338681
## 49  0.6935611 0.7355480
## 50  0.6934452 0.7378058
## 51  0.6933508 0.7404998
## 52  0.6932739 0.7434951
## 53  0.6932112 0.7466637
## 54  0.6931601 0.7498801
## 55  0.6931185 0.7530272
## 56  0.6930846 0.7559954
## 57  0.6930569 0.7587032
## 58  0.6930344 0.7611048
## 59  0.6930161 0.7631874
## 60  0.6930011 0.7649608
## 61  0.6929889 0.7664431
## 62  0.6929790 0.7676550
## 63  0.6929709 0.7686151
## 64  0.6929643 0.7693414
## 65  0.6929590 0.7698495
## 66  0.6929546 0.7701472
## 67  0.6929510 0.7702350
## 68  0.6929481 0.7701079
## 69  0.6929458 0.7697615
## 70  0.6929438 0.7691863
## 71  0.6929423 0.7683587
## 72  0.6929410 0.7672489
## 73  0.6929399 0.7658312
## 74  0.6929391 0.7640879
## 75  0.6929384 0.7620070
## 76  0.6929378 0.7595740
## 77  0.6929374 0.7567708
## 78  0.6929370 0.7535802
## 79  0.6929367 0.7499874
## 80  0.6929364 0.7459806
## 81  0.6929362 0.7415536
## 82  0.6929361 0.7367028
## 83  0.6929359 0.7314199
## 84  0.6929358 0.7257008
## 85  0.6929357 0.7195477
## 86  0.6929357 0.7129647
## 87  0.6929356 0.7059503
## 88  0.6929355 0.6984977
## 89  0.6929355 0.6906054
## 90  0.6929355 0.6822887
## 91  0.6929354 0.6735830
## 92  0.6929354 0.6645321
## 93  0.6929354 0.6551765
## 94  0.6929354 0.6455493
## 95  0.6929354 0.6356778
## 96  0.6929354 0.6255860
## 97  0.6929354 0.6152995
## 98  0.6929354 0.6048522
## 99  0.6929354 0.5942824
## 100 0.6929354 0.5836293
## 101 0.6929353 0.5729311
## 102 0.6929353 0.5622322
## 103 0.6929353 0.5515798
## 104 0.6929353 0.5410201
## 105 0.6929353 0.5305969
## 106 0.6929353 0.5203515
## 107 0.6929353 0.5103207
## 108 0.6929353 0.5005300
## 109 0.6929353 0.4909898
## 110 0.6929353 0.4816948
## 111 0.6929353 0.4726302
## 112 0.6929353 0.4637761
## 113 0.6929353 0.4551158
## 114 0.6929353 0.4466324
## 115 0.6929353 0.4383071
## 116 0.6929353 0.4301153
## 117 0.6929353 0.4220290
## 118 0.6929353 0.4140179
## 119 0.6929353 0.4060519
## 120 0.6929353 0.3981047
## 121        NA        NA
## 
## 
## $model.info
## $model.info$P1809Ae
## $model.info$P1809Ae$ModNegExp
## $model.info$P1809Ae$ModNegExp$method
## [1] "Mean"
## 
## $model.info$P1809Ae$ModNegExp$mean
## [1] 1.260764
## 
## $model.info$P1809Ae$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1809Ae$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1809Ae$Spline
## $model.info$P1809Ae$Spline$method
## [1] "Spline"
## 
## $model.info$P1809Ae$Spline$nyrs
## [1] 48
## 
## $model.info$P1809Ae$Spline$f
## [1] 0.5
## 
## $model.info$P1809Ae$Spline$n.zeros
## [1] 0
## 
## $model.info$P1809Ae$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1809Be
## $model.info$P1809Be$ModNegExp
## $model.info$P1809Be$ModNegExp$method
## [1] "Mean"
## 
## $model.info$P1809Be$ModNegExp$mean
## [1] 2.107642
## 
## $model.info$P1809Be$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1809Be$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1809Be$Spline
## $model.info$P1809Be$Spline$method
## [1] "Spline"
## 
## $model.info$P1809Be$Spline$nyrs
## [1] 80
## 
## $model.info$P1809Be$Spline$f
## [1] 0.5
## 
## $model.info$P1809Be$Spline$n.zeros
## [1] 0
## 
## $model.info$P1809Be$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1809Ce
## $model.info$P1809Ce$ModNegExp
## $model.info$P1809Ce$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1809Ce$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1809Ce$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8f5fa94a0>
## 
## $model.info$P1809Ce$ModNegExp$coefs
##     Estimate Std. Error   t value     Pr(>|t|)
## a  4.3977470 0.55100996  7.981248 1.105702e-12
## b -0.1766234 0.03017179 -5.853924 4.479816e-08
## k  1.2853264 0.05507676 23.337001 7.795304e-46
## 
## $model.info$P1809Ce$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1809Ce$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1809Ce$Spline
## $model.info$P1809Ce$Spline$method
## [1] "Spline"
## 
## $model.info$P1809Ce$Spline$nyrs
## [1] 80
## 
## $model.info$P1809Ce$Spline$f
## [1] 0.5
## 
## $model.info$P1809Ce$Spline$n.zeros
## [1] 0
## 
## $model.info$P1809Ce$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1810Ae
## $model.info$P1810Ae$ModNegExp
## $model.info$P1810Ae$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1810Ae$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1810Ae$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8f4442190>
## 
## $model.info$P1810Ae$ModNegExp$coefs
##     Estimate Std. Error   t value     Pr(>|t|)
## a  2.1988757 0.35923190  6.121048 1.252205e-08
## b -0.1325223 0.03088149 -4.291317 3.658355e-05
## k  0.7798402 0.04506284 17.305615 3.622986e-34
## 
## $model.info$P1810Ae$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1810Ae$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1810Ae$Spline
## $model.info$P1810Ae$Spline$method
## [1] "Spline"
## 
## $model.info$P1810Ae$Spline$nyrs
## [1] 81
## 
## $model.info$P1810Ae$Spline$f
## [1] 0.5
## 
## $model.info$P1810Ae$Spline$n.zeros
## [1] 0
## 
## $model.info$P1810Ae$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1810Be
## $model.info$P1810Be$ModNegExp
## $model.info$P1810Be$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1810Be$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1810Be$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8f92e29f8>
## 
## $model.info$P1810Be$ModNegExp$coefs
##     Estimate Std. Error   t value     Pr(>|t|)
## a  9.8709024 0.99905998  9.880190 8.735487e-17
## b -0.3775278 0.04533102 -8.328244 2.811068e-13
## k  1.2427521 0.05302609 23.436613 3.706576e-44
## 
## $model.info$P1810Be$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1810Be$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1810Be$Spline
## $model.info$P1810Be$Spline$method
## [1] "Spline"
## 
## $model.info$P1810Be$Spline$nyrs
## [1] 74
## 
## $model.info$P1810Be$Spline$f
## [1] 0.5
## 
## $model.info$P1810Be$Spline$n.zeros
## [1] 0
## 
## $model.info$P1810Be$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1810Ce
## $model.info$P1810Ce$ModNegExp
## $model.info$P1810Ce$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1810Ce$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1810Ce$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8da10b5f8>
## 
## $model.info$P1810Ce$ModNegExp$coefs
##     Estimate Std. Error   t value     Pr(>|t|)
## a  5.3494137 0.59159152  9.042411 3.896649e-15
## b -0.1212902 0.01943684 -6.240222 7.221669e-09
## k  1.2397702 0.08021652 15.455298 4.901308e-30
## 
## $model.info$P1810Ce$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1810Ce$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1810Ce$Spline
## $model.info$P1810Ce$Spline$method
## [1] "Spline"
## 
## $model.info$P1810Ce$Spline$nyrs
## [1] 80
## 
## $model.info$P1810Ce$Spline$f
## [1] 0.5
## 
## $model.info$P1810Ce$Spline$n.zeros
## [1] 0
## 
## $model.info$P1810Ce$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1811Ae
## $model.info$P1811Ae$ModNegExp
## $model.info$P1811Ae$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1811Ae$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1811Ae$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8db057bb0>
## 
## $model.info$P1811Ae$ModNegExp$coefs
##      Estimate  Std. Error    t value     Pr(>|t|)
## a  6.95565580 0.204400250  34.029585 1.598549e-62
## b -0.05960932 0.003049013 -19.550363 1.155472e-38
## k  0.50841242 0.053720404   9.464047 3.993843e-16
## 
## $model.info$P1811Ae$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1811Ae$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1811Ae$Spline
## $model.info$P1811Ae$Spline$method
## [1] "Spline"
## 
## $model.info$P1811Ae$Spline$nyrs
## [1] 80
## 
## $model.info$P1811Ae$Spline$f
## [1] 0.5
## 
## $model.info$P1811Ae$Spline$n.zeros
## [1] 0
## 
## $model.info$P1811Ae$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1811Be
## $model.info$P1811Be$ModNegExp
## $model.info$P1811Be$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1811Be$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1811Be$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8db1f4f10>
## 
## $model.info$P1811Be$ModNegExp$coefs
##      Estimate  Std. Error    t value     Pr(>|t|)
## a  6.55348540 0.313856521  20.880514 2.865551e-41
## b -0.05640673 0.004814147 -11.716869 1.885031e-21
## k  0.65809189 0.087918268   7.485269 1.456579e-11
## 
## $model.info$P1811Be$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1811Be$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1811Be$Spline
## $model.info$P1811Be$Spline$method
## [1] "Spline"
## 
## $model.info$P1811Be$Spline$nyrs
## [1] 80
## 
## $model.info$P1811Be$Spline$f
## [1] 0.5
## 
## $model.info$P1811Be$Spline$n.zeros
## [1] 0
## 
## $model.info$P1811Be$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1811Ce
## $model.info$P1811Ce$ModNegExp
## $model.info$P1811Ce$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1811Ce$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1811Ce$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8db9b20f8>
## 
## $model.info$P1811Ce$ModNegExp$coefs
##      Estimate  Std. Error   t value     Pr(>|t|)
## a  6.85692205 0.325567657  21.06144 1.288796e-41
## b -0.06779633 0.005342926 -12.68899 9.867547e-24
## k  0.88871951 0.074471494  11.93369 5.815118e-22
## 
## $model.info$P1811Ce$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1811Ce$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1811Ce$Spline
## $model.info$P1811Ce$Spline$method
## [1] "Spline"
## 
## $model.info$P1811Ce$Spline$nyrs
## [1] 80
## 
## $model.info$P1811Ce$Spline$f
## [1] 0.5
## 
## $model.info$P1811Ce$Spline$n.zeros
## [1] 0
## 
## $model.info$P1811Ce$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1812Ae
## $model.info$P1812Ae$ModNegExp
## $model.info$P1812Ae$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1812Ae$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1812Ae$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8dc9c62e8>
## 
## $model.info$P1812Ae$ModNegExp$coefs
##     Estimate Std. Error   t value     Pr(>|t|)
## a  2.9927117 0.36311491  8.241776 2.676100e-13
## b -0.1494706 0.02537143 -5.891294 3.703003e-08
## k  0.9690993 0.04132903 23.448389 3.107354e-46
## 
## $model.info$P1812Ae$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1812Ae$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1812Ae$Spline
## $model.info$P1812Ae$Spline$method
## [1] "Spline"
## 
## $model.info$P1812Ae$Spline$nyrs
## [1] 81
## 
## $model.info$P1812Ae$Spline$f
## [1] 0.5
## 
## $model.info$P1812Ae$Spline$n.zeros
## [1] 0
## 
## $model.info$P1812Ae$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1812Be
## $model.info$P1812Be$ModNegExp
## $model.info$P1812Be$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1812Be$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1812Be$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8f508c3c0>
## 
## $model.info$P1812Be$ModNegExp$coefs
##      Estimate  Std. Error   t value     Pr(>|t|)
## a  3.82955342 0.242276176 15.806562 5.705805e-30
## b -0.07394128 0.007699294 -9.603644 3.432608e-16
## k  0.51141960 0.054135362  9.447052 7.814276e-16
## 
## $model.info$P1812Be$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1812Be$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1812Be$Spline
## $model.info$P1812Be$Spline$method
## [1] "Spline"
## 
## $model.info$P1812Be$Spline$nyrs
## [1] 75
## 
## $model.info$P1812Be$Spline$f
## [1] 0.5
## 
## $model.info$P1812Be$Spline$n.zeros
## [1] 0
## 
## $model.info$P1812Be$Spline$zero.years
## integer(0)
## 
## 
## 
## $model.info$P1812De
## $model.info$P1812De$ModNegExp
## $model.info$P1812De$ModNegExp$method
## [1] "NegativeExponential"
## 
## $model.info$P1812De$ModNegExp$is.constrained
## [1] FALSE
## 
## $model.info$P1812De$ModNegExp$formula
## Y ~ I(a * exp(b * seq_along(Y)) + k)
## <environment: 0x7fe8f4cb3188>
## 
## $model.info$P1812De$ModNegExp$coefs
##     Estimate Std. Error   t value     Pr(>|t|)
## a  3.4011237 0.28064066 12.119141 5.505742e-22
## b -0.2047775 0.02257187 -9.072244 5.206424e-15
## k  0.6929353 0.02575550 26.904365 3.367422e-50
## 
## $model.info$P1812De$ModNegExp$n.zeros
## [1] 0
## 
## $model.info$P1812De$ModNegExp$zero.years
## integer(0)
## 
## 
## $model.info$P1812De$Spline
## $model.info$P1812De$Spline$method
## [1] "Spline"
## 
## $model.info$P1812De$Spline$nyrs
## [1] 75
## 
## $model.info$P1812De$Spline$f
## [1] 0.5
## 
## $model.info$P1812De$Spline$n.zeros
## [1] 0
## 
## $model.info$P1812De$Spline$zero.years
## integer(0)
## 
## 
## 
## 
## $data.info
## $data.info$P1809Ae
## $data.info$P1809Ae$n.zeros
## [1] 0
## 
## $data.info$P1809Ae$zero.years
## integer(0)
## 
## 
## $data.info$P1809Be
## $data.info$P1809Be$n.zeros
## [1] 0
## 
## $data.info$P1809Be$zero.years
## integer(0)
## 
## 
## $data.info$P1809Ce
## $data.info$P1809Ce$n.zeros
## [1] 0
## 
## $data.info$P1809Ce$zero.years
## integer(0)
## 
## 
## $data.info$P1810Ae
## $data.info$P1810Ae$n.zeros
## [1] 0
## 
## $data.info$P1810Ae$zero.years
## integer(0)
## 
## 
## $data.info$P1810Be
## $data.info$P1810Be$n.zeros
## [1] 0
## 
## $data.info$P1810Be$zero.years
## integer(0)
## 
## 
## $data.info$P1810Ce
## $data.info$P1810Ce$n.zeros
## [1] 0
## 
## $data.info$P1810Ce$zero.years
## integer(0)
## 
## 
## $data.info$P1811Ae
## $data.info$P1811Ae$n.zeros
## [1] 0
## 
## $data.info$P1811Ae$zero.years
## integer(0)
## 
## 
## $data.info$P1811Be
## $data.info$P1811Be$n.zeros
## [1] 0
## 
## $data.info$P1811Be$zero.years
## integer(0)
## 
## 
## $data.info$P1811Ce
## $data.info$P1811Ce$n.zeros
## [1] 0
## 
## $data.info$P1811Ce$zero.years
## integer(0)
## 
## 
## $data.info$P1812Ae
## $data.info$P1812Ae$n.zeros
## [1] 0
## 
## $data.info$P1812Ae$zero.years
## integer(0)
## 
## 
## $data.info$P1812Be
## $data.info$P1812Be$n.zeros
## [1] 0
## 
## $data.info$P1812Be$zero.years
## integer(0)
## 
## 
## $data.info$P1812De
## $data.info$P1812De$n.zeros
## [1] 0
## 
## $data.info$P1812De$zero.years
## integer(0)
```



## Apply Detrending Method

```r
pinery.rwi.info <- detrend(rwl = pinery.rwl.trunc, method = c("ModNegExp"), return.info = TRUE)

pinery.rwi <- pinery.rwi.info$series
```




## Descriptive Statistics

### Read IDs


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

### Standard Chronology


```r
rwi.stats(pinery.rwi, pinery.ids, prewhiten=FALSE) #running.window=TRUE, window.length = 50)
```

```
##   n.cores n.trees     n n.tot n.wt n.bt rbar.tot rbar.wt rbar.bt c.eff rbar.eff
## 1      12       4 3.983    66   12   54    0.197   0.414   0.149     3    0.244
##     eps   snr
## 1 0.562 1.284
```


### Residual Chronology


```r
rwi.stats(pinery.rwi, pinery.ids, prewhiten=TRUE) #running.window=TRUE, window.length = 50)
```

```
##   n.cores n.trees    n n.tot n.wt n.bt rbar.tot rbar.wt rbar.bt c.eff rbar.eff
## 1      12       4 3.95    66   12   54    0.325   0.399   0.309     3    0.516
##     eps   snr
## 1 0.808 4.204
```


### Interseries Correlations


```r
interseries.cor(pinery.rwi, biweight=TRUE) %>% round(3)
```

```
##         res.cor p.val
## P1809Ae   0.612     0
## P1809Be   0.477     0
## P1809Ce   0.546     0
## P1810Ae   0.478     0
## P1810Be   0.590     0
## P1810Ce   0.572     0
## P1811Ae   0.477     0
## P1811Be   0.488     0
## P1811Ce   0.440     0
## P1812Ae   0.555     0
## P1812Be   0.362     0
## P1812De   0.528     0
```


# Building a Mean Value Chronology

The simplest way to make a chronology in dplR is chronology is with the crn function which also has a plot method. This defaults to building a mean-value chronology by averaging the rows of the rwi data using Tukey’s biweight robust mean (function tbrm in dplR). 



```r
pinery.crn <- chron(pinery.rwi, prewhiten=TRUE, biweight = TRUE)
head(pinery.crn)
```

```
##         xxxstd    xxxres samp.depth
## 1898 0.8671256        NA          8
## 1899 0.8641290 0.9733467          8
## 1900 0.9841930 0.9200743          8
## 1901 0.8159552 0.8260720          8
## 1902 1.1733469 1.2835061          8
## 1903 1.1951044 1.0563245          8
```

```r
#pinery.crn$samp.depth <- NULL
```

Remove standard and residual chronologies

```r
pinery.STD <- pinery.crn %>% select(-xxxres)
pinery.RES <- pinery.crn %>% select(-xxxstd)
```

## Plot Chronology


```r
crn.plot(pinery.crn, add.spline = TRUE, nyrs=15)
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-30-1.png)<!-- -->


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
lines(yrs, ffcsaps(pinery.avg, nyrs = 15), col = "red", lwd = 1) #adds a spline
axis(1); axis(2); axis(3); axis(4)
box()
```

![](Dendrochronology_in_R_files/figure-html/unnamed-chunk-31-1.png)<!-- -->

# Save Outputs


```r
write.crn(pinery.STD, "outputs/pinery_STD.crn")
```

```
## [1] "outputs/pinery_STD.crn"
```

```r
write.crn(pinery.RES, "outputs/pinery_RES.crn")
```

```
##         xxxres samp.depth
## 1898 9.9900000          0
## 1899 0.9733467          8
## 1900 0.9200743          8
## 1901 0.8260720          8
## 1902 1.2835061          8
## 1903 1.0563245          8
```

```
## [1] "outputs/pinery_RES.crn"
```


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
## [1] "2020-06-05 15:34:03 PDT"
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
## [1] ggplot2_3.3.0 dplyr_0.8.5   dplR_1.7.1   
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.4.6       git2r_0.26.1       compiler_4.0.0     pillar_1.4.3      
##  [5] plyr_1.8.6         iterators_1.0.12   R.methodsS3_1.8.0  R.utils_2.9.2     
##  [9] tools_4.0.0        digest_0.6.25      gtable_0.3.0       evaluate_0.14     
## [13] tibble_3.0.1       lifecycle_0.2.0    lattice_0.20-41    pkgconfig_2.0.3   
## [17] png_0.1-7          rlang_0.4.5        foreach_1.5.0      Matrix_1.2-18     
## [21] yaml_2.2.1         xfun_0.13          withr_2.2.0        stringr_1.4.0     
## [25] knitr_1.28         vctrs_0.2.4        grid_4.0.0         tidyselect_1.0.0  
## [29] glue_1.4.0         R6_2.4.1           XML_3.99-0.3       rmarkdown_2.1     
## [33] purrr_0.3.4        magrittr_1.5       codetools_0.2-16   scales_1.1.0      
## [37] matrixStats_0.56.0 htmltools_0.4.0    ellipsis_0.3.0     MASS_7.3-51.6     
## [41] assertthat_0.2.1   colorspace_1.4-1   stringi_1.4.6      munsell_0.5.0     
## [45] signal_0.7-6       crayon_1.3.4       R.oo_1.23.0
```

