AdTracking\_3
================

(1) 필요한 데이타 불러오기
==========================

=======================================
=======================================

``` r
library(caret)
library(lightgbm)
source("ad_init.r")
source("ad_function.r")
```
(2) 모델 만들기
========================================

=======================================
=======================================

<p style="font-weight:bold;">
training Data 불러오기
</p>
=======================================
=======================================

```{r}
ad_df = readFile("ad_final.csv")
```

=======================================
=======================================

<p style="font-weight:bold;">
데이터셋 나누기 7 : 3
</p>
=======================================
=======================================

```{r}
selCol = c("ip", "app",  "device", "os", "channel", "cl_h",  "RT_WT")
ad_df$cl_h = as.integer(ad_df$cl_h)

tr_index   = createDataPartition(ad_df$is_attributed, p = 0.7, list = FALSE)
dtrain     = ad_df[ tr_index,]
dvalid     = ad_df[-tr_index,]
```

=======================================
=======================================

<p style="font-weight:bold;">
lightgbm 모델만들기
</p>
=======================================
=======================================

```{r}
dtrain_lgb = lgb.Dataset(data  = as.matrix(dtrain[, selCol])
                         , label = dtrain$is_attributed
                         , categorical_feature = selCol)

dvalid_lgb = lgb.Dataset(data  = as.matrix(dvalid[, selCol])
                         , label = dvalid$is_attributed
                         , categorical_feature = selCol)

params    = list(objective = "binary"
                 , metric = "auc"
                 , learning_rate= 0.1
                 , num_leaves= 7
                 , max_depth= 4
                 , scale_pos_weight= 99.7)

lgb_model = lgb.train(params
                      , dtrain_lgb
                      , valids = list(validation = dvalid_lgb)
                      , nthread = 8
                      , nrounds = 1000
                      , verbose= 1
                      , early_stopping_rounds = 50
                      , eval_freq = 100)

rm(ad_df)
rm(tr_index)
rm(dtrain)
rm(dvalid)
rm(dtrain_lgb)
rm(dvalid_lgb)
rm(params)
invisible(gc())
```
(3) 결과 값
==========================

=======================================
=======================================

<p style="font-weight:bold;">
[LightGBM] [Info] Number of positive: 3083, number of negative: 1291245<br />
[LightGBM] [Info] Total Bins 20165<br />
[LightGBM] [Info] Number of data: 1294328, number of used features: 7<br />
[1]:    validation's auc:0.92445<br />
[101]:  validation's auc:0.956123
</p>
=======================================
=======================================
