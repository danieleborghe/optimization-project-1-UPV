# Allocating Firefighting Vehicles in New York City: A Linear Optimization Model

This repository contains the code, data, and findings for a project focused on optimizing the allocation of fire-fighting resources across the neighborhoods of New York City. The goal is to strategically deploy specialized fire-fighting vehicles to maximize serviceability while adhering to budgetary and operational constraints.

The project was developed as part of the Bachelor's Degree in Data Science at the **Universitat Politècnica de València**.

[![Read the Report](https://img.shields.io/badge/Read_the_Full-Report-blue?style=for-the-badge&logo=adobeacrobatreader)](optimization_project_1_team_2_report.pdf)

---

## 📝 Table of Contents

- [The Problem: Optimizing Emergency Response in NYC](#-the-problem-optimizing-emergency-response-in-nyc)
- [Our Solution: A Two-Stage Mixed-Integer Linear Programming Model](#-our-solution-a-two-stage-mixed-integer-linear-programming-model)
- [Technical Stack & Methodologies](#-technical-stack--methodologies)
- [Dataset Overview](#-dataset-overview)
- [Experimental Scenarios & Key Findings](#-experimental-scenarios--key-findings)
- [Repository Structure](#-repository-structure)
- [How to Run the Model](#-how-to-run-the-model)
- [Authors](#-authors)

---

## 🎯 The Problem: Optimizing Emergency Response in NYC

The Fire Department of New York (FDNY) operates a complex network of fire stations and specialized vehicles to serve millions of citizens. Ensuring that the right resources are in the right place is critical for minimizing response times and maximizing public safety.

This project addresses the challenge of optimally deploying five types of specialized fire-fighting vehicles: **Ladders, Rescues, Squads, Hazardous Materials Units, and Engines**. The primary objective is to determine the best placement of these vehicles across NYC's fire stations to achieve the highest possible serviceability for the city's neighborhoods, considering factors like population density, environmental risks, and distance.

---

## 💡 Our Solution: A Two-Stage Mixed-Integer Linear Programming Model

To solve this complex allocation problem, we formulated and implemented a **two-stage Mixed-Integer Linear Programming (MILP)** model. This approach allows us to find the mathematically optimal deployment strategy under a given set of constraints.

### The Two-Stage Model:

1.  **Stage 1: Vehicle Assignment to Fire Stations**
    The first model determines which specialized vehicles should be assigned to which fire stations. The objective is to **maximize the overall serviceability score** across all neighborhoods. Serviceability is a composite metric we developed that reflects how well a neighborhood is covered by the necessary emergency resources. This stage is subject to a **global budget constraint** on the total number of vehicles of each type that can be deployed.

2.  **Stage 2: Neighborhood-to-Station Assignment**
    Taking the vehicle deployments from Stage 1 as a fixed input, the second model assigns each neighborhood to the **closest fire station** that can provide the required services. The objective here is to **minimize the total weighted distance** between neighborhoods and their assigned stations, ensuring that every neighborhood is covered. The weights are based on a "need" index, giving priority to more vulnerable or high-demand areas.

---

## 💻 Technical Stack & Methodologies

- **Language**: **Python 3.x**
- **Core Libraries**:
  - **Pandas** & **NumPy**: For data manipulation, cleaning, and preprocessing.
  - **scikit-learn**: Used for standardizing numerical data (e.g., `StandardScaler`) to create composite indices for our model.
  - **ortools (Google AI)**: The core library for modeling and solving the linear optimization problems.
  - **Matplotlib** & **Seaborn**: For data visualization, analysis, and plotting results.
  - **Jupyter Notebook**: For interactive development, data exploration, and model implementation.
- **Optimization Solvers**:
  - **SCIP (Solving Constraint Integer Programs)**: A high-performance, non-commercial solver for MILP, used as our primary tool.
  - **GLOP (Google Linear Optimization Platform)**: Used for comparison and benchmarking.
- **Data Processing**:
  - The project involved significant feature engineering to create a "need" index for each neighborhood based on demographic, economic, and environmental factors.
  - A serviceability score was calculated for each neighborhood based on the types of vehicles available at nearby stations.

---

## 📊 Dataset Overview

The dataset used in this project was compiled from various public sources and is located in the `/dataset` directory. Key files include:

-   `neighborhoods_info_standardized.csv`: Contains processed and standardized data for each NYC neighborhood, including population, income, crime rates, and the calculated "need" index.
-   `distance_stations_neighborhoods.csv`: A matrix containing the travel distances between every fire station and every neighborhood.
-   `/serviceability/*.csv`: A set of files defining which vehicle types can service which neighborhoods.
-   `/results/*.csv`: Contains the output deployment plans from our model for different experimental scenarios.

---

## 🔬 Experimental Scenarios & Key Findings

We tested our model under four different strategic scenarios, each reflecting a different set of priorities for resource allocation:

1.  **Population-Centric (40% Pop, 10% Env, 50% Dist)**: Prioritizes densely populated areas.
2.  **Environment-Centric (20% Pop, 60% Env, 20% Dist)**: Focuses on areas with higher environmental risks.
3.  **Balanced (35% Pop, 30% Env, 35% Dist)**: A balanced approach between all factors.
4.  **Distance-Focused (25% Pop, 25% Env, 50% Dist)**: Aims to minimize travel distances above all.

### Key Findings:

-   The MILP model successfully generated optimal deployment strategies for all scenarios, providing concrete, data-driven recommendations.
-   The **SCIP solver consistently outperformed GLOP**, finding optimal solutions more quickly and reliably for our problem structure.
-   The scenario analysis revealed significant trade-offs: a population-centric approach, for instance, improved coverage in dense urban cores but could leave areas with high environmental risk (like industrial zones) less protected.
-   The two-stage model proved to be an effective strategy, breaking down a highly complex problem into two manageable, interconnected optimization tasks.

---

## 📂 Repository Structure

```

.
├── dataset/
│   ├── distance\_stations\_neighborhoods.csv  \# Distance matrix
│   ├── neighborhoods\_info\_standardized.csv  \# Processed neighborhood data
│   ├── serviceability/                      \# Serviceability definitions
│   └── results/                           \# Model output files
├── data\_exploration.ipynb               \# EDA and visualization of the raw data
├── neighborhoods\_info.ipynb             \# Notebook for calculating the "need" index
├── neighborhoods\_serviceability.ipynb   \# Notebook for calculating serviceability scores
├── model\_implementation.ipynb           \# Core notebook with the MILP model implementation
├── experimentation.ipynb                \# Notebook for running the scenarios and analyzing results
├── optimization\_project\_1\_team\_2\_report.pdf \# The final, detailed project report
└── README.md                            \# This file

````

---

## 🚀 How to Run the Model

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/danieleborghe/optimization-project-1-UPV.git](https://github.com/danieleborghe/optimization-project-1-UPV.git)
    cd optimization-project-1-UPV
    ```

2.  **Set up a virtual environment and install dependencies:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    pip install pandas numpy scikit-learn ortools matplotlib seaborn jupyter
    ```

3.  **Launch Jupyter Notebook:**
    ```bash
    jupyter notebook
    ```

4.  **Run the notebooks in sequence:**
    -   Start with `data_exploration.ipynb`, `neighborhoods_info.ipynb`, and `neighborhoods_serviceability.ipynb` to understand and preprocess the data.
    -   Open `model_implementation.ipynb` to see the core MILP model definition and run it. You can adjust model parameters and scenarios directly within this notebook.
    -   Use `experimentation.ipynb` to reproduce the scenario analysis and view the final results.

---

## 👥 Authors

- **Eva Cantín Larumbe**
- **Eva Teisberg**
- **Mikel Baraza Vidal**
- **Francesco Pio Capoccello**
- **Daniele Borghesi**
