# Impact of Contextualised and Non-Contextualised Word Embeddings on Classification Performance

Repository for the project in the course 732A92 Text Mining at Linköping University 2019.

## Purpose

This is a project we have to conduct for the above mentioned course. This repository will include the source code (`src`), some notebooks for mingling around (`notebooks`) and the report itself (`report`).

## Getting Started

First clone the repository locally:

```git clone https://github.com/flennic/text-mining-project```

Next you need the data set itself as well as the word embeddings.
The dataset [Amazon Reviews for Sentiment Analysis](https://drive.google.com/open?id=0Bz8a_Dbh9QhbZVhsUnRWRDhETzA) can be found on Google Drive and is originally provided by Xiang Zhang. The data is expected to be in a folder `data/original/` but you can change the path in the `settings.json` if desired.
You will also need the Word2Vec embeddings, which can be found e.g. [here](https://github.com/mmihaltz/word2vec-GoogleNews-vectors). The embeddings are expected to be in `data/original/`, which can also be changed in the `settings.json` file.

If you want your sweet automation, just do `chmod +x fetch-data.sh` and then `./fetch-data.sh` and the script will download the data for you. Be aware that the script does not have any error handling, so use it with caution. It also assumes default paths.

For starting and training a model, you call `main.py`. The default settings fault in `main.py` will be overwritten by your settings file. If you don't specify an entry, the default from `main.py` is taken automatically.

You will find an example `settings.json` further down this readme.

## Requirements

I strongly suggest having at least 20GB of free disk space, 16GB of memory and a dedicated graphics card, otherwise you won't have fun running this project. The default settings assume above mentioned requirements and 8GB of VRAM, if you have less, feel free to reduce thge batch sizes. Be aware that training times might drastically increase.

The preprocessing will take some time, so be patient. Leave the `cache` parameter set to true, so that you only have to do it once.

You should install CUDA for make the GPU work [CUDA](https://developer.nvidia.com/cuda-downloads).

For the following I suggest setting up a new anaconda environment. You will need the following libraries:

- PyTorch 1.3.1 (probably also works with others, but you're probably best with this one for this project).
    - `conda install -c pytorch pytorch`
- The [transformers](https://github.com/huggingface/transformers) library from huggingface.
    - `pip install transformers`
- [gensim](https://radimrehurek.com/gensim/) is used for managing Word2Vec embeddings. This is just for my personal convenience.
    - `pip install gensim`

## Disclaimer

Please be aware that this is just a small project aimed at two weeks of full time work, so the code might be edgy in some parts. Nor is this code intended to be used in a productive environment in any sense.

## `config.json`

```json

{
    "orig_train_path": "data/original/train.csv",
    "orig_test_path": "data/original/test.csv",
    "processed_data_folder": "data/processed/",
    "cached_model_path": "checkpoints/2019-12-12_22-9_LstmWord2VecModelInteractor.pth",

    "word2vec_path": "data/embeddings/GoogleNews-vectors-negative300.bin",
    "splits": [0.85, 0.1, 0.05],
    "padding": 200,
    "embeddings": 1000000,
    "categories": 5,
    "run_model": "lstm_w2v",
    "load_cached_model": true,

    "models": {
        "ffn_w2v": {
            "data_loader_workers": 4,
            "batch_size": 8192,
            "learning_rate": 0.05,
            "epochs": 1,
            "embedding_size": 300,
            "dropout": 0.25,
            "hidden": 256
        },
        "lstm_w2v": {
            "data_loader_workers": 4,
            "batch_size": 1024,
            "learning_rate": 0.05,
            "epochs": 1,
            "embedding_size": 300,
            "dropout": 0.25,
            "lstm_layers": 2,
            "lstm_hidden": 128,
            "lstm_dropout": 0.25,
            "gradient_clip": 5
        }
    },

    "seed": 42,
    "cache": true
}
```

## License information

See the attached license file for further notice.

**I hereby explicitly prohibit using any the parts of this repository with respect to the course, tldr: don't copy my project!**
