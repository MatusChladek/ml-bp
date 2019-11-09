# Take-aways from Hidden Technical Debt in Machine Learning Systems [link](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf)

In this snippet I decided to summarize main findings from paper published by Google describing phenoema that most of the real-world ML systems incur massive ongoing maintenance costs (in SE terms *technical debt*). 

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
### 1.Entaglement

- if you change distribution of one input signal -> weights, importance & use of remaining features is automatically influenced (true for full, batch & online trained models)
- **CACE** (Change Anything Changes Everything) holds for input signals, hypeparameters, learning settings, sampling methods, convergence threshold & data selection
- due to CACE as opposite to SE design pattern encapsulation & modular design is hard

#### MITIGATION STRATEGY:
a) isolate models & serve ensembles where errors in the components models are uncorrelated
b) detect changes in predictions as they occur [paper](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/41159.pdf)


    
