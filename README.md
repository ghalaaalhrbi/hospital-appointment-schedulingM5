# Hospital Appointment Scheduling — Single-Agent Baseline (M5)

This repository contains a Jupyter notebook that implements and evaluates a rule-based Single-Agent (SA) baseline for hospital appointment scheduling. It covers two scheduling environments:

- **Experiment 1 — Easy / Static:** 20 doctors and 100 appointment requests.
- **Experiment 2 — Complex / Dynamic:** 40 doctors, 3,298 initial appointment requests, and 290 dynamic events.

The scheduler uses predefined rules and constraints. The notebook does not call an external LLM or API.

## What the notebook does

### Experiment 1 — Easy / Static

- Generates doctor, patient, appointment-request, and time-slot data.
- Assigns appointment requests to feasible slots using the Single-Agent scheduling logic.
- Validates the resulting schedule against the implemented constraints.
- Calculates assignment and patient-preference metrics.

### Experiment 2 — Complex / Dynamic

- Generates a larger scheduling scenario with doctor capacity and appointment-duration constraints.
- Builds a set of 290 dynamic events:
  - 100 patient cancellations
  - 40 doctor unavailability events
  - 120 new appointment requests
  - 30 doctor schedule changes
- Processes the events against the initial schedule.
- Validates and evaluates the final dynamic schedule.
- Reports event-processing and disruption-recovery metrics.

### M5 — Evaluation harness

The notebook also adds evaluation code for the Single-Agent baseline:

- **C5.1 — Golden Test Set:** hand-crafted scheduling cases, including decision-quality cases for doctor, date, time preferences, and tie-breaking.
- **C5.2 — Automated and Reproducible Scoring:** reuses the project's scoring functions and checks whether identical random seeds reproduce the same generated inputs, schedules, validation results, and scores.
- **C5.3 — End-to-End and Component-Level Evaluation:** checks the final dynamic schedule and selected event-handling components.
- **C5.4 — Baseline Score and Variability:** evaluates the Easy / Static scenario across three seeds and extends the analysis to the Complex / Dynamic scenario.
- **C5.5 — Trajectory-Level Metrics:** measures appointment movement and disruption effects across the dynamic runs.

## Requirements

- Python 3.10 or newer
- Jupyter Notebook or JupyterLab
- pandas
- IPython (provided with the standard Jupyter installation)

## Setup and run

1. Clone or download the repository.
2. Open a terminal in the repository folder.
3. Create and activate a virtual environment (recommended).

   **macOS / Linux**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

   **Windows (PowerShell)**
   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

4. Install the notebook dependencies:
   ```bash
   python -m pip install pandas jupyter
   ```
5. Start Jupyter:
   ```bash
   jupyter notebook
   ```
6. Open `hospital_appointment_scheduling_M5.ipynb`.
7. Select **Kernel → Restart Kernel and Run All Cells**.

Run the notebook from the first cell in order. Later cells reuse datasets, functions, and results created by earlier cells.

## Verified experiment outputs

The following values were obtained by executing the notebook's main experiment cells in order in the available Python environment:

### Experiment 1 — Easy / Static

| Metric | Observed value |
|---|---:|
| Appointment requests | 100 |
| Scheduled appointments | 100 |
| Unscheduled appointments | 0 |
| Successful assignment rate | 100.00% |
| Constraint violations | 0 |
| Preferred doctor satisfaction | 100.00% |
| Preferred date satisfaction | 98.00% |
| Preferred time satisfaction | 100.00% |
| Average date deviation | 0.02 days |
| Maximum date deviation | 1 day |

### Experiment 2 — Complex / Dynamic

**Initial schedule**

| Metric | Observed value |
|---|---:|
| Initial appointment requests | 3,298 |
| Scheduled appointments | 3,113 |
| Unscheduled appointments | 185 |
| Successful assignment rate | 94.39% |
| Constraint violations | 0 |
| Preferred doctor satisfaction | 90.43% |
| Preferred date satisfaction | 86.15% |
| Preferred time satisfaction | 98.47% |
| Average date deviation | 0.27 days |
| Maximum date deviation | 7 days |
| Scheduled minutes | 131,370 |

**After processing dynamic events**

| Metric | Observed value |
|---|---:|
| Initial appointment requests | 3,298 |
| Successful cancellations | 100 |
| New appointment requests | 120 |
| Final active requests | 3,318 |
| Scheduled appointments | 3,107 |
| Unscheduled active requests | 211 |
| Successful assignment rate | 93.64% |
| New-request scheduling rate | 78.33% |
| Constraint violations | 0 |
| Preferred doctor satisfaction | 89.24% |
| Preferred date satisfaction | 84.55% |
| Preferred time satisfaction | 97.98% |
| Average date deviation | 0.31 days |
| Maximum date deviation | 7 days |
| Scheduled minutes | 131,130 |

**Dynamic event processing**

| Metric | Observed value |
|---|---:|
| Total dynamic events | 290 |
| Successfully processed events | 264 |
| Failed events | 26 |
| Actionable event success rate | 91.03% |
| Appointment instances affected | 270 |
| Appointments affected by disruptions | 170 |
| Appointments recovered from disruptions | 170 |
| Disruption recovery rate | 100.00% |

### Evaluation checks executed

- Golden test set: **24/24 passed**.
- Easy / Static same-seed reproducibility check (seed 42): **PASS**.



## Exported experiment data

The notebook contains export cells that write generated experiment inputs and baseline outputs to:

- `shared_experiment_inputs/experiment_1_easy_static/`
- `shared_experiment_inputs/experiment_2_complex_dynamic/`

These outputs are generated by running the relevant notebook cells.

## Repository contents

- `hospital_appointment_scheduling_M5.ipynb` — notebook containing both experiments and the M5 evaluation harness.
- `README.md` — project overview, setup instructions, experiment description, and verified outputs.
