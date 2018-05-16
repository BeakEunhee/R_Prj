AdTracking\_2
<<<<<<< HEAD
================

(1) 필요한 데이타 불러오기
==========================

==========================================
==========================================

=======
=======================================================
(1) 필요한 데이타 불러오기
=======================================================
>>>>>>> 149519c7f1c914c7b7c4429369dea3dd01b32fa9
``` r
source("ad_init.r")
```

    ## Warning: package 'stringr' was built under R version 3.4.3

``` r
source("ad_function.r")

ad_df      = readFile(paste0(path_newData, "newSample.csv"))
ad_app     = readFile(paste0(path_newData, "ad_app.csv"))
ad_channel = readFile(paste0(path_newData, "ad_channel.csv"))
ad_cl_d    = readFile(paste0(path_newData, "ad_cl_d.csv"))
ad_cl_h    = readFile(paste0(path_newData, "ad_cl_h.csv"))
ad_device  = readFile(paste0(path_newData, "ad_device.csv"))
ad_os      = readFile(paste0(path_newData, "ad_os.csv"))
```

``` r
head(ad_app, 10)
```

    ##    app down notdown  click down_click down_RT down_WT
    ## 1    3   85  339122 339207    0.00025 0.01899 0.00005
    ## 2   64    4   18713  18717    0.00021 0.00089 0.00000
    ## 3    9  179  164732 164911    0.00109 0.03999 0.00044
    ## 4   14   30   99800  99830    0.00030 0.00670 0.00002
    ## 5   15   34  160174 160208    0.00021 0.00760 0.00002
    ## 6    2   48  215535 215583    0.00022 0.01072 0.00002
    ## 7    6    3   24887  24890    0.00012 0.00067 0.00000
    ## 8   13    7   43275  43282    0.00016 0.00156 0.00000
    ## 9   25    0   14684  14684    0.00000 0.00000 0.00000
    ## 10  23    1   26719  26720    0.00004 0.00022 0.00000

<<<<<<< HEAD
(2) 각 변수별로 해당 down\_WT값 찾아서 넣어주기
===============================================

==========================================
==========================================

=======
=======================================================
(2) 각 변수별로 해당 down\_WT값 찾아서 넣어주기
=======================================================
>>>>>>> 149519c7f1c914c7b7c4429369dea3dd01b32fa9
``` r
len = nrow(ad_df)

ls_ip       = c()
ls_app      = c()
ls_channel  = c()
ls_cl_d     = c()
ls_cl_h     = c()
ls_device   = c()
ls_os       = c()
ls_is       = c()

for(i in 1 : len)
{
  t_ip      = ad_df[i, 'ip']
  t_app     = ad_df[i, 'app']
  t_channel = ad_df[i, 'channel']
  t_cl_d    = ad_df[i, 'cl_d']
  t_cl_h    = ad_df[i, 'cl_h']
  t_device  = ad_df[i, 'device']
  t_os      = ad_df[i, 'os']
  t_is      = ad_df[i, 'is_attributed']
  
  ls_ip[i]      = t_ip
  ls_app[i]     = ad_app[ad_app$app == t_app, 'down_WT']
  ls_channel[i] = ad_channel[ad_channel$channel == t_channel, 'down_WT']
  ls_cl_d[i]    = ad_cl_d[ad_cl_d$cl_d == t_cl_d, 'down_WT']
  ls_cl_h[i]    = ad_cl_h[ad_cl_h$cl_h == t_cl_h, 'down_WT']
  ls_device[i]  = ad_device[ad_device$device == t_device, 'down_WT']
  ls_os[i]      = ad_os[ad_os$os == t_os, 'down_WT']
  ls_is[i]      = t_is
  
  # if(ls_app[i] == 0 & ls_channel[i] == 0)
}

df_ = data.frame(ls_ip, ls_app, ls_channel, ls_cl_d, ls_cl_h, ls_device, ls_os, ls_is)
colnames(df_) = c('ip', 'app', 'channel', 'cl_d', 'cl_h', 'device','os', 'is_attributed')

rm(ls_ip)
rm(ls_app)
rm(ls_channel)
rm(ls_cl_d)
rm(ls_cl_h)
rm(ls_device)
rm(ls_os)
rm(ls_is)

f_name = paste0(path_newData, "ad_down_RT.csv")
writeFile(df_, f_name)
rm(df_)
```

<<<<<<< HEAD
(3) 변수별 down\_WT값 곱해주고 곱해준 값(RT\_WT)을 파생변수로 추가
==================================================================

==========================================
==========================================

