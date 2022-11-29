## ATSEN

The source code used for [Distantly-Supervised Named Entity Recognition with Adaptive Teacher Learning and Fine-grained Student Ensemble](),published in AAAI 2023.

## Framework

![](https://github.com/zenhjunpro/ATSEN/blob/main/image/%E6%A1%86%E6%9E%B6.png)

## Requirements

At least one GPU is required to run the code.

enviroment:

- apex==0.1
- python==3.7.4
- pytorch==1.6.0
- tranformers==4.19.3
- numpy==1.21.6
- tqdm==4.64.0
- ...

you can see the enviroment in requirements.txt or you can use `pip3 install -r requirements.txt` to create environment

## Benchmark

The reuslts (entity-level F1 score) are summarized as follows:

|  Method   |  CoNLL03  | OntoNotes5.0 |  Twitter  |
| :-------: | :-------: | :----------: | :-------: |
|   BOND    |   81.48   |    68.35     |   48.01   |
|   SCDL    |   83.69   |    68.61     |   51.09   |
| **ATSEN** | **85.59** |  **68.95**   | **52.46** |

## Motivation

![](https://github.com/zenhjunpro/ATSEN/blob/main/image/2.png)

```
def _update_mean_model_variables(stu_model, teach_model, alpha, global_step,t_total,param_momentum):
    m = get_param_momentum(param_momentum,global_step,t_total)
    for p1, p2 in zip(stu_model.parameters(), teach_model.parameters()):    
        tmp_prob = np.random.rand()
        if tmp_prob < 0.8:
            pass
        else:
            p2.data = m * p2.data + (1.0 - m) * p1.detach().data
            
def get_param_momentum(param_momentum,current_train_iter,total_iters):

    return 1.0 - (1.0 - param_momentum) * (
        (math.cos(math.pi * current_train_iter / total_iters) + 1) * 0.5
    )
```



## Reproducing the Results

We provide three bash scripts `run_conll03.sh`,`run_ontonotes5.sh`,`run_webpage.sh` for running the model on the three datasets.

you can run the code like:

```
 sh <run_dataset>.sh <GPU ID> <DATASET NAME>
```

e.g.

```
sh run_conll03.sh 0,1 conll03
```

The bash scripts include arguments,they are important and need to be set carefully:

- `GPUID`:

- `DATASET` :

- `TOKENIZER_TYPE` :
- `STUDENT1_TYPE` :
- `STUDENT2_TYPE` :
- `TOKENIZER_NAME` :
- `STUDENT1_MODEL_NAME` :
- `STUDENT2_MODEL_NAME` :
- `LR` :
- `WARMUP` :
- `BEGIN_EPOCH` :
- `PERIOD` :

- `MEAN_ALPHA`:
- `THRESHOLD`:
- `TRAIN_BATCH`:
- `EPOCH`:
- `LABEL_MODE`:
- `WEIGHT_DECAY`:
- ``SEED:`
- `ADAM_EPS`:
- `ADAM_BETA1`:
- `ADAM_BETA2`:
- `EVAL_BATCH`:

## Running on New Datasets



## Models

We provide the models in this [page]().You can reproduce the results of the experiment.The result  we do can see in [log.txt](https://github.com/zenhjunpro/ATSEN/blob/main/log.txt)

## Notes and Acknowledgments

The implementation is based on https://github.com/AIRobotZhang/SCDL
