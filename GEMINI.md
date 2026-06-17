# Gemini Integration

This file contains documentation and prompts specific to Gemini's role in this repository. Gemini is configured to follow the global rules specified in `.agentrules` and assist with the development of the BRain-D project.

When interacting with this repository, Gemini must adhere to both the global project rules and the following technical development guidelines, which have been adapted from the project's core AI instructions.

## Project Structure & Data Handling
This project is a data processing and analysis pipeline for Brazilian rainfall gauge data. The workflow covers data cleaning, quality control, interpolation, and classification.

### HDF5 Storage Patterns
- **Format**: All large datasets are stored in HDF5 format using `pandas` (`to_hdf`, `read_hdf`, `HDFStore`). Always use `format='table'` for chunked reading/writing capabilities.
- **Chunk Processing**: Due to the massive size of the daily rainfall data, functions like `read_hdf_chunks` and `write_hdf_chunks` are strictly used for memory-efficient data handling. Always verify a table is in 'table' format before chunked operations.
- **Table Management**: Use `delete_hdf_table` to remove tables from HDF5 files. Use `show_hdf_tables` to list available tables. Use descriptive table names (e.g., `table_data`, `table_info`, `table_outlier_filter_1_export`).

### Workflow & Execution Conventions
- **Notebook Order**: Each notebook (`1.x`, `2.x`, `3.x`, `4.x`) represents a distinct step in the pipeline. Follow the numerical order strictly. Parameters and file paths are set at the top of each notebook.
- **Quality Control**: Outlier detection and quality metrics are computed and stored in separate tables. Merging and filtering are performed using pandas joins.
- **Parallelization**: When implementing compute-heavy metrics (e.g., consecutive dry days), use `joblib.Parallel` for parallelization.
- **Interpolation**: IDW interpolation is performed using `scipy.spatial.KDTree`.

### Developer Practices for Gemini
- **Data Reload**: Always reload data from HDF5 after writing to ensure consistency.
- **Error Handling**: Catch and print errors (e.g., missing files/keys) and return empty DataFrames when appropriate to prevent pipeline crashes.
- **Memory Management**: Explicitly delete intermediate variables and chunks to free memory after large operations, invoking the garbage collector if necessary.
- **Git Commits**: As defined in `.agentrules`, commit messages must be extremely verbose and detailed, utilizing the sequential 3-digit zero-padded build numbering pattern (e.g., `[Build #XXX]`).
