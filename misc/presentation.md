```r
install.packages("remotes")
remotes::install_github("rsquaredata/mmrClustVar")
```


```r
library(mmrClustVar)
data(mtcars)

actives <- mtcars[, c('mpg', 'disp', 'hp', 'drat', 'wt', 'qsec')]
descriptives <- mtcars[, c('cyl', 'vs', 'am', 'gear')]

obj <- Kmeans$new(K=3)
obj$fit(actives)
obj$summary()
```


```r
obj$predict(descriptives)
obj$plot("clusters")
```


```r
obj$plot("membership")
obj$interpret_clusters()
```


```r
data("metal_universe")

interface <- Interface$new("auto", K=3)
interface$fit(metal_universe[, 1:5])
interface$summary()
```


```r
interface$get_method()
```


```r
run_mmrClustVar_app()
```