Untitled
================

``` r
rm(list = ls())
path = "D:/08_R_Prj/02_data/ad_tracking/"
# path = "D:/00_Beky/01_R_prj/data/adTracking/"
ad_df = read.csv(paste0(path, "train_sample.csv"), na.strings=c("NA", ""), stringsAsFactors = F)
head(ad_df, 20)
```

    ##        ip app device os channel          click_time attributed_time
    ## 1   87540  12      1 13     497 2017-11-07 09:30:38            <NA>
    ## 2  105560  25      1 17     259 2017-11-07 13:40:27            <NA>
    ## 3  101424  12      1 19     212 2017-11-07 18:05:24            <NA>
    ## 4   94584  13      1 13     477 2017-11-07 04:58:08            <NA>
    ## 5   68413  12      1  1     178 2017-11-09 09:00:09            <NA>
    ## 6   93663   3      1 17     115 2017-11-09 01:22:13            <NA>
    ## 7   17059   1      1 17     135 2017-11-09 01:17:58            <NA>
    ## 8  121505   9      1 25     442 2017-11-07 10:01:53            <NA>
    ## 9  192967   2      2 22     364 2017-11-08 09:35:17            <NA>
    ## 10 143636   3      1 19     135 2017-11-08 12:35:26            <NA>
    ## 11  73839   3      1 22     489 2017-11-08 08:14:37            <NA>
    ## 12  34812   3      1 13     489 2017-11-07 05:03:14            <NA>
    ## 13 114809   3      1 22     205 2017-11-09 10:24:23            <NA>
    ## 14 114220   6      1 20     125 2017-11-08 14:46:16            <NA>
    ## 15  36150   2      1 13     205 2017-11-07 00:54:09            <NA>
    ## 16  72116  25      2 19     259 2017-11-08 23:17:45            <NA>
    ## 17   5314   2      1  2     477 2017-11-09 07:33:41            <NA>
    ## 18 106598   3      1 20     280 2017-11-09 03:44:35            <NA>
    ## 19  72065  20      2 90     259 2017-11-06 23:14:08            <NA>
    ## 20  37301  14      1 13     349 2017-11-06 20:07:00            <NA>
    ##    is_attributed
    ## 1              0
    ## 2              0
    ## 3              0
    ## 4              0
    ## 5              0
    ## 6              0
    ## 7              0
    ## 8              0
    ## 9              0
    ## 10             0
    ## 11             0
    ## 12             0
    ## 13             0
    ## 14             0
    ## 15             0
    ## 16             0
    ## 17             0
    ## 18             0
    ## 19             0
    ## 20             0

``` r
library(lubridate)
```

    ## Warning: package 'lubridate' was built under R version 3.4.3

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

``` r
cl_y_m_d = ymd(as.Date(ad_df$click_time))
cl_h = as.POSIXlt(ad_df$click_time)$hour
cl_y = year(cl_y_m_d)
cl_q = quarter(cl_y_m_d)
cl_m = month(cl_y_m_d)
cl_d = day(cl_y_m_d)

at_y_m_d = ymd(as.Date(ad_df$attributed_time))
at_h = as.POSIXlt(ad_df$attributed_time)$hour
at_y = year(at_y_m_d)
at_q = quarter(at_y_m_d)
at_m = month(at_y_m_d)
at_d = day(at_y_m_d)

ad_df = cbind(ad_df, cl_d, cl_h)
```

``` r
# ad_df
down_app = ad_df[ad_df$is_attributed == 1, ]
not_app = ad_df[ad_df$is_attributed == 0, ]
```

``` r
attributed_ <- function(tmp_down, tmp_not, tmp_all, tmp_col)
{
  ls_val = c()
  ls_1 = c()
  ls_2 = c()
  ls_3 = c()
  ls_4 = c()
  ls_5 = c()
  
  tmp_ls   = unique(tmp_all[, tmp_col])
  len = length(tmp_ls)
  
  for(i in 1 : len)
  {
    tmp_val   = tmp_ls[i]
    tmp_1     = length(tmp_down[tmp_down[, tmp_col] == tmp_val , tmp_col])
    tmp_2     = length(tmp_not[tmp_not[, tmp_col] == tmp_val , tmp_col])
    tmp_3     = length(tmp_all[tmp_all[, tmp_col] == tmp_val , tmp_col])
    tmp_4     = round(tmp_1 / tmp_3, 4)
    # tmp_5     = round(tmp_2 / tmp_3, 4)
    
    ls_val[i] = tmp_val
    ls_1[i]   = tmp_1
    ls_2[i]   = tmp_2
    ls_3[i]   = tmp_3
    ls_4[i]   = tmp_4
    # ls_5[i]   = tmp_5
  }
  
  for(i in 1 : len)
    ls_5[i]   = round(ls_1[i] / sum(ls_1), 4)

  df_ = data.frame(ls_val, ls_1, ls_2, ls_3, ls_4, ls_5)

  colnames(df_) = c(tmp_col, 'down', 'notdown', 'click', 'down/click', 'down_ratio')
  
  rm(ls_val)
  rm(ls_1)
  rm(ls_2)
  rm(ls_3)
  rm(ls_4)
  rm(ls_5)
  
  return(df_)
}
```

``` r
search = 'channel'            # 'app'     :: 어플 별 클릭 수, 다운 수, 다운받지 않은 수 알고싶을때.
tmp_down = down_app       # 다운DATA
tmp_not = not_app         # 광고만 노출됐고 다운받지 않음 DATA 
tmp_all = ad_df           # (down_app + not_app) DATA

result_1 = attributed_(tmp_down, tmp_not, tmp_all, search)
View(result_1[result_1$down > 0, ])

rm(tmp_down)
rm(tmp_not)
rm(tmp_all)

# View(result_1)
```

![](ad_Tracking_180420_files/figure-markdown_github/pressure-1.png)
