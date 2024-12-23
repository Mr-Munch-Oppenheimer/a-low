# 基本操作

```python
import tushare as ts
ts.set_token('57b81c613f14d6d45c832968542884a0fe97b8e2fcda6f1fc67d17c3')
pro = ts.pro_api()
```

```python
import pandas as pd
import os
import joblib
```

```python
stock_basic = pd.read_csv("stock_basic.csv")
stock_basic
```

```python
trade_cal = pd.read_csv("trade_cal.csv")
trade_cal
```

```python
custom_date = pd.read_csv("custom_date.csv")
start_date=str(custom_date.start_date[0])
end_date=str(custom_date.end_date[0])
print(start_date)
print(end_date)
```

```python
if not os.path.isdir("daily_basic"):
    os.makedirs("daily_basic")
    print("已创建daily_basic文件夹")
else:
    print("daily_basic文件夹已存在")
```

```python
#每日指标
all_index = stock_basic.shape[0]
df_index = pd.to_datetime(trade_cal.cal_date.tolist(),format='%Y%m%d')
for index, row in stock_basic.iterrows():
    stock_name = row.ts_code
    print(index, all_index, stock_name)
    current_df = pro.daily_basic(ts_code=stock_name,start_date=start_date,end_date=end_date)
    current_df.sort_values(by='trade_date',ascending=True,inplace=True)
    current_df.reset_index(inplace=True,drop=True)
    current_df.trade_date=pd.to_datetime(current_df.trade_date.tolist(),format='%Y%m%d')
    current_df.index=current_df.trade_date
    standard_df = pd.DataFrame(index=df_index,columns=current_df.columns.tolist())
    for column in current_df.columns:
        standard_df[column]=current_df[column]
    standard_df.trade_date=standard_df.index.tolist()
    standard_df.ts_code = stock_name
    standard_df.to_csv("daily_basic/"+stock_name+".csv",index=0)
```

# A股低频

| 研究     |                                                              |
| -------- | ------------------------------------------------------------ |
| 股票池   | 上市日期<br />ST<br />停牌<br />涨跌停<br />指数成分股（指数增强） |
| 单因子   | 行业市值中性化<br />相关系数<br />分组回测                   |
| 多因子   | 机器学习<br />深度学习                                       |
| 风险控制 | 年化收益<br />波动率<br />夏普<br />最大回撤                 |
| 实盘     | QMT                                                          |

# Record

|            |         |         |                                                              |
| ---------- | ------- | ------- | ------------------------------------------------------------ |
| 2024/10/24 | 闲鱼    | TuShare | f558cbc6b24ed78c2104e209a8a8986b33ec66b7c55bcfa2f46bc108     |
| 2024/11/15 | 闲鱼    | TuShare | import tushare as ts<br/># 初始化pro接口<br/>pro = ts.pro_api('20241114201856-4f34155e-3cd2-4a42-b798-df1713e24f07')<br/>pro._DataApi__http_url = 'http://tsapi.majors.ltd:7000' |
| 2024/11/16 | TuShare | TuShare | 57b81c613f14d6d45c832968542884a0fe97b8e2fcda6f1fc67d17c3     |

## 2024/10/27

- 交易日历
- 交易标的

## 2024/11/06

- 日线行情

## 2024/11/08

- 日线行情 (multiindex)

  ```python
  df.set_index(['trade_date','ts_code']).sort_index()
  ```


## 2024/11/10

- 生成因子合成DataFrame

## 2024/11/15

- return_-x 未来x天收益率
- return_x 过去x天收益率

## 2024/11/16

- ST
- 停牌
- 涨跌停
- 指数成分股
- 行业

# 其他

| Data           |                                                              |                                                              |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Download       | 交易日历<br />股票列表<br />日线行情<br />申万行业           | \                                                            |
| Clean          | 丢失数据                                                     | \                                                            |
|                | 股票池                                                       | ST<br />停牌<br />开盘涨停                                   |
| Feature/Factor | 趋势<br />动量<br />成交量<br />波动率<br />支撑与阻力<br />图表模式 | \                                                            |
|                | 盈利能力<br />成长性<br />财务健康状况<br />估值<br />其他   | \                                                            |
| Model/Strategy | \                                                            | \                                                            |
|                | 风险控制                                                     | Total Returns<br />Total Annualized Returns<br />Algorithm Volatility<br />Sharpe<br />Max Drawdown |
|                | 仓位管理                                                     | \                                                            |
| 实盘           | \                                                            | \                                                            |
