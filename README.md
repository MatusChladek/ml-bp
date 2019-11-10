# Take-aways from Hidden Technical Debt in Machine Learning Systems [link](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf)

In this snippet I decided to summarize main findings from paper published by Google describing phenoema that most of the real-world ML systems incur massive ongoing maintenance costs (in SE terms **technical debt**). 

****

> Developing & deploying ML is realtively fast & cheap **BUT** maintaining them over time is difficult & expensive.

> Feature complete != Production ready & Working software != Good Software

#### Technical debt may be paid down by:
- refactoring code
- improving unit tests
- deleting dead code
- reducing dependencies
- tightening APIs
- improving documentation

#### Goal is not to add new functionality but rather:
- enable future improvements
- reduce errors
- improve maintainability

***

## Complex models
### 1. Entanglement

- if you change distribution of one input signal -> weights, importance & use of remaining features is automatically influenced (true for full, batch & online trained models)
- **CACE** (Change Anything Changes Everything) holds for input signals, hypeparameters, learning settings, sampling methods, convergence threshold & data selection
- due to CACE as opposite to SE design pattern encapsulation & modular design is hard

#### MITIGATION STRATEGY:
a) isolate models & serve ensembles where errors in the components models are uncorrelated
b) detect changes in predictions as they occur [paper](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/41159.pdf)

### 2. Correction Cascades

- when we have *model X* modelling problem A. And we want to model problem B (with *model Y*) which is only slightly different to problem A. It is tempting to use *model X* as basis for *model Y*.
- it is hard do improvements on *model X* then since any change can influence *model Y*.

#### MITIGATION STRATEGY:
a) add deterministic features to *model X* to distinguish between modelling A or B.


### 3. Undeclared Consumers

- when some systems are silenty using outputs of the model
- change in model then obviously unintentionally affect behavioour of that system

#### MITIGATION STRATEGY:
a) access restrictions
b) SLAs
****

## Data Dependencies
### 4. Unstable Data Dependencies

- input signal that are unstable over time (qualitatively & quantitatively)
- eg. if there is miscalibration in input sensor, model will learn that and once fixed model is biased

#### MITIGATION STRATEGY:
a) versioned copy of all signal with static mapping (relevant mainly for latent spaces)

### 5. Underutilized Data Dependencies

- Legacy features - features included early in development but lost modelling power over time
- Bundled features - if new fact table is included whole and not evaluated on feature by feature basis
- Epsilon features - features with super small accuracy lift but additional overhead
- Correlated features - models picks one of two correlated features which is less causal. If correlations change this is problem.

#### MITIGATION STRATEGY:
a) Run Leave-one-feature-out regularly to identify & remove redundant features
****

## Feedback Loops
### 6. Direct Feedback Loops

- basic supervised algs are used more than theoretically optimal Contextual MABs (have problems with wide action spaces)

#### MITIGATION STRATEGY:
a) use randomization for small sample of data (or tune MABs)

### 7. Hidden Feedback Loops

- two models influence each other. Environments are not isolated well enough.

#### MITIGATION STRATEGY:
a) Take into account other model as feature if possible
b) Simply be aware of that
****

## Design Anti-Patterns
### 8. Glue Code

- code for getting data in/out of packages
- too many generalized packages means less flexibility in modelling specific env properties & tweaking loss functions

#### MITIGATION STRATEGY:
a) Consider creating clean native solution

### 9. Pipeline Jungles

- jungle of scrapes, joins, sampling steps, ..
- a lot of intermediate outputs

#### MITIGATION STRATEGY:
a) Think holistically about data collection & feature extraction

### 10. Dead Experimental Code

- hard to maintain backward compatibility in experimental feature branches to master
- can cause leaks to production

#### MITIGATION STRATEGY:
a) Periodically delete experimental branches

### 11. Common Smells

- Plain-Old-Data-Type smell - go from simple int/float to 2. point decimals/log odds
- Multiple-Language smell - harder testing & knowledge transfer
- Prototype smell - time pressure can push prototype to production

#### MITIGATION STRATEGY:
a) ad Prototype smell - be aware during development - write tests along the way
****

## Changes in Real World
### 12. Fixed threshold in Dynamic systems

- static thresholds for decision function are dangerous in changing world

#### MITIGATION STRATEGY:
a) Learn threshold in simple holdout evaluation each time

### 13. Monitoring & Testing

- comprehensive live monitoring of system behavior
in real time combined with automated response is critical for long-term system reliability

#### MITIGATION STRATEGY:
a) Prediction Bias - distribution of predicted labes vs distribution of observed labels should be same
b) Average observed value
c) Action Limits - eg in bidding - max value on bid, max amount of positive bids
d) Upstream Procedures - monitor success of preprocess/ingest/store pipelines

### 14. Reproducibility Debt

- make sure to be able to reproduce experiments that led to production decision

#### MITIGATION STRATEGY:
a) Keep experiments log

### 15. Cultural Debt

- hard line between ML research and engineering

#### MITIGATION STRATEGY:
a) reward deletion of features
b) reward reduction of complexity
c) reward improvements in reproducibility
d) reward stability
e) reward monitoring



    
