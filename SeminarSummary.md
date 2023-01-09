# Title: Survey Paper on ..

important topics: sequence serialization, input embedding, encoding decoding arch, attention, model efficiency

## Abstract

after success of transformers for NLP -> new SOTA achieved in structured tabular domain

hence development from Table2Vec, CNNs, RNNs, GNNs, LSTMs to Transormer based models
we are evaluating different pre-training paradigms and architectures 
& how they perform on various down-stream tasks for table understanding as table question-answering, cell filling, entity linking etc.


## Introduction
same stuff as abstract but more sources
and outlook on the results and approaches

## Preliminaries? 
why structured data difficult
datasets: web tables, database tables ...
relevant definitions?

## Main Part: Related Work: Model Architectures, Pre-Training and Fine-Tuning

### Existing Models and general approaches
Non transformer-based:
Table2Vec, CNNs, RNNs, LSTMs

Transformer-based:
TURL
TABERT
TABBIE
TAPAS

we only analyse transformer-based 

### Input Embedding
1)TURL

2) TaBERT:
- linearize table content  (Column name | Column type | Cell value; [SEP] symbol)
- create content snapshot based on input utterance (to reduce size, synthetic rows, cells with highest n-gram overlap)
- concat every row with input utterance

3) TABBIE:
- use pretrained fixed BERT to obtain out-of-context cell representations 
- use [CLS] tokens for rows and columns ([CLSROW], [CLSCOL]) 
- add positional encoding regarding the rows and columns respectively (randomly initialized & learned by pretraining)


### Encoder Decoder Architecture
1)TURL:
- embedding layer
- N stacked structure-aware transformers using Visibility Matrix to model row-column structure
- projection layer

2) TaBERT:
- Transformer (e.g. BERT) on content snapshot -> row-wise encoding vectors
- series of vertical self-attention layers -> pooling -> utterance and column representations
(Figure)

3) TABBIE:
- get out-of-context representations of each cell from input embedding module
- 2 Transformers: one for rows, one for columns individually
- pooling both representations after each layer for contextualized representations
(Figure)

### Pretraining
1) TURL:
- MER (Masked Entity Recovery) objective: -> learn factual knowledge; sometimes keep Entity Mention -> learn connections between words and entities
- MLM (Masked Language Model) from BERT

2)TaBERT:
- MLM
- MCP (Masked Column Prediction)
- CVR (Cell Value Recovery)

3) TABBIE:
- "Corrupted Cell Detection" from ELEKTRA

### Downstream Tasks and task-specific Fine-Tuning

1)TURL:
- entity linking
- col type annotation
- relation extraction
- row pop
- cell filling (same as col pop ?)
- schema augmentation

2)TaBERT:
- WikiTableQuestions & SPIDER

3) TABBIE:
- column pop
- row pop
- col type prediction
