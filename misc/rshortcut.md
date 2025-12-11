## Clean data
> Remove NA rows
```r
clean_df <- na.omit(df)
```
> Remove columns containing NA
```r
df_clean <- df[, colSums(is.na(df)) == 0]
```
## Select data
> Select columns by name
```r
selected <- df[, c("col1", "col2")]
```
> Select quqntitative columns
```r
numeric_cols <- df[, sapply(df, is.numeric)]
```
> Select qualitative columns
```r
qualitative_cols <- df[, sapply(df, function(x) is.factor(x) || is.character(x))]
```
## Check data
> Get column type
```r
class(df$col1)
```
