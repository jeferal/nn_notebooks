# Resumen de Experimentos — Algoritmos de Aprendizaje Profundo

Todos los experimentos siguientes son evaluados en el mismo conjunto de test del dataset **OffendES**
usando **F1-macro** como métrica principal, igual que los baselines clásicos.

---

## Clasificación binaria (2 clases: Offensive / No offensive)

| # | Experimento | Representación | Arquitectura | Parámetros | F1 Macro |
|---|-------------|----------------|--------------|-----------|---------|
| 1 | Standard MLP + BCE | TF-IDF unigramas | Linear(d→128) → Linear(128→64) → Linear(64→1) → Sigmoid | | 0.7609 |
| 2 | MLP + BCEWithLogitsLoss + pos_weight | TF-IDF unigramas | Linear(d→128) → Linear(128→64) → Linear(64→1) | | 0.7490 |
| 3 | MLP + Focal Loss (α=0.865, γ=2) | TF-IDF unigramas | Linear(d→128) → Linear(128→64) → Linear(64→1) | | 0.7658 |
| 4 | MLP + Focal Loss (α=0.865, γ=2) | TF-IDF bigramas | Linear(d→128) → Linear(128→64) → Linear(64→1) | | 0.7704 |
| 5 | SimpleEmbeddingNN + BCEWithLogitsLoss | Word Embeddings (dim=80) | Embedding → AvgPool → Linear(80→128) → ReLU → Linear(128→1) | | 0.7422 |
| 6 | TextCNN 1D | Word Embeddings (dim=100) | Embedding → 3×Conv1D(k=2,3,4, 128 filtros) → MaxPool → Linear(384→2) | | 0.7498 |
| 7 | BiLSTM (2 capas, bidireccional) | Word Embeddings (dim=100) | Embedding → BiLSTM(hidden=128, layers=2) → MaxPool → Linear(256→2) | | 0.7461 |
| 8 | Fine-tuning BERT español | Contextual (BERT) | `dccuchile/bert-base-spanish-wwm-cased` + cabeza de clasificación | | 0.8376 |
| 9 | Fine-tuning RoBERTuito hate-speech | Contextual (RoBERTa) | `pysentimiento/robertuito-hate-speech` + cabeza de clasificación | | 0.8506 |
| 10 | LLM Zero-shot † | — | `Qwen/Qwen2.5-3B-Instruct` (8-bit), sin entrenamiento | | 0.4608 |
| 11 | LLM Few-shot (2 ejs./clase) † | — | `Qwen/Qwen2.5-3B-Instruct` (8-bit), sin entrenamiento | | 0.7108 |
| 12 | LLM Chain-of-Thought † | — | `Qwen/Qwen2.5-3B-Instruct` (8-bit), sin entrenamiento | | 0.5343 |

† Evaluado sobre un subconjunto de **300 muestras** (no las 3 342 del test completo); los F1 no son directamente comparables con el resto.

---

## Clasificación de 4 clases (NO / NOM / OFP / OFG)

| # | Experimento | Representación | Arquitectura | Parámetros | F1 Macro |
|---|-------------|----------------|--------------|-----------|---------|
| 1 | Fine-tuning BERT español 4 clases | Contextual (BERT) | `dccuchile/bert-base-spanish-wwm-cased` + cabeza de clasificación (4 salidas) | | 0.6922 |
| 2 | Clasificación jerárquica BERT (2 etapas) | Contextual (BERT) | Etapa 1: BERT binario (Off./No off.) → Etapa 2a: BERT (OFP/OFG) + Etapa 2b: BERT (NO/NOM) | | 0.6730 |
