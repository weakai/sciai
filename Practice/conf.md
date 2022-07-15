# PyTorch 配置与日志

```python
torch.__version__
```

## 环境变量

```python
DATASET_PATH = os.environ.get("PATH_DATASETS", "data/")
CHECKPOINT_PATH = os.environ.get("PATH_CHECKPOINT", "saved_models/Activation_Functions/")

os.makedirs(CHECKPOINT_PATH, exist_ok=True)
```

### jupyter notebook

```python
%matplotlib inline
```

## 随机数初始化

```python
# Function for setting the seed
def set_seed(seed):
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    if torch.cuda.is_available():  # GPU operation have separate seed
        torch.cuda.manual_seed(seed)
        torch.cuda.manual_seed_all(seed)

set_seed(42)
# GPU 的随机锁定
torch.backends.cudnn.determinstic = True
torch.backends.cudnn.benchmark = False
```

## 设备

```python
os.environ['CUDA_VISIBLE_DEVICES'] = '0'
device = torch.device("cpu") if not torch.cuda.is_available() else torch.device("cuda:0")
```

## 命令行参数

```python
# 命令主题说明
parser = argparse.ArgumentParser(usage = 'peptide-HLA-I binding prediction')
parser.add_argument('--peptide_file', type = str, help = 'the path of the .fasta file contains peptides')
parser.add_argument('--threshold', type = float, default = 0.5, help = 'the threshold to define predicted binder, float from 0 - 1, the recommended value is 0.5')
parser.add_argument('--cut_peptide', type = bool, default = False, help = 'Whether to split peptides larger than cut_length?')
parser.add_argument('--cut_length', type = int, default = 9, help = 'if there is a peptide sequence length > 15, we will segment the peptide according the length you choose, from 8 - 15')

# 此时可以查看默认设置的参数
args = parser.parse_args(args = [])
args
```
