# Google Colaboratory Notebook Valuenet

This repository contains notebooks to run [Valuenet](https://github.com/brunnurs/valuenet) codebase on [Google Colaboratory](https://colab.research.google.com/)

The training on colab for the custom data take ~4h for 50 epochs with a batch_size of 4.

### Configuration

Just put these files in your Google Colaboratory environment.

### Notebooks
There are then 3 notebooks:
- generate_sql_statement_and_questions_hack_zurich_2021.ipynb: 

    This notebook allow you to generated some new custom data for training [Valuenet](https://github.com/brunnurs/valuenet)
    
- preprocess_custom_data_hack_zurich_2021.ipynb:

    This notebook prepares the custom data. If you generate some custom data (with the help of `generate_sql_statement_and_questions_hack_zurich_2021.ipynb` for instance), this notebooks takes you through the preprocessing steps to be ready to train.
    
- training_valuenet_hack_zurich_2021.ipynb:

    This notebooks is responsible for the training part. Make sure to import generated data from `generate_sql_statement_and_questions_hack_zurich_2021.ipynb` first if you use custom data. It then guides through the options and steps to train a model from scratch or pre-trained and to train on spider dataset or with your custom data.
