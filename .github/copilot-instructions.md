# Copilot Instructions for Brain-D-450mm

## Project Overview
This project is a data processing and analysis pipeline for Brazilian rainfall gauge data, organized in Jupyter notebooks and Python scripts. The workflow covers data cleaning, quality control, interpolation, and classification, with all intermediate and final datasets stored in HDF5 files.

## Key Directories & Files
- `Scripts e Dados/`: Main directory containing all notebooks and scripts.
  - `1.1 - 1.8`: Data standardization, cleaning, and pre-classification.
  - `2.x`: Quality control steps (availability, gaps, cycles, outliers, etc.).
  - `3.x`: Grid generation and interpolation (IDW, cross-validation).
  - `4.x`: Climate normals and comparison analyses.
  - `1 - Organized data gauge/`: Contains raw and processed HDF5 datasets.

## Data Handling Patterns
- **HDF5 Storage**: All large datasets are stored in HDF5 format using pandas (`to_hdf`, `read_hdf`, `HDFStore`). Use `format='table'` for chunked reading/writing.
- **Chunk Processing**: Functions like `read_hdf_chunks` and `write_hdf_chunks` are used for memory-efficient data handling. Always check if a table is in 'table' format before chunked operations.
- **Table Management**: Use `delete_hdf_table` to remove tables from HDF5 files. Use `show_hdf_tables` to list available tables.

## Workflow Conventions
- **Notebook Structure**: Each notebook represents a distinct step in the pipeline. Parameters and file paths are set at the top. Outputs are written to HDF5 for downstream steps.
- **Quality Control**: Outlier detection and quality metrics are computed and stored in separate tables. Merging and filtering are performed using pandas joins.
- **Parallelization**: Use `joblib.Parallel` for compute-heavy metrics (e.g., consecutive dry days).
- **Interpolation**: IDW interpolation is performed using `scipy.spatial.KDTree`.

## Developer Workflows
- **Data Reload**: Always reload data from HDF5 after writing to ensure consistency.
- **Table Naming**: Use descriptive table names (e.g., `table_data`, `table_info`, `table_outlier_filter_1_export`).
- **Error Handling**: Functions catch and print errors (e.g., missing files/keys) and return empty DataFrames.
- **Memory Management**: Delete intermediate variables and chunks to free memory after large operations.

## Example Patterns
```python
# Chunked reading
read_hdf_chunks(cleaned_path, key='table_data', chunksize=1_000_000)

# Table deletion
delete_hdf_table(cleaned_path, 'table_data_outlier_filtered')

# List tables
show_hdf_tables(cleaned_path)
```

## Integration Points
- **External Libraries**: pandas, numpy, scipy, joblib
- **File Paths**: Relative paths are used for HDF5 files; update as needed for your environment.

## Recommendations for AI Agents
- Follow notebook order for pipeline steps.
- Use provided utility functions for HDF5 operations.
- Prefer chunked operations for large datasets.
- Always check table format before chunked reads/writes.
- Document any new table names and update downstream notebooks accordingly.

---

_If any section is unclear or missing important details, please provide feedback to improve these instructions._
