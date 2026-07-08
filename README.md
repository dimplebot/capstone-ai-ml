**Capstone Project**

This repository contains the complete implementation of a four-part capstone project. The project covers data preprocessing, exploratory data analysis, predictive modeling, model evaluation, hyperparameter tuning, and the integration of a Large Language Model (LLM) for generating structured prediction explanations.

**REPOSITORY STRUCTURE:**
```text
capstone-ai-ml/
│
├── Part1/
│   ├── part1.ipynb
│   ├── README.md
│   ├── cleaned_data.csv
│   └── visual_output/
│
├── Part2/
│   ├── part2.ipynb
│   ├── README.md
|   └── visual_output/
│
├── Part3/
│   ├── part3.ipynb
│   ├── README.md
│   └── best_model.pkl
│
├── Part4/
│   ├── part4.ipynb
│   └── README.md
│
├── .gitignore
└── README.md
```

**Project Summary:**
Part 1 - Data Acquisition, Cleaning, and Exploratory Analysis
Part 2 - Supervised Machine Learning Model — Build, Train, and Evaluate
Part 3 - Advanced Modeling — Ensembles, Tuning, and Full ML Pipeline
Part 4 - LLM-Powered Feature: Structured Extraction, Tabular Batch Scoring, or Model Prediction Explanation

**Environment Variables:** Part 4 requires an OpenRouter API key. Create a `.env` file in the project root.
```text
LLM_API_KEY=your_openrouter_api_key
```
The `.env` file is intentionally excluded from this repository for security reasons.

**Installation:**
```bash
pip install pandas numpy matplotlib seaborn scikit-learn joblib requests jsonschema python-dotenv
```

Each project part has it own README.md.
The saved model is also included.
The plots generated in part 1 and part 2 tasks are available in visual output folder in respective part folder



