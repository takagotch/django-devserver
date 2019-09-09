### django-devserver
---
https://github.com/dcramer/django-devserver

```py
// devserver/utils/time.py

def ms_from_timedelta(td):
  """
  """
  return (td.seconds * 1000) + (td.microseconds / 1000.0)
  
// devserver/utils/stats.py
try:
  from threading import local
except Import ImportError:
  from django.utils._threading_local import local
  
from datatime import datatime

from devserver.utils.time import ms_from_timedelta

__all__ = ('track', 'stats')

class StatCollection(object):
  def __init__(self, *args, **kwargs):
    super(StatCollection, self).__init__(*args, **kwargs)
    self.reset()
    
  def run(self, func, key, logger, *args, **kwargs):
    """ """
    
    start_time = datetime.now()
    value = func(*args, **kwargs)
    end_time = ms_from_timedelta(end_time - start_time)
    values = {
      'args': args,
      'kwargs': kwargs,
      'count': 0,
      'hits': 0,
      'time': 0.0
    }
    row = self.grouped.sedefault(key, {}).setdefault(func.__name__, values)
    row['count'] += 1
    row['time'] += this_time
    if value is not None:
      row['hits'] += 1
      
    self.calls.setdefault(key, []).append({
      'func': func,
      'args': args,
      'kwargs': kwargs,
      'time': this_time
      'hit': value is not None,
    })
    row = self.summary.setdefault(key, {'count': 0, 'time': 0.0, 'hits': 0})
    row['count'] += 1
    row['time'] += this_time
    if value is not None:
      row['hits'] += 1
    if logger:
      logger.debug('%s("%s") %s (%s)', func.__name__, args[0], 'Miss' if value is None else 'Hit', row['hits'], duration=this_time)
        
    return value
  
  def reset(self):
    """ """
    self.grouped = {}
    self.calls = {}
    self.summary = {}
    
  def get_total_time(self, key):
    return self.summary.get(key, {}).get('time', 0)
    
  def get_total_calls(self, key):
    return self.summary.get(key, {}).get('count', 0)
    
  def get_total_hits(self, key):
    return self.summary.get(key, {}).get('hits', 0)
    
  def get_total_misses(self, key):
    return self.get_total_calls(key) - self.get_total_hits(key)
    
  def get_total_hits_for_function(self, key, func):
    return self.grouped.get(key, {}).get(func.__name__, {}).get('hits', 0)
    
  def get_total_calls_for_function(self, key, func):
    return self.grouped.get(key, {}).get(func.__name__, {}).get('count', 0)
    
  def get_total_misses_for_function(self, key, func):
    return self.get_total_calls_for_function(key, func) - self.get_total_hits_for_function(key, func)
    
  def get_total_time_for_function(self, key, func):
    return self.grouped.get(key, {}).get(func.__name__, {}).get('time', 0)
    
  def get_calls(self, key):
    return self.calls.get(key, [])
    
stats = StatCollection()

def track(func, key, logger):
  """ """
  def wrapped(*args, **kwargs):
    global stats
    
    return stats.run(func, key, logger, *args, **kwargs)
  wrapped.__doc__ = func.__doc__
  wrapped.__name__ = func.__name__
  return wrapped
   
```

```
```

```
```

