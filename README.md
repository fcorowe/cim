# Compositional Impact of Migration (CIM)

[![DOI](https://zenodo.org/badge/162118396.svg)](https://zenodo.org/badge/latestdoi/162118396) 
[![](http://cranlogs.r-pkg.org/badges/grand-total/CIM?color=green)](https://cran.r-project.org/package=CIM) 
[![](http://cranlogs.r-pkg.org/badges/last-month/CIM?color=green)](https://cran.r-project.org/package=CIM) 
[![](http://cranlogs.r-pkg.org/badges/last-month/CIM?color=green)](https://cran.r-project.org/package=CIM)
[![CRAN checks](https://cranchecks.info/badges/summary/CIM)](https://cran.r-project.org/web/checks/check_results_CIM.html)
[![](https://img.shields.io/github/last-commit/fcorowe/CIM.svg)](https://github.com/fcorowe/CIM/commits/master)
[![](https://img.shields.io/badge/Altmetric-15-green.svg)](https://www.altmetric.com/details/32435474)

[Francisco Rowe](http://www.franciscorowe.com) [[`@fcorowe`](http://twitter.com/fcorowe)]<sup>1*</sup>, Nikos Patias [[`@pat_nikos`](https://twitter.com/pat_nikos)]<sup>1</sup>, Jorge Rodriguez<sup>2</sup>

<sup>1</sup> *Geographic Data Science Lab, University of Liverpool, Liverpool, United Kingdom*

<sup>1</sup> *Latin American Demographic Centre (CELADE), United Nations Economic Commission for Latin America and the Caribbean, Santiago, Chile*

## Overview

This R package produces summary statistical indicators of the impact of migration on the socio-demographic composition of an area. Three measures can be used: ratios, percentages and the Duncan index of dissimilarity. The input data files are assumed to be in an origin-destination matrix format, with each cell representing a flow count between an origin and a destination area. Columns are expected to represent origins, and rows are expected to represent destinations. The first row and column are assumed to contain labels for each area. See [Rodríguez-Vignoli and Rowe (2018)](https://doi.org/10.1080/00324728.2017.1416155) for technical details.


## Getting Started

These instructions will get you the *CMI* package running on your local machine and provide an example on how to interpret the various output indicators.

### Prerequisites

This package has no pre-requisites.

### Installing

To install **{CIM}** from [CRAN](www..com), type:
```{r}
#install.packages("CIM")
```

To install **{CIM}** from [Github](www.github.com), type:
```{r}
#devtools::install_github("fcorowe/cim")
```

Load **{CIM}**
```{r}
#library(CIM)
```

### Use

We present two examples.

#### Example 1: Sex ratio

First, we use the package to quantify the impact of internal migration on the sex ratio of the Greater Santiago in Chile drawing on 2008-2013 transition data from the 2013 [CASEN survey](http://www.hacienda.cl/english/documents/statistics/casen-survey.html). For simplicity, data for this example are aggregated into 3 broad areas.

Read input data
```{r}
#m <- male
#f <- female
```

Display male input data
```{r}
#m
```
**NOTE**: The required input data must be in an origin-destination matrix, with origins as columns. 

Compute and print the CIM outputs
```{r}
#CIM.ratio <- CIM(m, f, calculation = "ratio", numerator = 1, denominator = 2)
#CIM.ratio
```

##### Interpretation

Interpreting the results from the table above:

*Factual Value* (FV) indicates the sex ratio at the end of the time interval (i.e. 2013).

*Counterfactual Value* (CFV) indicates the sex ratio at the start of the time interval (i.e. 2008). Alternatively, it can be interpreted as the counterfactual sex ratio; that is, what would have been the sex ratio if no migration had occurred.

*Compositional Impact of Migration* (CIM) is the difference between the FV and CFV and indicates the change in the area-specific sex ratio because of migration. The results indicate that internal migration contribute to increase the sex ratio of the Greater Santiago by 0.4.

CIM_PC is the CIM divided by the CFV and indicates the percentage change of the CMI i.e. the percentage change in the sex ratio. The results indicate that internal migration contributed to increase the sex ratio of the Greater Santiago by 0.46% between 2008 and 2013.

DIAG corresponds to the diagonal of the origin-destination matrix and indicates the sex ratio of the no-migrant population. The results indicate that the sex ratio relating to those staying in the Greater Santiago was 86.83.

CIM_I represents the change in the CMI due to migration inflows.

CIM_O represents the change in the CMI due to migration outflows.

CIM_I_PC = (CIM_I/CIM)*100

CMI_O_PC = (CIM_O/CIM)*100

**NOTE**: CIM = CIM_I + CIM_O

Taken together, the CMI_I_PC and CMI_O_PC tell us their respective contribution to changes in the CIM i.e. if changes in the CMI were due to migration inflows, migration outflows or both, and the extent of these influences. The results tell us that while migration inflows contributed to increase the sex ratio in the Greater Santiago by 148.83%, migration outflows operated to reduce it by 48.83%. Thus, in absence of migration outflows, migration would have increased the sex ratio by some additional 0.19.

#### Example 2: Duncan index

Next, we measure the impact of internal migration on residential age segregation in the Greater London Metropolitan Area, England, drawing on one-year migration data by age bands (i.e. 1-14, 15-29, 30-34, 45-64 and 65+) at the local authority level, [2011 UK Censuses](https://www.ons.gov.uk/census/2011census/2011ukcensuses/ukcensusesdata). Local authorities comprising outside the Greater London Metropolitan Area are collapsed into a single area, labelled "the Rest of the UK". We use the same approach employed by [Rodríguez-Vignoli and Rowe (2017)](https://www.researchgate.net/publication/321998653_The_Changing_Impacts_of_Internal_Migration_on_Residential_Socio-Economic_Segregation_in_the_Greater_Santiago) to measure the impact of internal migration on residential educational segregation in the Greater Santiago, Chile.

Compute and print the CIM outputs
```{r}
#CIM.duncan <- CIM(pop65over, pop1_14, pop15_29, pop30_44, pop45_64, calculation = "duncan", numerator = 1, DuncanAll= TRUE)
#CIM.duncan$duncan_index
```
The *CIM* for the Duncan index of dissimilarity indicates that internal migration has contributed to increase age segregation of the population aged 65 and over in the Greater London Metropolitan Area by 2.81% between 2010 and 2011 i.e. from 16.2% in 2010 to 19% in 2011.

To visualise where the population aged 65 and over in the Greater London Metropolitan Area is concentrating, we can map differences in the spatial distribution of this population across local authority districts.

First install and load the needed packages
```{r}
#install.packages(c("rgdal", "dplyr", "tmap"))
#library("rgdal")
#library("dplyr")
#library("tmap")
```

[NOTE: Download a shapefile containing the Greater London Local Authority Districts from the shapefile folder at:](https://github.com/fcorowe/cim)

NOTE: The Local Authority Districts for the City of London and Westminster in our shapefile are combined to make our shapefile consistent with our migration data.

Read the shapefile.
```{r}
#Greater_London <- readOGR(dsn = ".", layer = "Greater_London_districts", stringsAsFactors = FALSE)
```

Plot the shapefile
```{r}
#plot(Greater_London)
```

Obtain the differences in the spatial distribution of the population aged 65 and over across local authority districts using the CIM.Duncan function:
```{r}
#CIM.duncan <- CIM(pop65over, pop1_14, pop15_29, pop30_44, pop45_64, calculation = "duncan", numerator = 1, DuncanAll= TRUE)
#Dun_65over <- CIM.duncan$duncan_results
```

Visualise the results
```{r}
#head(Dun_65over)
```

Append these data to the shapefile using the local authority names as joiner
```{r}
#Duncan_65p <- merge(Greater_London, Dun_65over, by.x = "name", by.y = 0)
#head(Duncan_65p@data)
```

Set to a static map view and create a map using tmap
```{r}
#tmap_mode('plot')

#tm_shape(Duncan_65p) +
#  tm_polygons("ASShareFV_diff", style="quantile",border.alpha = 0.1, palette = "YlOrRd", 
#              title="ASShareFV_diff")+
#  tm_compass(position = c("left", "bottom")) +
#  tm_scale_bar(position = c("left", "bottom"))
```

Or, even better we can create an interactive map! by setting an interactive map view
```{r}
#tmap_mode('view')
#tm_shape(Duncan_65p) +
#  tm_polygons("ASShareFV_diff", style="quantile",border.alpha = 0.1, palette = "YlOrRd", 
#              title="ASShareFV_diff")+
#  tm_compass(position = c("left", "bottom")) +
#  tm_scale_bar(position = c("left", "bottom"))
```

### Citation

How to cite if you use the package:

[Rowe, F., Patias, N., Rodríguez-Vignoli, J. (2018). CIM: Compositional Impact of Migration. R package version 1.0.0.](https://zenodo.org/badge/latestdoi/162118396)

If you use the method: 

[Rodríguez-Vignoli, J. and Rowe, F., 2018. How is internal migration reshaping metropolitan populations in Latin America? A new method and new evidence. Population studies, 72(2), pp.253-273.](doi.org/10.1080/00324728.2017.1416155)


## License

This project is licensed under the MIT License - see the LICENSE.md file for details

## References

[Rodríguez-Vignoli, J.R. and Rowe, F., 2017. The Changing Impacts of Internal Migration on Residential Socio-Economic Segregation in the Greater Santiago. 28th International Population Conference of the International Union for the Scientific Study of Population (IUSSP), Cape Town, South Africa.](https://www.researchgate.net/publication/321998653_The_Changing_Impacts_of_Internal_Migration_on_Residential_Socio-Economic_Segregation_in_the_Greater_Santiago)

[Rodríguez-Vignoli, J. and Rowe, F., 2018. How is internal migration reshaping metropolitan populations in Latin America? A new method and new evidence. Population studies, 72(2), pp.253-273.](doi.org/10.1080/00324728.2017.1416155)
