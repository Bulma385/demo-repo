# Survey Paper
Title:
Abstract:
## Introduction (15%):
### relevant definitions for the topic:
- Token:
- Entity:
- Natural language utterance:
- Schema:

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
- 
### Classification of RW & Discussion of the approaches:
#### Data:
asmd
#### Model Architecture and Pretraining:
##### 1) TURL:
Consists of 3 Modules:
- embedding layer:
- N stacked structure-aware Transformer encoders
- projection layer
##### 2) TaBERT:
- Create content snapshot of table based on input utterance
- concat with input utterance 
- Transformer (e.g. BERT) -> row-wise encoding vectors 
- series of vertical self-attention layers -> utterance and column representations
##### 3) GitTables:
No new architecture, just data set?

classification of existing research
Core idea/concept
model architectures: Table comparing approaches (folie 29, workshop)
pretraining approaches
fine tuning
Evaluation environment: experiments and benchmarks, different goals, more powerful?
results

Conclusion & Outlook:
(10%)
