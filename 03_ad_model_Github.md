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

```{r}
ad_df = readFile("ad_final.csv")
```

(2) 데이터 셋 
========================================

=======================================
=======================================

```{r}
selCol = c("ip", "app",  "device", "os", "channel", "cl_h",  "RT_WT")
ad_df$cl_h = as.integer(ad_df$cl_h)

tr_index   = createDataPartition(ad_df$is_attributed, p = 0.7, list = FALSE)
dtrain     = ad_df[ tr_index,]
dvalid     = ad_df[-tr_index,]
```

(3) LightGBM 모델적용
========================================

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

=======================================
=======================================

<p style="font-weight:bold;font-size=30px;">
>> lightgbm 결과값
</p>

<p style="font-weight:bold;">
[LightGBM] [Info] Number of positive: 3083, number of negative: 1291245<br />
[LightGBM] [Info] Total Bins 20165<br />
[LightGBM] [Info] Number of data: 1294328, number of used features: 7<br />
[1]:    validation's auc:0.92445<br />
[101]:  validation's auc:0.956123
</p><br /><br />

(4) XGboost 모델적용
========================================

=======================================
=======================================

```{r}
model_train = xgb.DMatrix(data = as.matrix(dtrain[, selCol])
                          , label = dtrain$is_attributed)
model_val   = xgb.DMatrix(data = as.matrix(dvalid[, selCol])
                          , label = dvalid$is_attributed)

params = list(objective = "binary:logistic"
               , booster = "gbtree"
               , eval_metric = "auc"
               , nthread = 8
               , eta = 0.05
               , max_depth = 10
               , gamma = 0.9
               , subsample = 0.8
               , colsample_bytree = 0.8
               , scale_pos_weight = 50
               , nrounds = 100)

myxgb_model = xgb.train(params
                        , model_train
                        , params$nrounds
                        , list(val = model_val)
                        , print_every_n = 20
                        , early_stopping_rounds = 50)


rm(model_train)
rm(model_val)
rm(params)
rm(myxgb_model)
```

=======================================
=======================================

<p style="font-weight:bold;font-size=30px;">
>> XGboost 결과값
</p>

<p style="font-weight:bold;">
[1]     val-auc:0.932501<br />
Will train until val_auc hasn't improved in 50 rounds.<br /><br />

[21]    val-auc:0.958273<br />
[41]    val-auc:0.959588<br />
[61]    val-auc:0.958954<br />
Stopping. Best iteration:<br />
[30]    val-auc:0.959616<br />
</p>
