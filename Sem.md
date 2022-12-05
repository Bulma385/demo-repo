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

2) TaBERT:
- previous work on joint representation learning of NL utterances and structured data
(Bogin 2019b, Wang 2019a)
- with BERT for DB but no pretraining (Guo 2019, Zhang 2019, Hwang 2019) 


3) GitTables:

### Classification of RW & Discussion of the approaches:
#### Data:
1) TURL: 570k relational Web tables from Wikipedia
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
- Create content snapshot of table based on input utterance
- concat with input utterance 
- Transformer (e.g. BERT) -> row-wise encoding vectors 
- series of vertical self-attention layers -> utterance and column representations
##### 3) GitTables:
No new architecture, just data set.

#### Fine-tuning:

#### Evaluation (Experiments, Benchmarks, goals):



## Conclusion & Outlook:
(10%)
