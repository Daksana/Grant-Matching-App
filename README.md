# Grant-Matching-App
AI-powered grant matching system using hybrid retrieval (semantic search + metadata filtering) and Cohere LLM for intelligent eligibility scoring.

## Features

- **Intelligent Feature Extraction**: Cohere LLM extracts structured fields from unstructured company descriptions (funding amount, business type, sector, priorities)

- **Multi-Level Fallback Logic**: 3-tier retrieval strategy handling real-world edge cases:
  1. Full metadata filters (country, region, years, employees, amount, percentage)
  2. Country-only fallback if no results
  3. Pure semantic search as final fallback
  
- **Hybrid Retrieval**: Combines INSTRUCTOR embeddings (`hkunlp/instructor-base`) with conditional metadata filtering that skips null fields automatically

- **AI-Powered Evaluation**: Generates fit scores (0-1), eligibility classifications, detailed reasoning, and risk assessments

## Getting Started

### 1. Install dependencies:
```bash
pip install sentence-transformers chromadb google-genai cohere pandas numpy openpyxl
```

### 2. Set up API key:
```python
COHERE_API_KEY = "your_key_here"
```

### 3. Prepare data files:
- `Extracted_Details.xlsx` - Structured grant details with metadata
- `sample_company.json` - Raw company descriptions and pitch decks

### 4. Run:
```python
# Execute notebook cells sequentially
# Pipeline: Ingest grants → Extract features → Retrieve → Judge & rank
```

## How It Works

1. **Ingest grants** → Create INSTRUCTOR embeddings and store in ChromaDB vector database
2. **Extract company features** → LLM extracts structured data from descriptions
3. **Hybrid retrieval** → Apply smart filtering logic:
```
IF full_filters available → Apply all
  IF no results → Try country-only
    IF no results → Fall back to semantic-only
```
4. **Judge & rank** → Cohere evaluates fit considering sector alignment, eligibility, funding adequacy, deadlines, and data completeness
5. **Output results** → Ranked matches with scores and explanations

## Example Output

```json
[
  {
    "grant_id": "G001",
    "fit_score": 0.87,
    "eligibility": "Likely eligible",
    "reasons": "Strong alignment with agriculture technology sector. Ontario-based SME with 15 employees matches eligibility criteria. Funding amount of $500k is within grant range.",
    "risks": "Deadline in 2 months. Percentage coverage (60%) requires verification against grant terms."
  }
]
```

---

*Project developed under AI Orchestration Bootcamp by Senzmate IoT Intelligence*
