# 🎯Facility Projects Hub

This repository consolidates two independent research projects centered on facility location, quadratic optimization, resource allocation, and fairness.  

The two included projects are:

1. **Heuristics for a capacitated facility location problem with a quadratic objective function**  
2. **Preferential access and fairness in waste management**

---
## 📄 Table of Contents

1. Heuristics for a capacitated facility location problem
   - Requirements for running the code
   - Repository content
   - Example

2. Preferential access and fairness in waste management
   - Repository content
   - Requirements to run code

---
<details>
<summary><h2>📂Heuristics for a capacitated facility location problem with a quadratic objective function</h2></summary>

This project contains the code used in both M. Schmidt's completed master thesis and the article **The Balanced Facility Location Problem: Complexity and Heuristics** by M. Schmidt and B. Singh published in the [INFORMS Journal on Computing](https://pubsonline.informs.org/doi/full/10.1287/ijoc.2024.0693). We provide the implementations of all the algorithms and heuristics discussed in the article as well as the instances we use to test our heuristics on.

---

## ⚙️Requirements for running the code

Before cloning the code, make sure you have [Git Large File Storage (Git LFS)](https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage) installed. Please see instructions for downloading and installing Git LFS on the linked website. 

Note that the data files will not function properly, due to their size, if you just download the zipped file. After installing Git LFS:

1. Clone the repository as usual (`git clone https://github.com/Malena205/heuristics_quadratic_facility_location.git`) to get the actual data files.  
2. Navigate into the folder as usual (`cd`) and  
3. Pull the large data files with Git LFS (`git lfs pull` and then `git lfs install`).  
4. Copy the `Data` folder into the `heuristics_and_mips` folder.

Further, before running the code, make sure you have the below installed and linked with their respective paths:

- python3  
- numpy  
- pandas  
- geopy  
- pyomo  
- gurobi  

---

## 🗂️Repository content

Please refer to the preprint for the terminology. The repository is structured as follows:

- `data`:  
  This folder contains the data for the four instances. For each instance, there is:  
  - a file containing the travel probabilities  $P_{ij}$ (e.g., `instance_1_travel_dict.json.pbz2`)  
  - a file containing the rest of the input data in a dataframe (e.g., `instance_1_users_and_facs.csv`)  

  For each instance we also provide a corresponding instance where the capacity has been scaled to  $C_j = \sum_{i \in I} U_i P_{ij}$ (```instance_1_suff_users_and_facs.csv```). 

  To load the data, we provide the function `load_an_instance`. Example:

```python
users_and_facs_df, travel_dict, users, facs = load_an_instance(1, False)
```
- `heuristics_and_mips`: This folder contains the main part of the code. We briefly describe the purpose of each of the files:
  - `BFLP_heuristics.py`: This file contains the code for all the BFLP heuristics. 
  - `BFLP_MIP.py`: This file contains the code for running the BFLP MIP.
  - `BUAP_heuristics.py`: This file contains all the BUAP heuristics and also implementations of those that are just needed for use within the BFLP heuristics; e.g., the optimization of `relaxation rounding` when used in the first version of `close greedy`.
  - `BUAP_MIP.py`: This file contains the code for running the BUAP MIP and also for editing the model, as needed for `relaxation rounding` with the first version of `close greedy`.
  - `results_heuristics.py`: This file contains functions for running all the heuristics across multiple budget factors and input parameters. For each BFLP heuristic, we provide a function for running the heuristics with multiple inputs (see the function name starting with `get_`). Results are written in JSON files; we also provide a function for converting these results into more easily readable Excel tables (see the function name starting with `write_`). All results created are saved in the `own_results` folder, which is created if it does not exist yet. For the BUAP, there is a function that runs all the heuristics on a single instance and the corresponding function which writes the results into a table.
  - `utils.py`: This file contains a few subroutines that are useful for reading and writing data, and some subroutines that are used in multiple heuristics.
  - `main.py`: This file contains a concrete example of how to run the functions in the repository. The line `results = ` may be changed to run the relevant heuristic (with the appropriate data input existing).
  - `run_instance_i.py`: This file contains the code for instance `i = 1, ..., 4` which solve model naively and greedy_heuristic to obtain optimality gap and overall access in a table.

