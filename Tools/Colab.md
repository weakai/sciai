---
title: colab
---

## 文件操作

```sh
from google.colab import files

uploaded = files.upload()

for fn in uploaded.keys():
  print('User uploaded file "{name}" with length {length} bytes'.format(
      name=fn, length=len(uploaded[fn])))

# 下载目录下的文件
files.download('example.txt')

# 挂载 Gdrive
from google.colab import drive
drive.mount('/content/drive')

with open('/content/drive/My Drive/foo.txt', 'w') as f:
  f.write('Hello Google Drive!')

!cat /content/drive/My\ Drive/foo.txt

# 将笔记的修改同步到云盘中去
drive.flush_and_unmount()
print('All changes made in this colab session should now be visible in Drive.')
```

