# MP1 Data Pipeline

This project is a command line pipeline that loads, validates, cleans, and saves tabular data using settings from a YAML config file. The data is loaded, checked for required columns and valid numbers, cleaned, summarized in a report, and saved as a CSV. `pipeline.py` runs these steps in order and handles errors, while the reusable code lives in the `src/` package. In `src/`, `data_loaders.py` reads CSV, JSON, and YAML files, `data_validator.py` checks columns and removes invalid values, `data_processor.py` removes duplicates, missing values, and outliers and builds the report, `data_output.py` saves the results, and `utils.py` handles logging and file checks. All cleaning settings are stored in `config/config.yaml`, so the pipeline can process other datasets without changing the code.

## Example

```bash
python pipeline.py --input fixtures/sample_data.csv --output output/clean.csv --config config/config.yaml --verbose
```

Output:

```text
INFO     src.utils — Input file validated: fixtures/sample_data.csv
INFO     src.utils — Input file validated: config/config.yaml
INFO     src.data_loaders — Loaded CSV file: fixtures\sample_data.csv (100 rows)
INFO     src.data_loaders — Loaded YAML file: config\config.yaml
WARNING  src.data_validator — Removed 2 rows with invalid numeric values in rating
DEBUG    src.data_validator — Validation: 100 -> 98 rows
INFO     __main__ — Validation complete: 100 -> 98 rows
DEBUG    src.data_processor — remove_duplicates: 98 -> 96 rows
DEBUG    src.data_processor — handle_missing: 96 -> 94 rows
DEBUG    src.data_processor — rating: method=iqr, threshold=1.5, removed=2
INFO     __main__ — Processing complete: 98 -> 92 rows
DEBUG    src.data_output — Saved 92 rows to output\clean.csv
INFO     __main__ — Saved cleaned data to output\clean.csv

Cleaning report:
{'rows_before': 98, 'rows_after': 92, 'rows_removed': 6, 'columns_before': 5, 'columns_after': 5, 'columns_removed': 0}
```