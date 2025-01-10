# Epydemix Data

This repository contains real-world population data and synthetic contact matrices used for epidemic modeling and simulation in the [**epydemix**](https://github.com/ngozzi/epydemix/tree/main) python package. The data covers demographic distributions and various contact matrices for more than $400$ regions worldwide.

The contact matrices indicate interactions between individuals in different contexts (e.g., home, work, school, community) and are sourced from the following studies:
- [Inferring high-resolution human mixing patterns for disease modeling](https://www.nature.com/articles/s41467-020-20544-y) (`mistry_2021`)
- [Projecting contact matrices in 177 geographical regions: An update and comparison with empirical data for the COVID-19 era](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1009098) (`prem_2021`)
- [Projecting social contact matrices in 152 countries using contact surveys and demographic data](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1005697) (`prem_2017`)

When using the contact data provided by the **Epydemix** package please ensure that you cite the relevant research papers associated with each data source.

## Supported Geographies

A comprehensive list of supported geographies can be found in the [locations.csv](https://github.com/ngozzi/epydemix-data/blob/main/locations.csv) file. This file provides detailed information about the available contact matrices and population data for each location. A sample of the file is shown below:

| **location**    | **primary_contact_source** | **mistry_2021** | **prem_2021** | **prem_2017** | **population_source** |
|-----------------|----------------------------|-----------------|---------------|---------------|------------------------|
| Afghanistan     | prem_2021                   | False          | True          | False         | https://population.un.org/wpp/Download/Standard/CSV/ |
| Albania         | prem_2021                   | False          | True          | True          | https://population.un.org/wpp/Download/Standard/CSV/ |
| Algeria         | prem_2021                   | False          | True          | True          | https://population.un.org/wpp/Download/Standard/CSV/ |
| Andorra         | prem_2021                   | False          | False         | True          | https://population.un.org/wpp/Download/Standard/CSV/ |
| Angola          | prem_2021                   | Fals           | True          | False         | https://population.un.org/wpp/Download/Standard/CSV/ |
| ...             | ...                         | ...            | ...           | ...           | ...                    |


The file contains the following information:
- **Location Names**: The geographic regions for which contact and demographic data are available.
- **Primary Contact Source**: The default contact matrix source used by **Epydemix** for each location. If a specific contact source isn't specified, **Epydemix** will attempt to import the primary source listed. When available, Mistry 2021 is prioritized as the primary source, followed by Prem 2021, and then Prem 2017.
- **Availability of Contact Matrices**: The file indicates whether contact matrices are available from the Mistry 2021, Prem 2021, or Prem 2017 studies for each location. Some locations may have multiple sources, while others may only have one.
- **Population Data Source**: The file also provides the source of demographic data, primarily from the [United Nations World Population Prospects 2024](https://population.un.org/wpp/Download/Standard/CSV/).


### Example Folder Structure: `United_States`

Each location folder is organized as follows:

```
United_States/
    ├── demographic/
    │   └── age_distribution.csv
    ├── contact_matrices/
        ├── mistry_2021/
        │   ├── contacts_matrix_all.csv
        │   ├── contacts_matrix_home.csv
        │   ├── contacts_matrix_work.csv
        │   ├── contacts_matrix_community.csv
        │   └── contacts_matrix_school.csv
        ├── prem_2017/
        │   ├── ...
        └── prem_2021/
            ├── ...
```
Where:
- **`demographic/`**: Contains demographic data for the population of the United States.
  - **`age_distribution.csv`**: A CSV file detailing the population distribution by age group.
  
- **`contact_matrices/`**: This directory contains the contact matrices, which describe interaction patterns between different age groups in various contexts.
  - **`mistry_2021/`**: Contains contact matrices from the Mistry 2021 study, with data separated by context (e.g., home, work, school, community).
    - **`contacts_matrix_all.csv`**: Aggregated contact matrix across all contexts.
    - **`contacts_matrix_home.csv`**: Contact matrix for interactions within households.
    - **`contacts_matrix_work.csv`**: Contact matrix for workplace interactions.
    - **`contacts_matrix_community.csv`**: Contact matrix for community-based interactions.
    - **`contacts_matrix_school.csv`**: Contact matrix for interactions in schools.
  - **`prem_2017/`**: Contains similar contact matrix files from the Prem 2017 study, broken down by the same contexts (home, work, school, community, all).
  - **`prem_2021/`**: Contains updated contact matrices from the Prem 2021 study, also structured by context.

---

### Using Data with the **Epydemix** Package

The **Epydemix** package provides flexibility in loading demographic data and contact matrices for various regions. The `epydemix.model.load_epydemix_population` function allows you to load this data either via **online import** (fetching from a remote source) or **offline import** (loading from a local directory).

- **Online Import**: If `path_to_data` is not provided, **Epydemix** will attempt to import the data directly from GitHub.
- **Offline Import**: If `path_to_data` is provided, **Epydemix** will attempt to load the data from a local directory. For this option, users must first download the corresponding data folder.

### Example: Loading Population Data for the United States

Below is an example of how to load population data for the United States, specifying both online and offline import options:

```python
from epydemix.model import load_epydemix_population

# Example 1: Online import (data will be fetched from GitHub)
population_online = load_epydemix_population(
    population_name="United_States",
    # Specify the preferred contact data source (needed only if you want to override the default primary source)
    contacts_source="mistry_2021",  
    layers=["home", "work", "school", "community"]  # Load contact layers (by default all layers are imported)
)

# Example 2: Offline import (data will be loaded from a local directory)
# Ensure that the folder is downloaded locally before running this
population_offline = load_epydemix_population(
    population_name="United_States",
    path_to_data="path/to/local/epydemix_data/",  # Path to the local data folder
    # Specify the preferred contact data source (needed only if you want to override the default primary source)
    contacts_source="mistry_2021", 
    layers=["home", "work", "school", "community"]  # Load contact layers (by default all layers are imported)
)

# Use the population in your epidemic model
model.set_population(population=population_online)  # Using online data
# or
model.set_population(population=population_offline)  # Using offline data
```

---

For any issues or questions regarding this data, please contact the maintainers of the **Epydemix** package.

