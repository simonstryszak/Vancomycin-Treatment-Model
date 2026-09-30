# Vancomycin Treatment Model

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonstryszak/Vancomycin-Treatment-Model/blob/main/Vancomycin_Treatment_Model.ipynb)

A pharmacokinetic-pharmacodynamic (PK-PD) model of a vancomycin treatment course for methicillin-resistant *Staphylococcus aureus* (MRSA). The model links vancomycin distribution and renal elimination to bacterial growth, infection spread, adaptive resistance, nephrotoxicity, and infusion-related histamine release. It compares how loading-only, continuous-infusion, and intermittent-dosing strategies affect drug exposure and bacterial burden over five days.

Built in Google Colab. The complete model and its exploratory analyses are contained in `Vancomycin_Treatment_Model.ipynb`.

## Overview

The model represents vancomycin concentrations in blood and four peripheral compartments: skin/wound, kidney, lung, and the rest of the body. Blood flow, tissue partition coefficients, compartment volumes, and renal clearance determine how the drug moves through and leaves the system. Clearance changes with glomerular filtration rate (GFR), while high simulated kidney concentrations can reduce GFR and feed back into subsequent drug elimination.

MRSA populations are tracked at the wound, in the bloodstream, and in the lungs. Bacteria grow logistically, spread from the wound to blood and from blood to lung, and are killed by local vancomycin exposure through an Emax/Hill response. A reversible adaptive-resistance state raises the effective EC50, allowing resistant and non-resistant simulations to be compared.

The coupled system is expressed as ordinary differential equations and integrated with SciPy's `odeint` over a 120-hour treatment period.

## What the model includes

### Vancomycin pharmacokinetics

- Central blood compartment plus skin/wound, kidney, lung, and rest-of-body compartments.
- Tissue-specific volumes, blood flows, and partition coefficients.
- GFR-dependent renal clearance.
- Loading-only, continuous-infusion, and intermittent-dosing regimens.
- Concentration-time plots by tissue, including a displayed target-trough range.

### MRSA pharmacodynamics

- Logistic bacterial growth at the wound, in blood, and in lung tissue.
- Dissemination from wound to blood and blood to lung.
- Concentration-dependent bacterial killing using an Emax model.
- Reversible adaptive resistance that shifts the effective EC50.
- Comparisons of bacterial burden with and without adaptive resistance.

### Safety-related responses

- A concentration-driven nephrotoxicity signal and dynamic GFR response.
- Exploratory comparisons of GFR under each dosing regimen.
- Infusion-rate-dependent histamine release with fast and slow response components.
- Comparison of standard and more conservative maintenance doses.

## Key state variables

| Variable | Meaning |
| --- | --- |
| `Cb`, `Cs`, `Ck`, `Cl`, `Crob` | Vancomycin concentration in blood, skin/wound, kidney, lung, and the rest of the body |
| `P_wound`, `P_blood`, `P_lung` | MRSA burden at each infection site |
| `AR_on`, `AR_off` | Local adaptive-resistance states |
| `GFR` | Dynamic glomerular filtration rate |
| `H_slow`, `H_fast` | Slow and fast histamine-response components |

## Requirements

The notebook uses:

```text
numpy
scipy
matplotlib
```

Install the dependencies locally with:

```bash
pip install numpy scipy matplotlib
```

## Running the model

Open `Vancomycin_Treatment_Model.ipynb` in Jupyter or use the **Open in Colab** button above, then run the cells from top to bottom. The notebook defines the dosing functions, coupled PK-PD equations, parameter sets, numerical simulations, and plots.

The later cells contain exploratory component models for untreated MRSA growth, simplified vancomycin pharmacokinetics, GFR response, and histamine release.

## Parameters

The main parameter dictionary can be edited to explore different assumptions. Parameters include compartment volumes, tissue blood flows and partition coefficients, reference and baseline GFR, nephrotoxicity thresholds, bacterial growth and carrying capacities, vancomycin Emax and EC50, Hill coefficient, resistance switching rates, and infection dissemination rates.

Dose amounts, infusion rates, and dosing intervals are defined in the dosing functions near the beginning of the notebook.

## Notes and limitations

- This is a deterministic, well-mixed compartment model; it does not represent spatial variation or variability between individual patients.
- Several physiological, bacterial, resistance, nephrotoxicity, and histamine parameters are conceptual or literature-informed estimates rather than values fitted to a clinical dataset.
- Some later notebook cells are exploratory alternatives to the combined model and use different assumptions or parameter values.
- The displayed concentration targets and kidney-disease bands are contextual visual references, not dosing recommendations.
- Results are computational predictions intended for education and model exploration. The project is not a validated clinical decision-support tool and must not be used to determine patient treatment.

## Author

Simon Stryszak
