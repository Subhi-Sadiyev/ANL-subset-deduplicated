# ANL-subset-deduplicated

Code and notebooks for reproducing **208,293 Azerbaijani text rows** filtered and deduplicated from [LocalDoc/AzTC-full](https://huggingface.co/datasets/LocalDoc/AzTC-full).

**Dataset:** [Hugging Face](https://huggingface.co/datasets/subhi-sadiyev/ANL-subset-deduplicated)

## Environment and setup

The notebooks were run in the official PySpark notebook image, which provides Jupyter and a configured Spark environment.

Install the additional FastText dependency in a notebook cell:

```python
%pip install --only-binary=:all: fasttext-numpy2==0.10.4
```

Keep the notebooks and dataset directories in the same working directory. Download the original dataset’s Parquet shards into `aztc_full_localdoc/`.

## Reproduce the results

Run the notebooks in this order:

1. `EDA.ipynb` — explore the source data and download the `lid.176.bin` language model.
2. `post_process.ipynb` — filter, deduplicate, and export to `processed_data/`.
3. `verify_dataset.ipynb` — verify the saved dataset.

The processing and verification notebooks reuse the language model at `/tmp/lid.176.bin`. Run them in the same environment after EDA.

Processing retains ANL text rows of at least 500 characters, applies character and Azerbaijani-language filters, removes exact duplicates, and performs approximate MinHash deduplication at a 90% similarity cutoff. Retained text remains unchanged.

Near-duplicate features and pairs are saved under `near_duplicated_data_post/`. Keep these files for verification.

## Output

The completed run produces **208,293 text rows** in:

```text
processed_data/train-00000-of-00001.parquet
```

The dataset contains two string columns: `text` and `source`. All retained rows have `source = ANL`.

## Attribution

Original corpus by **[LocalDoc](https://huggingface.co/LocalDoc)**, published under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). See the [source dataset card](https://huggingface.co/datasets/LocalDoc/AzTC-full) for attribution and citation.