---

## ▶️Example

We provide a short concrete example of how to run this code in the `main.py` file. To run the file, make sure you have navigated to the appropriate repository folder in your terminal. Then, run:```python3 ./heuristics_and_mips/main.py```. By default, we run the `open greedy` algorithm on Instance 1. The algorithm tests out how different values for the parameters $n_c$ and $d$ perform across different budgets. The results for the BFLP MIP for this instance are saved in the file ```instance_1_BFLP_MIP.json``` in the `own_results` folder. 

Similarly, to run the `close greedy` algorithm, or the BFLP and BUAP models, just copy-paste the corresponding get_ command as we explained above into the main file and run it. 


</details>

<details>
<summary><h2>📂Preferential access and fairness in waste management</h2></summary>

This project is based on the theory and application of quadratic optimization models within the context of undesirable facility location problems. All data and code for the associated papers are provided in this repository.

For the theoretical side, see and cite the published [article](https://pubsonline.informs.org/doi/10.1287/ijoc.2022.0308) **Quadratic optimization models for balancing preferential access and fairness: Formulations and optimality conditions** by C. Schmitt and B. Singh in the *INFORMS Journal on Computing*. Here, we propose a Mixed Integer Quadratic Programming (MIQP) Model for a Facility Location Problem that seeks to assign users to facilities in a manner that balances access for users and fairness among facilities. We refer to the article for further information on methods and assumptions.

For the motivation of this project, see and cite the published [article](https://onlinelibrary.wiley.com/doi/10.1002/net.22221) **Selectively closing recycling centers in Bavaria: Reforming policy and reducing disparities** by M. Schmidt and B. Singh in *Networks*. Here, we seek efficient ways to close recycling centers in Bavaria to meet climate neutrality goals, while still not overburdening the remaining centers and ensuring residents have good access to recycling.

---

## 🗂️Repository content

The repository contains the following content:

- `catchment_population`  
  Presents an efficient algorithm to compute the "catchment population" of each recycling center that was used to estimate the capacities.  
  Includes:
  - `catchment_population.py`: implementation of the algorithm  
  - `bavaria_grid_population.csv`: latitude, longitude, and population of each 100m x 100m grid in Bavaria  
  - `rc_locations.csv`: latitude and longitude of each recycling center  

- `data`  
  Contains the two main input data files for the MIQP model:
  - `users_and_facilities.xlsx`: ZIP codes and recycling center attributes, population, spatial type (rural/urban), capacities, and centroid coordinates  
  - `travel_dict.json.pbz2`: compressed travel probabilities from each ZIP code to each recycling center  

- `model`  
  Contains scripts for optimization and visualization:
  - `model.py`: builds and optimizes the MIQP model  
  - `greedy_heuristic.py`: greedy heuristic for feasible assignment  
  - `plotting.py` / `results.py`: visualization functions, plotting, table generation  
  - `tables_and_figures.py`: generates the exact figures/tables from the paper  
  - `utils.py`: helper functions  

- `subsequent_work`  
  Expanded versions of the model files used for additional or extended applications.

- `results`  
  Contains excel tables and figures included in the paper.

- `appendix.pdf`  
  Supplements the main text with proofs, data sources, and figures/tables.

---

## ⚙️Requirements to run code

The following open-source Python packages are required:

- **Pyomo**: Python-based optimization modeling language for building MIQP models  
- **Gurobi**: Industrial optimization solver capable of handling MIQPs  
- **Geopy**: Used for computing geodesic (earth-surface) distances  

Make sure these packages are installed and accessible before running the scripts.

</details>


