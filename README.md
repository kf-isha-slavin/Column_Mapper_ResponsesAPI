# Column Mapper Responses API

This repository contains a system for automatically mapping column names from competitor datasets to Korn Ferry's standardized column architecture using OpenAI's GPT-5 reasoning model with the Responses API.

## Overview

The system performs semantic column mapping by:
1. Processing competitor data with 198 columns
2. Using OpenAI's GPT-5 model with file search capabilities to map columns to Korn Ferry's 44-column template
3. Providing top-3 predictions for each input column
4. Evaluating accuracy against ground truth mappings

## Repository Structure

```
├── Code/
│   ├── 1_setup_vector_store.ipynb    # Vector store setup and KF template processing
│   ├── 2_responses_API.ipynb         # Main API implementation and column mapping
│   └── 3_calculate_accuracy.ipynb    # Accuracy evaluation and performance metrics
├── Files/
│   ├── Competitor/                   # Competitor dataset (198 columns)
│   ├── Ground_Truth/                 # Ground truth mappings for evaluation
│   └── KF/
│       ├── Vector_Store/             # Processed Korn Ferry template files
│       └── ORGDC_BA_General_en_IDC 1.xlsx  # Original KF template
└── Responses/
    ├── Clean/                        # Cleaned response data
    ├── DF_of_GPT5_Response_v1.csv   # Structured mapping results
    └── GPT5_Response_v1.txt         # Raw API response
```

## Key Components

### 1. Vector Store Setup (`1_setup_vector_store.ipynb`)
- Extracts and processes the Korn Ferry template from Excel
- Creates a structured dataset with 44 columns including:
  - Column titles
  - Descriptions
  - Status (Required/Optional/If Applicable)
- Saves processed data as CSV and text files for vector store

### 2. Column Mapping API (`2_responses_API.ipynb`)
- Implements OpenAI's Responses API with GPT-5 model
- Uses file search tool with vector store for context
- Maps 198 competitor columns to 3 most relevant KF columns each
- Generates structured JSON output with predictions

### 3. Accuracy Evaluation (`3_calculate_accuracy.ipynb`)
- Compares predictions against ground truth data
- Calculates top-1, top-2, and top-3 accuracy metrics
- Results show:
  - Top-1 Accuracy: 80.56%
  - Top-2 Accuracy: 91.67%
  - Top-3 Accuracy: 94.44%

## Data Files

### Korn Ferry Template
The system uses a 44-column template covering:
- Employee identification (ID, Job Title, Department)
- Compensation (Basic Payments, Variable Payments, Benefits)
- Job classification (Function Codes, Reference Levels)
- Personal information (Gender, Location, Hire Date)
- Long-term incentives and allowances

### Competitor Dataset
Contains 198 columns from a competitor's compensation survey submission, including various naming conventions and data structures that need to be mapped to the KF standard.

## API Configuration

The system uses OpenAI's Responses API with:
- Model: GPT-5
- Tool: File search with vector store
- Vector Store ID: `vs_69025034e2fc81918fdcad91301db0cf`
- Output format: Structured JSON with 3 predictions per column

## Performance Results

Based on evaluation against ground truth data (72 matched columns):
- **Top-1 Accuracy**: 80.56% - The first prediction matches the ground truth
- **Top-2 Accuracy**: 91.67% - The correct answer is in the top 2 predictions
- **Top-3 Accuracy**: 94.44% - The correct answer is in the top 3 predictions

## Usage

1. Run `1_setup_vector_store.ipynb` to process the KF template
2. Execute `2_responses_API.ipynb` to perform column mapping
3. Use `3_calculate_accuracy.ipynb` to evaluate results

## Dependencies

- pandas
- numpy
- openai
- openpyxl
- tiktoken
- tqdm

## Notes

- The system requires OpenAI API credentials and project configuration
- Vector store must be set up with the processed KF template
- Ground truth data is used for evaluation but not required for inference
- All column names are normalized (lowercase, whitespace stripped) for comparison
