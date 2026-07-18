# Multiscale fatigue model for high-Ni 316L stainless steel at cryogenic temperatures

## Overview

This repository contains the source code and input files for the multiscale fatigue analyses of high-Ni 316L stainless steel at 293, 193, and 77 K.

The model predicts fatigue life by linking:

1. macroscopic finite element analysis,
2. statistical microstructure generation, and
3. microstructure-sensitive fatigue-crack-growth analysis.

Two controlled simulation cases are included:

- **Case 1:** temperature-dependent mechanical properties with the as-received microstructure at all temperatures.
- **Case 2:** the same temperature-dependent mechanical properties with microstructural descriptors measured after fatigue testing at each temperature.

Each case contains separate calculation folders for **77 K**, **193 K**, and **293 K**.

## Repository structure

```text
.
├── Case-1
│   ├── 77k
│   ├── 193k
│   └── 293k
├── Case-2
│   ├── 77k
│   ├── 193k
│   └── 293k
└── README.md
```

## Requirements

- **Abaqus:** required to generate the `.dat` file from the supplied `.inp` file.
- **Python 3.10:** required to run the multiscale fatigue simulation.
- Required third-party Python libraries:

```bash
pip install numpy pandas scipy
```

The other imported modules are included in the Python standard library.

## Input files

Each calculation folder contains the following files:

- `MultiscaleModel.py`: main program for the multiscale fatigue simulation.
- `H_SmoothT_T40.inp`: Abaqus finite element model and analysis conditions.
- `H_SmoothT_T40.zip`: compressed Abaqus `.dat` file required by the Python program.
- `H_Austenitic grain size.csv`: equivalent grain-size distribution.
- `H_Austenitic grain aspect ratio.csv`: grain aspect-ratio distribution.
- `H_Austenitic grain angle.csv`: grain major-axis orientation distribution.
- `H_Friction strength.csv`: friction strength for dislocation motion.
- `H_Monotonic tensile test.csv`: yield strength, tensile strength, and reduction in area.
- `CombinedData_Ellipse_WF_CF.json`: weight-function and correction-factor data for semi-elliptical cracks.

## Structure of the code

The main components of `MultiscaleModel.py` are:

1. **Basic parameter definition** — `BasicParameters`
2. **Abaqus data import** — `AbaqusDatabaseCreator`
3. **Material and microstructural data import** — `MaterialDataImporter`
4. **Field-value interpolation** — `FieldValuesFunction`
5. **Life evaluation for a single crack** — `CrackLifeCalc`
6. **Life evaluation for an area element** — `ElementLife`
7. **Execution of the fatigue-life simulations**

## How to execute

1. Open one of the six calculation folders.
2. Extract `H_SmoothT_T40.zip` in the same folder. The extracted file must retain the name:

```text
H_SmoothT_T40.dat
```

3. Alternatively, import `H_SmoothT_T40.inp` into Abaqus and run the analysis to generate the corresponding `.dat` file.
4. Keep all input files and `MultiscaleModel.py` in the same folder.
5. Modify the calculation settings in the `BasicParameters` class when necessary, particularly the applied stress amplitudes and number of iterations.
6. Run:

```bash
python MultiscaleModel.py
```
## Note

The file names and folder structure should not be changed because the program identifies the required input files by their predefined names.
