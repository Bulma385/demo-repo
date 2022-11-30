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
- Developing new Model Architectures to better capture tabular and natural language data.
- Accumulating better data sets: larger and rather database-like data than web-tables (see GitTables paper)


## Main Part (75%):
### Related Work:
#### Classification of RW & Discussion of the approaches:
##### Data:
asmd
##### Model Architecture and Pretraining:
###### 1) TURL:
Consists of 3 Modules:
- embedding layer:
- N stacked structure-aware Transformer encoders
- projection layer
###### 2) TaBERT:
- Create content snapshot of table based on input utterance
- concat with input utterance 
- Transformer (e.g. BERT) -> row-wise encoding vectors 
- series of vertical self-attention layers -> utterance and column representations
###### 3) GitTables:
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
