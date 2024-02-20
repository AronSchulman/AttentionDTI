# MMADTA - MultiModal Attention Drug-Target Affinity prediction

Code repository for the work "Attention-based method to predict drug-target interactions across seven protein classes". Link to master's thesis: https://urn.fi/URN:NBN:fi:aalto-202401282133
![workflow](https://github.com/AronSchulman/MMADTA/assets/63584295/81e0224f-a6e3-4fc1-9d5c-8b727eda8229)


## User instructions

To use the models, you need to download them from Zenodo: [add link]. All the data used for model training and testing are also found there. Place the downloaded `data` directory in the root of this repository.

The relevant script for users is `src/main_user_predict.py`.

Requirements:
```
pandas
numpy
torch
rdkit
```
To make predictions for your drug-target pairs, you need to provide a `.csv` file with the following four columns:
* smiles
* uniprot_id
* protein_class
* model_type

Values in the `protein_class` column should be one or more from the following: `[enzyme, epigenetic_regulator, gpcr, ion_channel, kinase, nuclear_receptor, transporter]`.
Values in the `model_type` column should be either `pchembl` or `interaction_score` depending on your preferred label type.
See the `example_input.csv` for reference.

You need to provide the path to your input file when running the script:
```
python main_user_predict.py -i /path/to/user_input.csv
```

All predictions are mean values generated by an ensemble of five models. It is recommended to run the prediction on a GPU-equipped machine.

After execution, the script produces a `model_output_predictions.csv` file in the same directory as the executed script. The file is identical to the input file, with the addition of a `prediction` column that contains the pairwise affinity predictions in pchembl or interaction score format, depending on your specification.

### Optional

You may provide paths to the model directory and the `model_config_params.json` file with flags `-m` and `-j`, respectively. However, if you do not modify file names and repository structure, the provided default values should work fine.

### Limitations

For the protein descriptors, the user is limited to the pre-calculated values found in `descriptor_data`. If the user supplies an unavailable protein, the descriptor values are filled with zeros. This is not a problem for the subclass label. However, for sequence-based and Zernike descriptors, this may negatively impact the prediction accuracy.

## Reproducing thesis results

For reproducing some of the results in the thesis, we provide the following scripts:
```
train.py
tune.py
test.py
LASSO_feature_selection.py
```
In addition to the previous requirements, these scripts require:
```
ray
hebo
sklearn
scipy
lifelines
matplotlib
seaborn
wandb
```
Do be in touch in case of dependency issues or bugs.
