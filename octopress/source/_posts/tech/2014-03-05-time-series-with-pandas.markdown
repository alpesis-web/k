---
layout: post_tech
title: "Time Series with Pandas"
date: 2014-03-05 22:20:32 +0800
comments: true
categories: [tech]
tags: [python, pandas, time series]
---

### 1. date and time

```python
# get datetime from system
from datetime import datetime
now = datetime.now()
now.year, now.month, now.day

# calculate delta between days
delta = datetime(2011, 1, 7) - datetime(2008, 6, 24, 8, 15)
delta.days
delta.seconds

from datetime import timedelta
start = datetime(2011, 1, 7)
start + timedelta(12)
start - 2 * timedelta(12)
```

convert datetime and string

```python
# convert with strftime
stamp = datetime(2011, 1, 3)
str(stamp)
stamp.strftime('%Y-%m-%d')

value = '2011-01-03'
datetime.strptime(value, '%Y-%m-%d')

datestrs = ['7/6/2011', '8/6/2011']
[datetime.strptime(x, '%m/%d/%Y') for x in datestrs]

# converte with parse
from dateutil.parser import parse
parse('2011-01-03')
parse('Jan 31, 1997 10:45 PM')
parse('6/12/2011', dayfirst=True)


# coverte with pandas
pandas.to_datetime(datestrs)
idx = pd.to_datetime(datestrs + [None])
idx
idx[2]
pandas.isnull(idx)
```


### 2. time series

create dates with pandas

```python
from datetime import datetime

dates = [datetime(2011, 1, 2), datetime(2011, 1, 5), datetime(2011, 1, 7), 
         datetime(2011, 1, 8), datetime(2011, 1, 10), datetime(2011, 1, 12)]

ts = pandas.Series(np.random.randn(6), index=dates)
ts

type(ts)
ts.index
ts.index.dtype

stamp = ts.index[0]
stamp
```

indexing, selection, subsetting

```python
stamp = ts.index[2]
ts[stamp]
ts['1/10/2011']
ts['20110110']

longer_ts = pandas.Series(np.random.randn(1000), index=pd.date_range('1/1/2000', periods=1000))
longer_ts['2001']
longer_ts['2001-05']

ts[datetime(2011, 1, 7):]
ts['1/6/2011':'1/11/2011']

ts.truncate(after='1/9/2011')

dates = pandas.date_range('1/1/2000', periods=100, freq='W-WED')
long_df = pandas.DataFrame(np.random.randn(100, 4), index=dates, columns=['Colorado', 'Texas', 'New York', 'Ohio'])
```


time series with duplicate indices

```python
dates = pandas.DatetimeIndex(['1/1/2000', '1/2/2000', '1/2/2000', '1/2/2000', '1/3/2000'])
dup_ts = pandas.Series(np.arange(5), index=dates)
dup_ts.index.is_unique
dup_ts['1/3/2000']
dup_ts['1/2/2000']

grouped = dup_ts.groupby(level=0)
grouped.mean()
grouped.count()
```

date ranges, frequencies, shifting

```python
# 1. resample
ts
ts.resample('D')

# 2. date_range
index = pandas.date_range('4/1/2012', '6/1/2012')

pandas.date_range(start='4/1/2012', periods=20)
pandas.date_range(end='6/1/2012', periods=20)
pandas.date_range('1/1/2000', '12/1/2000', freq='BM')

pandas.date_range('5/2/2012 12:56:31', periods=5)
pandas.date_range('5/2/2012 12:56:31', periods=5, normalize=True)

# 3. frequencies

from pandas.tseries.offsets import Hour, Minute
hour = Hour()
four_hours = Hour(4)
pandas.date_range('1/1/2000', '1/3/2000 23:59', freq='4h')

Hour(2) + Minute(30)
pandas.date_range('1/1/2000', periods=10, freq='1h30min')

# 4. week of month
rng = pandas.date_range('1/1/2012', '9/1/2012', freq='WOM-3FRI')
list(rng)

# 5. shifting (leading/lagging)
ts = pandas.Series(np.random.randn(4),index=pandas.date_range('1/1/2000', periods=4, freq='M'))
ts.shift(2)
ts.shift(-2)
ts.shift(2, freq='M')
ts.shift(3, freq='D')
ts.shift(1, freq='3D')
ts.shift(1, freq='90T')

# shifting dates with offsets
from pandas.tseries.offsets import Day, MonthEnd
now = datetime(2011, 11, 17)
now + 3 * Day()
now + MonthEnd()
now + MonthEnd(2)

offset = MonthEnd()
offset.rollforward(now)
offset.rollback(now)

ts = pandas.Series(np.random.randn(20),index=pandas.date_range('1/15/2000', periods=20, freq='4d'))
ts.groupby(offset.rollforward).mean()
ts.resample('M', how='mean')
```


### 3. timezone handling

```python
import pytz
pytz.common_timezones[-5:]
tz = pytz.timezone('US/Eastern')
```

localization and conversion

```python
rng = pandas.date_range('3/9/2012 9:30', periods=6, freq='D')
ts = pandas.Series(np.random.randn(len(rng)), index=rng)
print(ts.index.tz)

pandas.date_range('3/9/2012 9:30', periods=10, freq='D', tz='UTC')
ts_utc = ts.tz_localize('UTC')
ts_utc.index
ts_utc.tz_convert('US/Eastern')

ts_eastern = ts.tz_localize('US/Eastern')
ts_eastern.tz_convert('UTC')
ts_eastern.tz_convert('Europe/Berlin')
ts.index.tz_localize('Asia/Shanghai')
```

