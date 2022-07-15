# 日志

## lightning

### rank_zero

```python
# rank_zero_warn, rank_zero_debug, rank_zero_deprecation
from pytorch_lightning.utilities import rank_zero_info
rank_zero_info(
    'info 1'
    'info 2'
)
```



## 日志简单过滤

```python
from log import warning

warnings.filterwarnings("ignore")
```

## 初始化日志

```python
import logging

class Logger(object):
    level_relations = {
        'debug':logging.DEBUG,
        'info':logging.INFO,
        'warning':logging.WARNING,
        'error':logging.ERROR,
        'crit':logging.CRITICAL
    }  # 日志级别关系映射

    def __init__(self,filename,level='info',fmt='%(asctime)s : %(message)s'):
        self.logger = logging.getLogger(filename)
        # 设置日志格式
        format_str = logging.Formatter(fmt)
        # 设置日志级别
        self.logger.setLevel(self.level_relations.get(level))
        # 往屏幕上输出
        sh = logging.FileHandler(filename, mode = 'w')
        # 设置屏幕上显示的格式
        sh.setFormatter(format_str)
        # 把对象加到logger里
        self.logger.addHandler(sh)

# 使用
from utils import Logger

## 紧急
    log = Logger('./error.log')
    log.logger.critical('The threshold invalid, please check whether it ranges from 0-1.')
    sys.exit(1)
```