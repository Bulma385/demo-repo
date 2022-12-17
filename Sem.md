# Survey Paper
Title:
Abstract:
## Introduction (15%):
### relevant definitions for the topic:
- Token:
- Entity:
- Entity mention:
- Natural language utterance:
- Schema:
- (Benchmarks: entity linking, column type annotation, relation extraction, row population, cell filling, schema augmentation)

### Potential Challenges:
- Developing new Model Architectures to better capture tabular and natural language data. (Semi-structured format and factual knowledge of rel. tables (2 main challenges of TURL)
- Accumulating better data sets: larger and rather database-like data than web-tables (see GitTables paper)
- Generalization. Less task-specific finetuning.

## Main Part (75%):
### Related Work:
1) TURL:
Data:
- Cafarella [6,7] 154M rel. Tables
- Bhagavatula [4] 1,6M high quality Wikipedia rel. tables
- table interpretation [4,17,30,36,48,49], table augmentation [1,6,12,43,46,47], but e.g [4,46,47] too much task specific engineering
- representation learning model [13] (using Word2Vec [29]), but lacks contextualized representation
- GloVe [31]
- contextualized word representations [16,32,44]
- Embedding KB into continuous vector spaces [38], KB completion [5,41], relation extraction [35,42], data integration [18]
- ! ERNIE [50] combination KB information and BERT
- ! (recent for TURL) pretraining on Web tables [21,45]
- benchmarks?

2) TaBERT: (Yin 2020)
- previous work on joint representation learning of NL utterances and structured data
(Bogin 2019b, Wang 2019a)
- with BERT for DB but no pretraining (Guo 2019, Zhang 2019, Hwang 2019) 
- 

3) GitTables:

4) TABBIE ("TABular Information Embedding) (2021):
- cometetive or better than TaBERT
- TaBERT or TaPas more comp expensive because of longer sequences
- repurpose ELEKCTRA's objective function (Clark 2020)

() 5) TaPas:(Herzig 2020) concat tabular data with text like TaBERT

Others:
- Bogin 2019 ("Representing Schema Structure with Graph Neural Networks for Text-to-SQL Parsing") (2019th state-of-art)
- Wang 2019 ("TabFact: A Large-scale Dataset for Table-based Fact Verification") (still far from human performance)
- Hwang 2019 ("A Comprehensive Exploration on WikiSQL with Table-Aware Word Contextualization") (first to beat human performance on wikisql dataset)

### Classification of RW & Discussion of the approaches:
#### Data:
1) TURL: 570k relational Web tables from Wikipedia
2) TaBERT: parallel corpus of 26 million (semi-)structured web tabels and english paragraphs from Wikipedia and WDC WebTable Copus (Lehmberg 2016, from CommonCrawl), aggressive cleaning used, (sub-tokenization using Wordpiece tokenizer shipped with BERT?)
3) GitTables
4) SPIDER text-to-sql (zero-shot)
#### Model Architecture and Pretraining:
##### 1) TURL:
The model architecture consists of 3 modules:
- embedding layer:
- N stacked structure-aware Transformer encoders
- projection layer

Approach: Contextualized representation learning using unsupervised pretraining :
- Visibility Matrix to model row-column structure (instead of conventional bi-directional Transformer)
- MER (Masked Entity Recovery) objective: -> learn factual knowledge; sometimes keep Entity Mention -> learn connections between words and entities
- MLM (Masked Language Model) from BERT

##### 2) TaBERT:
Approach: 
- on top of BERT
- learn contextualized representation of both natural language utterances and structured data
- linearize table content
- Pretraining: 
    - MLM for NL representations
    - MCP (Masked Column Prediction) objective: recover column information from context
    - CVR (Cell Value Recovery) obj.: 


- Create content snapshot of table based on input utterance
- concat with input utterance 
- Transformer (e.g. BERT) -> row-wise encoding vectors 
- series of vertical self-attention layers -> utterance and column representations

##### 3) GitTables:
No new architecture, just data set.

##### 4) TABBIE:
Approach:
- Problem: joint pretraining with tabular data and text (e.g. question answering) -> underperforming on task with only tables (e.g. cell population)
- provide embeddings of all substructures (cells,rows,columns)
- pretraining objective ("Corrupt Cell Detection") from ELEKTRA (Clark 2020): binary classifier as final layer (corrupted or not)
   -> turns out no need for seperate corrupted candidates generator model, intra/table corruption strategies sufficient
- reduce sequence length by architecture
- produce representations of cells x_ij, rows r_i, columns c_j mor accessable through architecture

Architecture:
- 2 Transformers: one for row, one for columns
- pooling both representations after each layer
1. fixed BERT model for out-of-context representations of each cell individually
2. add learned positional embeddings (one for row, one for column)
-> initialization of x_ij
3. contextualize embeddings: row-wise & column-wise with respective Transformers, then fuse them by averaging
(4. [CLS] tokens for rows and columns [CLSROW], [CLSCOL])

#### Fine-tuning:

TABBIE:
- 3 tasks: column population, row population, column type prediction
- Strategy: use final layer representaions, i.e. final row and column representaions and place classification layer on top of the relevant one 
- task-specific hyperparameters
- backpropagate error to ALL model parameters (no "freeze")
- Column Population: Multi-Label Classification (Zhang and Balog 2017 dataset, 1.6M wiki tables with 130k possible header labels) state-of-art

#### Evaluation (Experiments, Benchmarks, goals):
Goals: 
TaBERT: 
- can be general-purpose encoder (for e.g. neural semantic parser)("drop-in replacement of parser's original encoder")
Benchmarks/ Evaluation metrics:
- (1) "Spider" text-to-SQL dataset (Yu 2018c)(only competetively)
- (2) WikiTableQuestions (Pasupat and Liang 2015)(new state of art)

TABBIE comparison with TaBERT (even with same training data?): 
- table showing comparison (outperforms everywhere but few exceptions)
- but NOT tested for table-and-text tasks! -> maybe architecture only suited for tested tasks! -> future work



## Conclusion & Outlook:
(10%)
dcwc
