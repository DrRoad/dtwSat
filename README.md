---
output:
  md_document:
    variant: markdown_github
---

<!-- README.md is generated from README.Rmd. Please edit that file -->

dtwSat
=====

### Time-Weighted Dynamic Time Warping for remote sensing time series analysis
The dtwSat provides a Time-Weighted Dynamic Time Warping (TWDTW) algorithm to measure similarity between two temporal sequences. This adaptation of the classical Dynamic Time Warping (DTW) algorithm is flexible to compare events that have a strong time dependency, such as phenological stages of cropland systems and tropical forests. This package provides methods for visualization of minimum cost paths, time series alignment, and time intervals classification.

### Install

```r
devtools::install_github("vwmaus/dtwSat")
```


### Quick demo

This dome performs a dtwSat analysis and show the results.

```r
library(dtwSat)
names(query.list)
```

```
## [1] "Soybean" "Cotton"  "Maize"
```

```r
alig = twdtw(query.list[["Soybean"]], template, 
             weight = "logistic", alpha = 0.1, beta = 50, alignments=4, keep=TRUE) 
print(alig)
```

```
## Time-Weighted DTW alignment object
## Alignments:
##   query       from         to distance normalizedDistance
## 1     1 2011-10-04 2012-01-28 3.956483         0.03140066
## 2     1 2012-10-06 2013-02-15 4.008838         0.03181617
## 3     1 2009-09-13 2010-03-05 4.539202         0.03602541
## 4     1 2010-10-20 2011-03-18 5.528445         0.04387655
```

### Plot examples

Plot alignments

```r
library(dtwSat)
library(gridExtra)
gp1 = plot(alig, type="alignment", attribute="evi", alignment=1, shift=0.5) + 
          ggtitle("Alignment 1") +
		      theme(axis.title.x=element_blank())
gp2 = plot(alig, type="alignment", attribute="evi", alignment=2, shift=0.5) +
          ggtitle("Alignment 2") + 
          theme(legend.position="none")
grid.arrange(gp1,gp2,nrow=2)
```

![plot of chunk define-demo-plot-alignments](figure/define-demo-plot-alignments-1.png) 


Plot path for all classese

```r
library(dtwSat)
library(gridExtra)
gp.list = lapply(query.list, function(query){
  				alig = twdtw(query, template, weight = "logistic", alpha = 0.1, beta = 50,
  				             alignments = 4, keep = TRUE)
  				plot(alig, normalize = TRUE, show.dist = TRUE) + 
  				  ggtitle(names(query.list)[2]) +   
  				  theme(axis.title.x=element_blank(),
  				        legend.position="none")
})
grid.arrange(gp.list[[1]],
             gp.list[[2]],
             gp.list[[3]],
             nrow=3)
```

![plot of chunk define-demo-plot-paths](figure/define-demo-plot-paths-1.png) 


<ol>
   <li>Plot classification:
 		<code>
			malig = mtwdtw(query.list, template, weight = "logistic", 
               alpha = 0.1, beta = 100)
 
      gp = plot(x=malig, type="classify", attribute="evi", from=as.Date("2009-09-01"),  
              to=as.Date("2013-09-01"), by = "6 month",
              normalized=TRUE, overlap=.7) 
      gp
      </code>
  </li>
</ol>
![alt text](README-classify.png "Classification plot")


<ol>
  <li>Plot alignments: <code>
	df = data.frame(Time=index(template), value=template$evi, variable="Raw")
	df = rbind( df, data.frame(Time=index(sy), value=sy$evi, variable="Wavelet filter") )
	gp = ggplot(df, aes(x=Time, y=value, group=variable, colour=variable)) +
  		geom_line() + 
  		theme(legend.position="bottom") +
  		ylab("EVI")
	gp
        </code>
   </li>
</ol>
  
![alt text](README-filter.png "Smoothing plot")

<h3>How to build the package:</h3>
<ol>
	<li>Clone the project: <code>git clone https//github.com/vwmaus/dtwSat.git</code>.</li>
	<li>Open Rstudio, go to File - Open Project and pick the file <code>dtwSat.Rproj</code>.</li>
	<li>Install the required packages <code>install.packages(c("roxygen2", "testthat"))</code>.</li>
	<li>Go to the <i>Build</i> tab in the upper-right panel and press the button <i>Build & Reload</i>. After this the package is ready to use.</li>
	<li>You can also create a source package: Go to the <i>Build</i> tab, display the menu <i>More</i> and select the option <i>Build Source Package</i>.</li>
</ol> 



