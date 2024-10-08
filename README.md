# Stochastic CSLR

This is the PyTorch implementation for the ECCV 2020 paper: [Stochastic Fine-grained Labeling of Multi-state Sign Glosses for Continuous Sign Language Recognition](http://www.ecva.net/papers/eccv_2020/papers_ECCV/papers/123610171.pdf).

## Quick Start

### 1. Installation

```bash
pip install git+https://github.com/zheniu/stochastic-cslr
```

Also, you need to install `sclite` for evaluation. Take a look at step 2 for instructions.

### 2. Prepare the dataset

- Download the RWTH-PHOENIX-2014 dataset [here](https://www-i6.informatik.rwth-aachen.de/~koller/RWTH-PHOENIX/).
- Unzip it and obtain the path to `phoenix-2014-multisigner/` folder for later use.
- Install `sclite` for evaluation. Check `phoenix-2014-multisigner/evaluation/NIST-sclite_sctk-2.4.0-20091110-0958.tar.bz2` for detail.
- After installing `sclite`, put it in your `PATH`.

### 3. Run a quick test

You can use the script `quick_test.py` for a quick test.

```bash
python3 quick_test.py --data-root your_path_to/phoenix-2014-multisigner
```

By specifying the model type `--model sfl/dfl`, the data split `--split dev/test`, whether to use a language model`--use-lm`, you can get the following results:

| Model    | WER (dev) | sub/del/ins (dev) | WER (test) | sub/del/ins (test) |
| -------- | --------- | ----------------- | ---------- | ------------------ |
| DFL      | 27.1      | 12.7/7.4/7.0      | 27.7       | 13.8/7.3/6.6       |
| SFL      | 26.2      | 12.7/6.9/6.7      | 26.6       | 13.7/6.5/6.4       |
| DFL + LM | 25.6      | 11.5/9.2/4.9      | 26.4       | 12.4/9.3/4.7       |
| SFL + LM | 24.3      | 11.4/8.5/4.4      | 25.3       | 12.4/8.5/4.3       |

Note that these results are slightly different from the paper as a different random seed is used.

You may also take a look at `quick_test.py` as it shows how to use the pretrained models.

### 4. Train your own model

The configuration files for deterministic and stochastic fine-grained labeling are put under `config/`. The training script is based on a PyTorch experiment runner [torchzq](https://github.com/enhuiz/torchzq/tree/main/torchzq), which automatically reads the hyperparameters in the YAML file and passes them to `stochastic_cslr/runner.py`.

Before running, change the `data_root` in the YAML configurations to `phoenix-2014-multisigner/` first.

#### Train (for instance, dfl)

```bash
tzq config/dfl-fp16.yml train
```

#### Test the trained model

```bash
tzq config/dfl-fp16.yml test
```

## Citation

You may cite this work by:

```bibtex
@inproceedings{niu2020stochastic,
  title={Stochastic Fine-Grained Labeling of Multi-state Sign Glosses for Continuous Sign Language Recognition},
  author={Niu, Zhe and Mak, Brian},
  booktitle={European Conference on Computer Vision},
  pages={172--186},
  year={2020},
  organization={Springer}
}
```
