# simpleTorchWrapper

<img src="./logo.jpg" alt="simpleTorchWrapper" title="simpleTorchWrapper" width="300" />


A general framework for PyTorch classification and regression tasks. It is lightweight, easy to use, and currently under development.

## Installation

- In Google Colab, run the following code to download `pyTorchWrapper`.

```
import os
import sys

import os
import sys

try:
    if not os.path.isdir('/content/simpleTorchWrapper'):
        raise FileNotFoundError
except FileNotFoundError:
    print("(◕‿◕✿) Downloading simpleTorchWrapper from GitHub.")
    os.system(f'git clone https://github.com/shuliu2017/simpleTorchWrapper.git')
except Exception as e:
    print(f"(◕‿◕✿) An unexpected error occurred: {e}")

sys.path.append('/content/simpleTorchWrapper')
```

- Install requirements
  
```
!pip install -r /content/pyTorchWrapper/requirements.txt
```

## Available Models

- efficientNetB2
- efficientNetV2S
- VIT

## Load Modules

```
# autoreload
# %load_ext autoreload
# %autoreload 2

import simple_torch_wrapper as  stw
```

## Model Training


- Example

```
epochs = 6

stw.pytorch_tools.set_random_seed(seed=0)

model = stw.models.vit_regressor.ViTRegressor()
device = stw.pytorch_tools.get_device()
model = model.to(device)
model = stw.pytorch_tools.enable_multi_gpu(model)

model_name = 'vit_regressor'
loss_fn = torch.nn.MSELoss()
task_type = 'regression'

optimizer = torch.optim.Adam(params=model.parameters(), lr=0.001)
early_stopping = stw.workflow.EarlyStopping(patience=8)
metrics = stw.customized_metrics.regression_metrics
result = stw.workflow.train(model=model,
                                  train_dataloader=train_loader,
                                  validation_dataloader=val_loader,
                                  optimizer=optimizer,
                                  loss_fn=loss_fn,
                                  metrics=metrics,
                                  task_type=task_type,
                                  epochs=epochs,
                                  early_stopping=early_stopping,
                                  save_freq=2,
                                  device=device)
```

- Regression
  
```
loss_fn = torch.nn.MSELoss()
task_type = 'regression'
metrics = stw.customized_metrics.regression_metrics # MSE, MAE, R2; evaluated per epoch
```

- Classification

```
loss_fn = torch.nn.CrossEntropyLoss()
task_type = 'classification'
metrics = stw.customized_metrics.classification_metrics # Accuracy, Recall, Precision, F1; evaluated per epoch
```

- Commonly used optimizers

```
torch.optim.Adam(params=model.parameters(), lr=0.001)
torch.optim.SGD(params=model.parameters(), lr=0.001)
```

## Model Evaluation

```
test_model = stw.models.vit_regressor.ViTRegressor().to(device)
stw.pytorch_tools.load_model_state(test_model, target_dir='/content', model_name= f'model_checkpoint.pt')
test_result = stw.workflow.evaluation_step(test_model, test_loader, loss_fn, metrics, task_type, device)
```

## Example Notebooks

- regression [simple regression on random noise](https://colab.research.google.com/github/shuliu2017/pyTorchWrapper/blob/main/notebooks/simple_regression.ipynb)


## Team

LYL is an independent research and development team made up of PhDs in computer science, statistics, and physics. We are dedicated to creating innovative applications and conducting cutting-edge research to simplify daily life and make life more enjoyable.

## License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

## Acknowledgements

This project is inspired by the code from other open-source projects. We would like to thank the authors of these projects for their contributions:

- [PyTorch](https://pytorch.org/)
- [Zero to Mastery Learn PyTorch for Deep Learning](https://www.learnpytorch.io/)

