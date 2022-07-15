# Pytorch Lightning

## Debugging

有限步骤

```python
# runs 1 train, val, test batch and program ends
trainer = Trainer(fast_dev_run=True)

# runs 7 train, val, test batches and program ends
trainer = Trainer(fast_dev_run=7)
```

小批量

```python
# use only 10% of training data and 1% of val data
trainer = Trainer(limit_train_batches=0.1, limit_val_batches=0.01)

# use 10 batches of train and 5 batches of val
trainer = Trainer(limit_train_batches=10, limit_val_batches=5)
```

理智的验证步骤

```python
# DEFAULT
trainer = Trainer(num_sanity_val_steps=2)
```

小数据过拟合

```python
# use only 1% of training data (and turn off validation)
trainer = Trainer(overfit_batches=0.01)

# similar, but with a fixed 10 batches
trainer = Trainer(overfit_batches=10)
```

## Eearly Stopping

```python
from pytorch_lightning.callbacks.early_stopping import EarlyStopping

class LitModel(LightningModule):
    def validation_step(self, batch, batch_idx):
        loss = ...
        self.log("val_loss", loss)

model = LitModel()
early_stopping = EarlyStopping(monitor="val_acc", min_delta=0.1, patience=3, verbose=False, mode="max")
trainer = Trainer(callbacks=[early_stopping])
trainer.fit(model)
```

## Hyperparameters

### ArgumentParser

```python
from argparse import ArgumentParser

parser = ArgumentParser()
parser.add_argument("--layer_1_dim", type=int, default=128)
args = parser.parse_args()
```