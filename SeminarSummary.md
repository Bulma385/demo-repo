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
potential challenges

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
TAPAS: table QA; extends BERT; weak supervision; trains end-to-end
TUTA
TAPEX: novel pretraining by sql execution over synthetic corpus; combats problem of lacking high quality tabular data; new SOTA on TableQA and TableFV (like WikiTableQuestions, TabFact..), train  end-to-end; fine-tuning for NL 'execution'

#### Datasets
- TURL etc. web tables (bad quality, noizy, need cleaning)
- (Tabfact)
- Gittables
- TaPEx synthesized

### Input Embedding
1) TURL
- linearize table content & compute embedding for each token and entity
- token representation: x_t = w + t + p; w word embedding, t type embedding, p position embedding 
- entity representation: x_e = LINEAR(e_e, e_m) + t_e; e_e learned entity embedding, t_e type embedding, e_m entity mention embedding (mean of word embeddings)  

2) TaBERT:
- linearize table content  (Column name | Column type | Cell value; [SEP] symbol)
- create content snapshot based on input utterance (to reduce size, synthetic rows, cells with highest n-gram overlap)
- concat every row with input utterance

3) TABBIE:
- use pretrained fixed BERT to obtain out-of-context cell representations 
- use [CLS] tokens for rows and columns ([CLSROW], [CLSCOL]) 
- add positional encoding regarding the rows and columns respectively (randomly initialized & learned by pretraining)

4) TAPAS:
- linearize NL query and table content row-wise
- Sum 6 embeddings for each token: token, position, segment (query or table), column, row, rank (sorting comparable column values, asc) (figure) (+ figure in results showing results with removed respective embedding)

5) TAPEx:
- lin. NL and table
- [HEAD] and [Row index] tokens and vertical bar between cells

### Encoder Decoder Architecture
1) TURL:
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

4) TAPAS:
- input embedding -> BERT?
Cell Selection:
- after hidden layers -> classification layer for cell selection: cells modelled as independent Bernoulli vars;  cell logits are avg of token logits in that cell -> probabilities p_s(c) to select cell c (choose all cells with prob > 0.5 for aggregation)
- (inductive bias? for columns; column probs?)
Aggregation Operator selection:
- linear layer & softmax after final hidden layer of first token -> probability p_a(op)

5) TAPEX:
- just uses BART (could be any sequence generating LM) without modifications??????? (needs to be able to generate sequences)

### Pretraining
1) TURL:
- MER (Masked Entity Recovery) objective: -> learn factual knowledge; sometimes keep Entity Mention -> learn connections between words and entities
- MLM (Masked Language Model) from BERT

2) TaBERT:
- MLM
- MCP (Masked Column Prediction)
- CVR (Cell Value Recovery)

3) TABBIE:
- "Corrupted Cell Detection" from ELEKTRA

4) TAPAS:
- mainly Wikipedia tables 2.9M ( < 500 cells), Infoboxes? 3.3M (not typical but increase performance); use table caption, acticle tile and description as proxy for the questions in the end task
- MLM (masking whole words and whole cells beneficial instead of just word pieces)
- no benefit from second pretraining obj (random table with text detection)
- restrict word piece sequence length of associated text & table

5) TAPEX:
- ONLY sql execution: use off-the-shelf sql executor like MySQL for supervision
- more efficient, pretraining corpus does not need to be that large as for MLM derivats; end-to-end
- DATASET: no need for millions of tables; chose 1500 high quality tables from WikiTableQuestions
- Query Sampling: instantiate SQL templates (Zhong et al., 2020a) -> SELECT num1 WHERE text1
= val1, for different values

### Downstream Tasks and task-specific Fine-Tuning

1) TURL:
- entity linking
- col type annotation
- relation extraction
- row pop
- cell filling (same as col pop ?)
- schema augmentation

2) TaBERT: 
- WikiTableQuestions & SPIDER

3) TABBIE:
- column pop
- row pop
- col type prediction

4) TAPAS:
- Table QA: WikiTQ; SQA; WikiSQL
- data (x,T,y) ; split target y into (C,s) where C is the set of selected cells and s can be a scalar answer (if exists) 
- Cell selection: (first column selection?)
- Scalar answer: has to be aggregated; no explicit supervision for aggregation type; given the cell selection probabilities calculate estimate for every operator then sum them weighted with the normalized probabilities of the operators
- loss functions for cell & column probabilities, operator probs, returned scalar
- threshold for choosing only cell selection or aggregation
- fine-tuning takes 10-20 h where pretraining was 3 days (quite long?)
- new SOTA or on par but size limitations and cant handle multiple aggragations at once correctly

### Limitations & Outlook:
- supporting large table input & multiple tables
- aggragating better datasets (gittables paper)




Turl: N stacked structure aware transformers 

TABERT: horizontal row-wise transformer -> row-wise encodings -> vertical (column-wise) self-attention -> pooling -> contextualized
- one encoder, needs additional decoder for sequence generation or classification layer for downstream task like rest; uses MAPO (Liang et al.,
2018) as base sematic parser and replaces encoder with TABERTmodel

TABBIE: first out of context representations; then 2 Transformers row & column individually; pooling after each layer


Transformer based tabular language models all utilize the attention mechanism [Source attention is all you need] to build powerful encoders. Different encoder architectures have been proposed. 
Some approaches (e.g. TAPAS, TAPEX) direktly apply the same self attention mechanism like BERT and BART to the linearized table content while other models have extended 
the architecture specifically for tabular data [e.g. TABERT, TABBIE, TURL].
TABERT presents serial row and column attention. First a transformer computes row-wise representations of the NL text and all rows included in the content snapshot. Then multiple stacked vertical self attention layers enable contextualized representations throughout columsn(rows?). 


1. direct self attention to linearized content: TAPAS, TAPEX

But one can leverage table structure to build more efficient attention mechanisms.

2. serial row & column attention: tabert

3. parallel row & column attention: tabbie

4. joint row column attention: turl by restricting attention mechanism using visibility matrix


TAPEX and UnifiedSKG: encoder and decoder architecture for sequence to sequence 

note: since attention mechanism has quadratic complexity (source?) it benefits from small sequence lengths. so multiple attention comps with smaller seq like row or column can be more efficient than inputting whole table (and rather comp tractable)