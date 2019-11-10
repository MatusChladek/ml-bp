# Take-aways from [Rules of Machine Learning: Best Practices](http://martin.zinkevich.org/rules_of_ml/rules_of_ml.pdf)

#### Rule #1: Don’t be afraid to launch a product without machine learning 
- basic heuristic can be better when time of deployment and sample size is taken into consideration
#### Rule #2: First, design and implement metrics. 
- track everything, low fidelity experiments, get proxies for unknowns
#### Rule #3: Choose machine learning over a complex heuristic
***

### ML Phase I:
#### Rule #4: Keep the first model simple and get the infrastructure right 
- how to get examples to your model
- how success will look like
- what will be pre-trained what will need streaming
- start with reasonable features
#### Rule #5: Test the infrastructure independently from the machine learning. 
- validate that model perform same way in serving env as in train env
#### Rule #6: Be careful about dropped data when copying pipelines.
- basically data leakage
#### Rule #7: Turn heuristics into features, or handle them externally. 
- think carefully about how to define response
#### Rule #8: Know the freshness requirements of your system. 
- how much performance degrades depending on time since last training
#### Rule #9: Detect problems before exporting models 
- sanity checks before you export model
#### Rule #11: Give feature column owners and documentation 
- have proper description of features; how they are obtained/maintained (by who = assign person)
#### Rule #12: Don’t overthink which objective you choose to directly optimize. 
- meaning number of clicks or number of site visits. But think carefully what objective really captures desired scenario
#### Rule #13: Choose a simple, observable and attributable metric for your first objective. 
- try modeling direct metrics first (was email opened?) vs indirect (was something purchased next day)
- Leave indirect for MAB
#### Rule #14: Starting with an interpretable model makes debugging easier 
- easier to debug than models that use objectives (zero­one loss, various hinge losses, etc)
- Separate Spam Filtering and Quality Ranking in a Policy Layer  
- be careful about answering different questions from one model
- feature/data selection can cause harm to second order answers
***

### ML Phase II: Feature Engineering 
After you have a working end to end system with unit and system tests instrumented

#### Rule #16: Plan to launch and iterate. 
- don't look for perfect model but rather iterate; time is money
#### Rule #17: Start with directly observed and reported features as opposed to learned
features. 
- deep features (deep feature synthesis) comes second
#### Rule #19: Use very specific features when you can. 
- use Label/OneHot encoders with regularization
#### Rule #20: Combine and modify existing features to create new features in
human­understandable ways. 
- discretization & crosses aka create discrete bundles out of more sparse/continuous feature & create cross product of dummy vars
#### Rule #21: The number of feature weights you can learn in a linear model is roughly
proportional to the amount of data you have. 
- you are always able to infer something; If you have smaller dataset just decrease complexity of the model
#### Rule #22: Clean up features you are no longer using. 
- personalized feature which only applies to 10% of pop sucks but when that feature is significant you want probably include it
#### Rule #23: You are not a typical end user. 
- If you really want to have user feedback, use user experience methodologies. Create user
personas (one description is in Bill Buxton’s Designing User Experiences) early in a process and
do usability testing (one description is in Steve Krug’s Don’t Make Me Think) later.
#### Rule #24: Measure the delta between models 
- explore differences in actual predictions between models
#### Rule #25: When choosing models, utilitarian performance trumps predictive power. 
- look for robust model from multiple POVs
#### Rule #26: Look for patterns in the measured errors, and create new features. 
- based on current error try to create features that would be good at capturing those errs
#### Rule #27: Try to quantify observed undesirable behavior. 
- measure first, optimize second
#### Rule #28: Be aware that identical short term behavior does not imply identical long­term behavior. 
- revisit your models, don't be obsessed with longer dsets since local data may be more relevant
#### Rule #29: The best way to make sure that you train like you serve is to save the set of features used at serving time, and then pipe those features to a log to use them at training time.
#### Rule #30: Importance weight sampled data, don’t arbitrarily drop it! 
- Importance weighting means that if you decide that you are going to sample example X with a 30% probability, then give it a weight of 10/3. add eg exponential decay as new feature to your dset
#### Rule #31: Beware that if you join data from a table at training and serving time, the data in the table may change. 
- prevent data leakage in both directions (use featuretools instead of dummy way)
#### Rule #32: Re­use code between your training pipeline and your serving pipeline
whenever possible. 
- keep same language in training and serving envs
#### Rule #33: If you produce a model based on the data until January 5th, test the model on the data from January 6th and after. 
- area under the curve, which represents the likelihood of giving the positive example a score higher than a negative example
#### Rule #34: In binary classification for filtering (such as spam detection or determining interesting e­mails), make small short­term sacrifices in performance for very clean data. 
- do not try to learn on missclassified data directly but rather keep holdout data (eg keep sending email to customers on crazy hours) to keep training sample nonbiased
#### Rule #35: Beware of the inherent skew in ranking problems. 
- when you switch ranking alg (aka test open feqs on times which does not make sense but you want data) you are changing data your alg will see
#### Rule #36: Avoid feedback loops with positional features. 
- create feature for rank item was shown to user in training env
#### Rule #37: Measure Training/Serving Skew. 
- tune your regularization to optimize for tomorrow data
#### Rule #38: Don’t waste time on new features if unaligned objectives have become the issue. 
- For instance, you may optimize clicks, plus­ones, or downloads, but make
launch decisions based in part on human raters.
#### Rule #39: Launch decisions are a proxy for long­term product goals. 
- launch decisions depend on multiple criteria, only some of which can be directly optimized using ML = if you improve something be careful that something else doesn't get worse; The only easy launch decisions are when all metrics get better (or at least do not get worse). 
- Multi­objective learning? - formulate a constraint satisfaction problem that has lower bounds on each metric, and optimizes some linear combination of metrics
#### Rule #40: Keep ensembles simple. 
- To keep things simple, each model should either be an ensemble only taking the input of other models, or a base model taking many features, but not both. 
#### Rule #41: When performance plateaus, look for qualitatively new sources of information to add rather than refining existing signals. 
- start building the infrastructure for radically different features, such as the history of
documents that this user has accessed in the last day, week, or year, or data from a different property.
- Use wikidata entities or something internal to your company (such as Google’s knowledge graph). Use deep learning.
#### Rule #42: Don’t expect diversity, personalization, or relevance to be as correlated with popularity as you think they are. 
- you can do post­processing to increase diversity or relevance. If you see longer term objectives increase, then you can declare that diversity/relevance is valuable, aside from popularity. You can then either continue to use your post­processing, or directly modify the objective based upon diversity or relevance.
#### Rule #43: Your friends tend to be the same across different products. Your interests tend not to be. 
-  use features from different sources