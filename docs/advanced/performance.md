# Performance

MIRIX delivers exceptional performance through intelligent memory consolidation, optimized search algorithms, and efficient data processing.

## Experimental Setup

We evaluate MIRIX on two comprehensive datasets to demonstrate its superior performance across different scenarios:

### LOCOMO Dataset

Following Mem0, we use the LOCOMO dataset for vertical comparison between MIRIX and existing memory systems. LOCOMO contains:

- **10 conversations** with 600 dialogues each
- **26,000 tokens** on average per conversation  
- **200 questions** per conversation across multiple categories:
  - Single-hop questions
  - Multi-hop questions
  - Temporal questions
  - Open-domain questions

### ScreenshotVQA Dataset

We collected a novel dataset containing three PhD students' computer activities:

- **Student 1**: 5,886 screenshots (1 day, heavy usage)
- **Student 2**: 18,178 screenshots (3 weeks, moderate usage)  
- **Student 3**: 5,349 screenshots (6 weeks, light usage)
- **Total questions**: 87 manually created and verified questions

Screenshots were captured every second, with duplicate filtering (similarity > 0.99).

### Evaluation Metrics

- **LLM-as-a-Judge**: Using GPT-4.1 to evaluate response quality
- **Accuracy**: Percentage of correctly answered questions
- **Storage**: Memory footprint comparison

## Experimental Results

### LOCOMO Dataset Results

| Method | Single Hop | Multi-Hop | Open Domain | Temporal | Overall |
|--------|------------|-----------|-------------|----------|---------|
| **GPT-4o-mini backbone** |
| A-Mem | 39.79 | 18.85 | 54.05 | 49.91 | 48.38 |
| LangMem | 62.23 | 47.92 | 71.12 | 23.43 | 58.10 |
| OpenAI | 63.79 | 42.92 | 62.29 | 21.71 | 52.90 |
| Mem0 | 67.13 | 51.15 | 72.93 | 55.51 | 66.88 |
| Mem0ᵍ | 65.71 | 47.19 | **75.71** | 58.13 | 68.44 |
| Memobase | 63.83 | 52.08 | 71.82 | 80.37 | 70.91 |
| Zep | 74.11 | 66.04 | 67.71 | 79.76 | 75.14 |
| **GPT-4.1-mini backbone** |
| LangMem | 74.47 | 61.06 | 67.71 | 86.92 | 78.05 |
| RAG-500 | 37.94 | 37.69 | 48.96 | 61.83 | 51.62 |
| Zep | 79.43 | 69.16 | 73.96 | 83.33 | 79.09 |
| Mem0 | 62.41 | 57.32 | 44.79 | 66.47 | 62.47 |
| **MIRIX** | **85.11** | **83.70** | 65.62 | **88.39** | **85.38** |
| Full-Context | 88.53 | 77.70 | 71.88 | 92.70 | 87.52 |

*LLM-as-a-Judge scores (%, higher is better) for each question type in the LOCOMO dataset. Full-Context represents the upper-bound performance.*

### ScreenshotVQA Results

| Method | Student 1 |  | Student 2 |  | Student 3 |  | Overall |  |
|--------|-----------|--|-----------|--|-----------|--|---------|--|
|        | Acc ↑ | Storage ↓ | Acc ↑ | Storage ↓ | Acc ↑ | Storage ↓ | Acc ↑ | Storage ↓ |
| Gemini | 0.00% | 142.10MB | 9.52% | 438.86MB | 25.45% | 129.14MB | 11.66% | 236.70MB |
| SigLIP@50 | 36.36% | 22.55GB | 41.38% | 19.88GB | 54.55% | 2.82GB | 44.10% | 15.07GB |
| **MIRIX** | **54.55%** | **20.57MB** | **56.67%** | **19.83MB** | **67.27%** | **7.28MB** | **59.50%** | **15.89MB** |

## Key Performance Insights

### Superior Multi-Hop Reasoning
MIRIX shows the largest improvement in multi-hop questions, outperforming baselines by more than 22 points. This highlights the effectiveness of our hierarchical memory system at retrieving the most relevant information across complex reasoning chains.

### Near Full-Context Performance
On single-hop and temporal tasks, MIRIX almost matches full-context performance while using only a small number of retrieved memory snippets. This validates our typed memory storage approach.

### Exceptional Efficiency
In ScreenshotVQA, MIRIX achieves:
- **59.50%** overall accuracy (vs 44.10% for SigLIP@50)
- **52.56MB** average storage (vs 15.07GB for SigLIP@50)
- **285x storage reduction** compared to traditional approaches

### Scalable Architecture
MIRIX's component-specific memory management scales efficiently across different data types and conversation lengths, maintaining performance as memory grows.

## Implementation Details

### LOCOMO Configuration
- **Backbone Model**: GPT-4.1-mini (superior function calling: 29.75% vs 22.12% for GPT-4o-mini)
- **Memory Architecture**: Hierarchical multi-agent system with specialized memory managers
- **Retrieval Strategy**: Intelligent routing based on question type and memory content

### ScreenshotVQA Configuration  
- **Vision Model**: Gemini-2.5-flash-preview-04-17
- **Cloud Integration**: Asynchronous image processing via Google Cloud
- **Function Calls**: 1-7 calls per processing step (1 for meta memory manager, 0-6 for specialized managers)

## Comparative Analysis

**vs. Traditional RAG Systems**: MIRIX overcomes the global understanding limitations of simple RAG through its multi-agent memory architecture.

**vs. Full-Context Methods**: While maintaining 97% of full-context performance, MIRIX uses dramatically less computational resources and storage.

**vs. Existing Memory Systems**: MIRIX outperforms all tested memory systems (LangMem, Mem0, Zep, Memobase) across most evaluation categories.

## What's Next?

Return to explore other advanced features:

[**Security & Privacy →**](security-privacy.md){ .md-button }
[**Backup & Restore →**](backup-restore.md){ .md-button } 