# COVID-19 Prediction using Machine Learning

A machine learning-aided global diagnostic and comparative tool to assess the effect of quarantine control in COVID-19 spread. This project implements a neural network-augmented epidemiological model (QSIR) to predict COVID-19 infection dynamics and analyze quarantine effectiveness across multiple countries and US states.

## 📊 Overview

This repository contains the complete implementation of a hybrid physics-informed neural network approach that combines traditional SEIR epidemiological models with deep learning to:

- **Predict COVID-19 spread** across different geographical regions
- **Quantify quarantine effectiveness** using learned quarantine strength parameters Q(t)
- **Calculate effective reproduction number** (Rₑ) and identify transition points
- **Compare intervention strategies** across countries and states

The model uses a modified QSIR (Susceptible-Infected-Recovered-Quarantined) differential equation system where a neural network learns the time-varying quarantine parameter Q(t) from real-world data.

## 🔬 Methodology

### QSIR Model

The model extends the classical SIR model with a quarantine compartment:

- **S**: Susceptible population
- **I**: Infected population  
- **R**: Recovered population (including deaths)
- **Q**: Quarantined population

A neural network learns the quarantine function Q(t) that represents intervention strength, allowing the model to capture complex, non-linear policy effects that traditional compartmental models cannot represent.

### Neural Network Architecture

- Input layer: 3 nodes (S, I, R compartments)
- Hidden layer: 10 nodes with ReLU activation
- Output layer: 1 node (quarantine parameter)
- Optimized using ADAM optimizer with adjoint sensitivity methods

### Key Metrics

- **Effective reproduction number (Rₑ)**: Measures disease spread potential
- **Transition point**: When Rₑ crosses below 1 (disease containment)
- **Quarantine strength Q(t)**: Time-varying intervention effectiveness

## 📁 Repository Structure

```
Covid-19-Prediction-main/
├── Codes/                      # Julia implementation files
│   ├── USA/                    # US state-specific models
│   │   ├── Qmodel_Texas.jl
│   │   ├── Qmodel_NewYork.jl
│   │   ├── Qmodel_California.jl
│   │   └── ... (19 states)
│   ├── Asia/                   # Asian country models
│   │   ├── Qmodel_China.jl
│   │   ├── Qmodel_India.jl
│   │   └── Qmodel_Korea.jl
│   ├── Europe/                 # European country models
│   │   ├── Qmodel_UK.jl
│   │   ├── Spain_Qfinal_Check.jl
│   │   └── ... (5 countries)
│   └── South America/          # South American country models
│       ├── Brazil, Chile, Peru
│       └── ...
├── Data/                       # COVID-19 time series data (.mat files)
│   ├── USA/                    # State-level data
│   ├── Asia/
│   ├── Europe/
│   └── South America/
├── Plots/                      # Generated visualizations
│   └── PDF plots for each region
├── Schematics/                 # Model architecture diagrams
│   ├── NN_archn.PNG           # Neural network architecture
│   ├── NNschemn.PNG           # Full system schematic
│   └── schemn.PNG             # QSIR model diagram
└── README.md                   # This file
```

## 🚀 Getting Started

### Prerequisites

- **Julia** (v1.5 or higher)
- Required Julia packages:
  ```julia
  using Pkg
  Pkg.add("MAT")
  Pkg.add("Plots")
  Pkg.add("Flux")
  Pkg.add("DifferentialEquations")
  Pkg.add("DiffEqFlux")
  Pkg.add("LaTeXStrings")
  Pkg.add("StatsPlots")
  Pkg.add("Measures")
  Pkg.add("JLD")
  ```

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/Covid-19-Prediction-main.git
   cd Covid-19-Prediction-main
   ```

2. Install Julia dependencies (see Prerequisites)

3. Update data file paths in the code to match your local directory structure

### Usage

#### Running a Regional Model

1. Navigate to the appropriate code file (e.g., `Codes/USA/Qmodel_Texas.jl`)

2. Update the data file path on line 15:
   ```julia
   vars = matread("path/to/your/Data/USA/Rise_Texas_Trackn.mat")
   ```

3. Run the model in Julia REPL:
   ```julia
   include("Codes/USA/Qmodel_Texas.jl")
   ```

4. The script will:
   - Train the neural network (60,000 iterations for USA states, 30,000 for others)
   - Generate predictions for I(t), R(t), and Q(t)
   - Calculate Rₑ and identify transition points
   - Save results to `.jld` files
   - Generate three visualization plots (PDF format)

#### Output Files

For each region, the model generates:

1. **Data vs Prediction Plot**: Comparison of model predictions with actual infection and recovery data
2. **Effective Reproduction Number (Rₑ) Plot**: Shows disease spread parameter over time with transition point
3. **Quarantine Strength Q(t) Plot**: Time-varying quarantine effectiveness

All plots are saved as PDF files in the working directory.

## 📈 Key Results

The model successfully:

- Quantifies quarantine effectiveness across 30+ regions globally
- Identifies when interventions became effective (Rₑ < 1)
- Provides comparative analysis of different policy approaches
- Achieves high accuracy in predicting infection dynamics

### Example Findings

- **Early intervention**: Regions with rapid Q(t) increase showed faster Rₑ decline
- **Transition timing**: Varied significantly across regions (8-40+ days post-500 infections)
- **Quarantine strength**: Ranged from 0.1 to 0.8 depending on policy stringency

## 🔧 Customization

### Training Parameters

Modify training iterations in the code:
```julia
datan = Iterators.repeated((), 60000)  # Number of training iterations
opt = ADAM(0.01)                        # Learning rate
```

### Initial Conditions

Adjust population and initial infection values:
```julia
u0 = Float64[population, initial_infected, initial_recovered, initial_quarantined]
```

### Model Parameters

Initial parameter guesses:
```julia
p2 = Float64[β, γ, δ]  # Transmission, recovery, quarantine exit rates
```

## 📊 Data Format

Input data files (`.mat` format) should contain:
- `Region_Infected_All`: Total confirmed cases
- `Region_Recovered_All`: Total recovered cases  
- `Region_Dead_All`: Total deaths
- `Region_Time`: Time series indices

## 🎓 Citation

If you use this work, please cite:

```bibtex
@article{covid19prediction,
  title={A machine learning aided global diagnostic and comparative tool to assess effect of quarantine control in Covid-19 spread},
  author={[Your Name/Team]},
  journal={[Journal Name]},
  year={2020/2021}
}
```

## 📝 Publication

This code accompanies the manuscript: **"A machine learning aided global diagnostic and comparative tool to assess effect of quarantine control in Covid-19 spread"**

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request



## 🙏 Acknowledgments

- COVID-19 data from Johns Hopkins University CSSE
- Julia community for excellent differential equations and machine learning packages
- All healthcare workers and researchers fighting the pandemic

## ⚠️ Disclaimer

This model is for research and educational purposes only. It should not be used as the sole basis for public health decisions. Always consult with epidemiologists and public health experts for policy-making.

---

**Last Updated**: December 2024  
**Maintainer**: [Rishabh Sharma]
