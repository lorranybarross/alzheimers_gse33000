# Alzheimer's disease in the prefrontal cortex

## Question
How much of an Alzheimer's signal in the prefrontal cortex is disease, and how much is age, sex, or neuron loss?

## Seed
Random seed: 22

## Phase 1
- Created the folder and repository
- Created a new Python environment
- Installed all requirements
    - `ipykernel` and `six` were necessary to run
    - `six` was necessary to run `GEOparse`
- Seed defined
- `/data` removed from commit

### The data
[Series GSE33000](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE33000)

The study focused on discovering which pairs of genes gained or lost correlation in Alzheimer's disease or Huntington's disease, using post-mortem prefrontal cortex tissues from 467 patients and 157 controls. The samples were obtained from the Harvard Brain Tissue Resource Center (HBTRC), and they used the GPL4372 Rosetta/Merck Human 44k 1.1 microarray.

The data chosen was the `GSE33000_series_matrix.txt` because the other `GSE33000_raw_data.txt` doesn't have the `GSM` nor the metadata, which wouldn't enable me to link the expression to the diagnosis.

### Separating data and metadata

I started by importing the pandas library and trying to read the data. Since both data and metadata are in the same file, I used the `comment` parameter to skip all lines from the metadata so I could have the data dataframe, instead of using `skiprows` and placing a fixed number.

For the metadata, I opened the file and read all lines, keeping only the lines starting with `!Sample_`. Since I only wanted the metadata, I ignored the `!Series_` lines because they are about the whole study, not each sample. The last row from the metadata had a `\n`, which I realized when I was comparing IDs, and it was returning `False`. So I cleaned that before creating the dataframe.

There were columns with the same name on the metadata dataframe. I set a name using a label present in each row to remove the duplicates, and added a `_ch1` (reference pool) or `_ch2` (person) based on the previous name to prevent new duplicated column names.

At the end, I verified there were:
- Alzheimer's disease     310
- non-demented            157
- Huntington's disease    157

All IDs are in both dataframes, which was verified by comparing both columns using `pd.Series.equals` and guaranteeing they're in the same order.

In the data dataframe, I got 39280 probes and kept the 624 samples, each as a column.