operations with timezone

```python
stamp = pandas.Timestamp('2011-03-12 04:00')
stamp_utc = stamp.tz_localize('utc')
stamp_utc.tz_convert('US/Eastern')
stamp_moscow = pandas.Timestamp('2011-03-12 04:00', tz='Europe/Moscow')

stamp_utc.value
stamp_utc.tz_convert('US/Eastern').value

from pandas.tseries.offsets import Hour
stamp = pandas.Timestamp('2012-03-12 01:30', tz='US/Eastern')
stamp + Hour()
stamp = pandas.Timestamp('2012-11-04 00:30', tz='US/Eastern')
stamp + 2 * Hour()

rng = pandas.date_range('3/7/2012 9:30', periods=10, freq='B')
ts = pandas.Series(np.random.randn(len(rng)), index=rng)
ts1 = ts[:7].tz_localize('Europe/London')
ts2 = ts1[2:].tz_convert('Europe/Moscow')
result = ts1 + ts2
result.index
```


### 4. periods

periods of year, month, quarter

```python
# period of years
p = pandas.Period(2007, freq='A-DEC')
p + 5
p - 2
pandas.Period('2014', freq='A-DEC') - p

# period of months
rng = pandas.period_range('1/1/2000', '6/30/2000', freq='M')
pandas.Series(np.random.randn(6), index=rng)

# period of quarter
values = ['2001Q3', '2002Q2', '2003Q1']
index = pandas.PeriodIndex(values, freq='Q-DEC')
```

period frequencies conversion

```python
p = pandas.Period('2007', freq='A-DEC')
p.asfreq('M', how='start')
p.asfreq('M', how='end')

p = pandas.Period('2007', freq='A-JUN')
p.asfreq('M', 'start')
p.asfreq('M', 'end')

p = pandas.Period('2007-08', 'M')
p.asfreq('A-JUN')

rng = pandas.period_range('2006', '2009', freq='A-DEC')
ts = pandas.Series(np.random.randn(len(rng)), index=rng)
ts.asfreq('M', how='start')
ts.asfreq('B', how='end')
```


quarterly period frequencies

```python
p = pandas.Period('2012Q4', freq='Q-JAN')
p.asfreq('D', 'start')
p.asfreq('D', 'end')

# to get the timestamp at 4PM on the 2nd to last business day of the quarter
p4pm = (p.asfreq('B', 'e') - 1).asfreq('T', 's') + 16 * 60
p4pm.to_timestamp()

rng = pandas.period_range('2011Q3', '2012Q4', freq='Q-JAN')
ts = pandas.Series(np.arange(len(rng)), index=rng)
new_rng = (rng.asfreq('B', 'e') - 1).asfreq('T', 's') + 16 * 60
ts.index = new_rng.to_timestamp()
```

converting timestamps to periods

```python
rng = pandas.date_range('1/1/2000', periods=3, freq='M')
ts = pandas.Series(np.random.randn(3), index=rng)
pts = ts.to_period()

rng = pandas.date_range('1/29/2000', periods=6, freq='D')
ts2 = pandas.Series(np.random.randn(6), index=rng)
ts2.to_period('M')
pts = ts.to_period()
pts.to_timestamp(how='end')
```

### 5. resampling

```python
rng = pandas.date_range('1/1/2000', periods=100, freq='D')
ts = pandas.Series(np.random.randn(len(rng)), index=rng)
ts.resample('M', how='mean')
ts.resample('M', how='mean', kind='period')
```

downsampling

```python
rng = pandas.date_range('1/1/2000', periods=12, freq='T')
ts = pandas.Series(np.arange(12), index=rng)
ts.resample('5min', how='sum')
ts.resample('5min', how='sum', closed='left')
ts.resample('5min', how='sum', closed='left', label='left')

# ohlc: open high low close
ts.resample('5min', how='ohlc')
ts.resample('5min', how='sum', loffset='-1s')

rng = pandas.date_range('1/1/2000', periods=100, freq='D')
ts = pandas.Series(np.arange(100), index=rng)
ts.groupby(lambda x: x.month).mean()
ts.groupby(lambda x: x.weekday).mean()
```

upsampling and interpolation

```python
frame = pandas.DataFrame(np.random.randn(2, 4),
                         index=pandas.date_range('1/1/2000', periods=2, freq='W-WED'),
                         columns=['Colorado', 'Texas', 'New York', 'Ohio'])
frame[:5]
df_daily = frame.resample('D')
df_daily
frame.resample('D', fill_method='ffill')
frame.resample('D', fill_method='ffill', limit=2)
frame.resample('W-THU', fill_method='ffill')
```

resampling with periods

```python
frame = pandas.DataFrame(np.random.randn(24, 4),
                         index=pandas.period_range('1-2000', '12-2001', freq='M'),
                         columns=['Colorado', 'Texas', 'New York', 'Ohio'])
frame[:5]
annual_frame = frame.resample('A-DEC', how='mean')
annual_frame.resample('Q-DEC', fill_method='ffill')
annual_frame.resample('Q-DEC', fill_method='ffill', convention='start')
annual_frame.resample('Q-MAR', fill_method='ffill')
```