=======
=======================================================
(3) 변수별 down\_WT값 곱해주고 곱해준 값(RT\_WT)을 파생변수로 추가
=======================================================
>>>>>>> 149519c7f1c914c7b7c4429369dea3dd01b32fa9
``` r
ratio_df = readFile(paste0(path_newData, "ad_down_RT.csv"))
head(ratio_df, 10)
```

    ##        ip     app channel    cl_d    cl_h  device      os is_attributed
    ## 1   83230 0.00005   1e-05 0.00064 0.00100 0.01193 0.00205             0
    ## 2    5323 0.00000   0e+00 0.00064 0.00044 0.01193 0.00205             0
    ## 3   42102 0.00005   1e-05 0.00064 0.00044 0.01193 0.00205             0
    ## 4  179519 0.00005   1e-05 0.00064 0.00044 0.01193 0.00014             0
    ## 5   12955 0.00005   1e-05 0.00064 0.00044 0.01193 0.00205             0
    ## 6   88537 0.00044   1e-05 0.00064 0.00030 0.01193 0.00205             0
    ## 7   21354 0.00002   0e+00 0.00064 0.00030 0.01193 0.00205             0
    ## 8  104366 0.00044   1e-05 0.00064 0.00030 0.01193 0.00013             0
    ## 9  171396 0.00002   0e+00 0.00064 0.00030 0.01193 0.00002             0
    ## 10 209663 0.00002   2e-05 0.00064 0.00030 0.01193 0.00029             0

``` r
# col = c('app', 'channel', 'cl_h', 'cl_d', 'device')
col = c('app', 'channel', 'device')
col_len = length(col)
len = nrow(ratio_df)
RT_WT = c()

RT_WT = round((((ratio_df[, col[1]] + (1 / sq_val_10)) * sq_val_10) *
               ((ratio_df[, col[2]] + (1 / sq_val_10)) * sq_val_10) *
               ((ratio_df[, col[3]] + (1 / sq_val_10)) * sq_val_10)) / ((sq_val_10  ^ (col_len - 1)) * 1), sq_val)
  
# View(head(RT_WT, 100))
```

``` r
ratio_df_multi = cbind(ratio_df, RT_WT)
f_name = paste0(path_newData, "ad_final.csv")
writeFile(ratio_df_multi, f_name)

rm(RT_WT)
rm(ratio_df)
rm(ratio_df_multi)
```

<<<<<<< HEAD
(4) 파생변수 RT\_WT가 어느 정도영향을 주는지 확인해 보기
========================================================

==========================================
==========================================

=======
=======================================================
(4) 파생변수 RT\_WT가 어느 정도영향을 주는지 확인해 보기
=======================================================
>>>>>>> 149519c7f1c914c7b7c4429369dea3dd01b32fa9
``` r
ratio_df = readFile(paste0(path_newData, "ad_final.csv"))
head(ratio_df, 10)
```

    ##        ip     app channel    cl_d    cl_h  device      os is_attributed
    ## 1   83230 0.00005   1e-05 0.00064 0.00100 0.01193 0.00205             0
    ## 2    5323 0.00000   0e+00 0.00064 0.00044 0.01193 0.00205             0
    ## 3   42102 0.00005   1e-05 0.00064 0.00044 0.01193 0.00205             0
    ## 4  179519 0.00005   1e-05 0.00064 0.00044 0.01193 0.00014             0
    ## 5   12955 0.00005   1e-05 0.00064 0.00044 0.01193 0.00205             0
    ## 6   88537 0.00044   1e-05 0.00064 0.00030 0.01193 0.00205             0
    ## 7   21354 0.00002   0e+00 0.00064 0.00030 0.01193 0.00205             0
    ## 8  104366 0.00044   1e-05 0.00064 0.00030 0.01193 0.00013             0
    ## 9  171396 0.00002   0e+00 0.00064 0.00030 0.01193 0.00002             0
    ## 10 209663 0.00002   2e-05 0.00064 0.00030 0.01193 0.00029             0
    ##    RT_WT
    ## 1  0e+00
    ## 2  0e+00
    ## 3  0e+00
    ## 4  0e+00
    ## 5  0e+00
    ## 6  1e-05
    ## 7  0e+00
    ## 8  1e-05
    ## 9  0e+00
    ## 10 0e+00

``` r
down_     = ratio_df[ratio_df$is_attributed == 1, ]
not_      = ratio_df[ratio_df$is_attributed == 0, ]

down_len  = nrow(down_)
not_len   = nrow(not_)

down_len_1  = nrow(down_[down_$RT_WT > 0.00003, ])
not_len_1   = nrow(not_[not_$RT_WT > 0.00003, ])

print(paste0("    down :: ", down_len_1,"/", down_len
             , " >> "
             , round((down_len_1 / down_len), sq_val)))
```

    ## [1] "    down :: 3858/4476 >> 0.86193"

``` r
print(paste0("not down :: ", not_len_1,"/", not_len
             , " >> "
             , round((not_len_1 / not_len), sq_val)))
```

    ## [1] "not down :: 125123/1844563 >> 0.06783"

``` r
rm(down_)
rm(not_)
```
