# -*- coding: utf-8 -*-
# @Author :Taoli
# @Email :taolideng@gmail.com
# @Time :2018-03-23 13:30:04
from __future__ import division
from scipy import linalg
import numpy as np
import pandas as pd
import ffn
import math
import matplotlib.pyplot as plt
import datetime as dt
import pandas_datareader.data as web
import statsmodels.api as sm

import tushare as ts
datas=ts.get_k_data('000001',start='2014-01-01',end='2017-01-01')

datas.index=pd.to_datetime(datas.date)

close=datas.close

#计算前5天平均作为第1期
ema5_number1=np.mean(close[0:5])
#计算第6天加权平移
ema5_number2=0.2*close[5]+(1-0.2)*ema5_number1

ema5=pd.Series(0.00,index=close.index)

ema5[4]=ema5_number1
ema5[5]=ema5_number2

for i in range(6,len(close)):
    expo=np.array(sorted(range(i-4),reverse=True))
    w=(1-0.2)**expo
    ema5[i]=sum(0.2*w*close[5:(i+1)])+ema5_number1*0.2**(i-5)

plt.plot(close[4:])

plt.plot(ema5[4:])

import k_picture
ema6=k_picture.ema_call(close,5,0.2)

plt.plot(ema6[4:])
