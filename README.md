BitJam provides a data structure to monitor and detect traffic congestion.
It can be used for rate limiting, as back-pressure mechanism, or some sort
of interval sampling.

`pip install bitjam`

```python

>>> import bitjam
>>> import time
>>> rate_limit = bitjam.CongestionMonitor(capacity=6, duration=20)
>>> rate_limit.count_observed('x')
0
>>> for _ in range(3): rate_limit.allows('x')
...
True
True
True
>>> rate_limit.count_allowed('x')
3
>>> rate_limit.count_rejected('x')
0
>>> rate_limit.is_saturated('x')
False
>>> time.sleep(10)
>>> for _ in range(3): rate_limit.allows('x')
...
True
True
True
>>> rate_limit.is_saturated('x')
True
>>> rate_limit.allows('x')
False
>>> rate_limit.count_rejected('x')
1
>>> rate_limit.count_observed('x')
7
>>> time.sleep(10)
>>> rate_limit.is_saturated('x')
False
>>> rate_limit.allows('x')
True
>>> time.sleep(10)
>>> rate_limit.count_allowed('x')
1
>>> rate_limit.count_rejected('x')
0
>>> rate_limit.count_observed('x')
1

```
