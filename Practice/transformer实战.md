# Tiltle

## first

transformer 这个模型已经在 tensorflow/tensor2tensor tensor2tensor 里面封装好了。直接拿过来用就行。

## 创建目录

```sh
PROBLEM=translate_ende_wmt32k
MODEL=transformer
HPARAMS=transformer_base_single_gpu

DATA_DIR=./t2t_data
TMP_DIR=./tmp/t2t_datagen
TRAIN_DIR=./t2t_train/$PROBLEM/$MODEL-$HPARAMS

mkdir -p $DATA_DIR $TMP_DIR $TRAIN_DIR
```

## Portal

<https://zhuanlan.zhihu.com/p/51245148>

[tensor2tensor github page](https://github.com/tensorflow/tensor2tensor)
