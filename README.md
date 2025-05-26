
# Renewable Energy Community Simulation-Optimization Pipeline

This package provides an integrated pipeline for modeling, simulating, optimizing, and evaluating hybrid energy systems in Renewable Energy Communities (RECs). It automates the workflow from scenario generation to Simulink simulation, power dispatch optimization using Pyomo, and KPI evaluation.

---

## 📦 Repository Structure

```
project-root/
├── HES-REC-RC__Info/           # Running instruction and requiremet.txt
├── HES-REC-RC_config/          # Study definitions, system configuration, and parameter files
├── HES-REC-RC_PipelineOrchestrator/       # Main pipeline execution script
├── HES-REC-RC_DispatchOptimisation/      # Pyomo-based optimization module
├── HES-REC-RC_KPI_Evaluation/            # Post-processing and KPI computation
├── HES-REC-RC_log_data/                  # Output logs and results (auto-generated)
├── HES-REC-RC_IntegrationModel/          # Simulink model and scenario executables
├── HES-REC-RC_requirements.txt           # Python package dependencies
```

---

## 🔧 Prerequisites

- **Python 3.12.0**
- **MATLAB R2024b**
  - Required Add-on: Simscape Electrical
- **CBC Solver** (for Mixed-Integer Linear Programming with Pyomo)  
  - Download from: [https://github.com/coin-or/Cbc/releases](https://github.com/coin-or/Cbc/releases)  
  - Recommended file: `Cbc-releases.2.10.12-w64-msvc17-md.zip` (for modern Windows systems)  
  - Extract it (e.g., to `C:\cbc`)  
  - Add the `C:\cbc\bin` directory to your system `Path` environment variable  
  - Open a new terminal and verify installation with:
    ```bash
    cbc --version
    ```

- **Python packages**  
  Install required packages using:
  ```bash
  pip install -r requirements.txt

---

## 🚀 How to Run the Pipeline

1. **Configure your study**  
   Edit `config/Study_difinition_Soria.yaml` or `Study_difinition_Portici.yaml`.
   Configuration default for optimisation module also can be edited through `Config_Opt.xlsx`
   Configuration default for simulink module also can be edited through `config_simulation_Portigi.xlsx` or `config_simulation_Soria.xlsx`

2. **Select the study in the dispatcher**  
   Open `PipelineOrchestrator/PipelineDispatcher.py` and toggle between:
   ```python
   # dispatcher = PipelineDispatcher(study_file_Nm="Study_difinition_Portici")
   dispatcher = PipelineDispatcher(study_file_Nm="Study_difinition_Soria")
   ```

3. **Run the pipeline**
   ```
   python HES-REC-RC_PipelineOrchestrator/PipelineDispatcher.py
   ```
---

## 📊 Output Structure

Each pipeline run creates a timestamped folder inside `log_data/`:

- `Execution_definition.json`: Overview of the study and scenarios
- `scenario_run_XX_YY.yaml`: Input for Simulink
- `config_scenario_run_XX_YY.yaml`: Input and output for optimization
- `scenario_run_XX_YY_KPI.csv/json`: Simulation results
- `KPI_outputs_scenario_run_XX_YY.xlsx/json`: KPI results
- `KPI_summary.xlsx`: Aggregated summary of all KPI results

---

## 📈 KPI Definitions

| Abbreviation   | Description                                                  | Unit       |
|----------------|--------------------------------------------------------------|------------|
| FF             | Flexibility Factor                                           | –          |
| FF_base        | Baseline Flexibility Factor                                  | –          |
| FF_W           | Weighted Flexibility Factor                                  | –          |
| FF_SB          | Flexibility Shift Benefit                                    | EUR        |
| FF_shift       | Relative Flexibility Shift                                   | –          |
| Eff_el         | Electrical Efficiency                                        | –          |
| Eff_th         | Thermal Efficiency                                           | –          |
| Eff            | Overall System Efficiency                                    | –          |
| LCOE           | Levelized Cost of Energy                                     | EUR/kWh    |
| Capex          | Capital Expenditure                                          | EUR        |
| Annual_Opex    | Annual Operational Expenditure                               | EUR/year   |
| Opex_per_kWh   | Operational Cost per Unit Energy Delivered                   | EUR/kWh    |
| Co2_emission   | CO₂ Emission reduction vs. no-renewable baseline             | kgCO₂      |

---

## 📎 Notes

- This package is modular: scenario generation, simulation, optimization, and KPI evaluation.
- MATLAB must be accessible via command line for simulation to run correctly.
- The consistency between Simulink and optimization configuration modules are implemented using translation dictionaries in the `HES-REC-RC_config/` directory.
- For more information, refer to the README files in each directory