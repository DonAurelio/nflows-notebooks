# nFlows: Notebooks

Analysis notebooks used to analyze [nFlows](https://github.com/DonAurelio/nflows) traces geerated with [nFlows-experiments](https://github.com/DonAurelio/nflows-experiments).

## Directory Structure

```
.
├── 1_improvement_violin.ipynb
├── 2_locality_violin.ipynb
├── 3_balance_violin.ipynb
├── 4_migrations_violin.ipynb
├── cleaned_all.csv
├── LICENSE
├── README.md
└── requirements.txt
```

The `cleaned_all.csv` dataset is **not included** in this repository. You must download it from:

> Vivas, A. & Castro, H. (2025). *Scheduling strategies for efficient data movement in data-intensive scientific workflow execution: Execution Traces and Experimental Configurations for NUMA-Aware Workflow Scheduling* [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.17625295

## Usage

### Improvement metric

Make sure the following variables are set as follows. `algorithm_mode = 'N'` indicates the non-NUMA-aware version of `fifo`; it can be switched to `'A'` to indicate the NUMA-aware version of `fifo`.

```python
path = './cleaned_all.csv'
df_condition = (df_system_pivot['algorithm_name'] == 'fifo')
algorithm_mode = 'N'
```

### Locality, balance, and page migrations

For locality, balance, and page migrations, an additional setting variable is needed when melting the dataframe. `2` and `4` denote the number of NUMA nodes considered in a given execution scenario, while `A` and `N` stand for the (A)ware and (N)on-aware versions of the scheduling algorithm. In the paper, we uncommented only `2N` and `4N`, or `2A` and `4A`.

```python
df_melt = pd.melt(
    df_plot,
    id_vars=["workflow_class", "system_cores", "system_name", "workflow_instance"],
    value_vars=[
        # "1L",
        # "1R",
        # "2A",
        "2N",
        # "4A",
        "4N",
    ],
    var_name="metric",
    value_name="value"
)
```