# Data Analysis and Statistical Inference

In this first chapter we define and discuss some important concepts regarding data and scientific investigation.

## Data, Experiments, and Studies

We encounter numerical summaries of information constantly in our everyday lives and sort through these in order to make all sorts of decisions.  In this section we will formalize the concept of *data* connected to scientific study.  

Two types of studies in which data are collected and analyzed are in designed, interventional experiments, and in observational studies.  The following illustrations help to differentiate these two types of studies.

### James Lind's Scurvy Trial

Clinical trials are familiar designed, interventional experiments.  A famous, early example is James Lind's Scurvy trial.  Lind was a doctor aboard a British ship.  Several sailors with him were suffering from scurvy.  He selected 12 sailors in similar, poor condition, and assigned to pairs of them 6 different treatments (the intervention).  The two who received oranges and lemons to eat recovered fully; those who received apple cider fared next best.

### Framingham Heart Study

Named for Framingham, Massachusetts, where the participants were first recruited, is a long-running observational study of Americans aimed at understanding risks associated with heart disease.  Participants agreed to medical testing every 3-5 years, from which the study researchers concluded a number of important findings, such as cigarett smoking substantially increases the risk of heart disease.  There are no interventions; the researchers simply observe the patients and make 
conclusions based on how the patients choose to live, e.g., tobacco use.  

### Harris Bank Sex Pay Study 

93 salaries of entry-level clerical workers who started working at Harris Bank between 1969 and 1971 show men were paid more than women.  (From The Statistical Sleuth, reproduced from “Harris Trust and Savings Bank: An Analysis of Employee Compensation” (1979), Report 7946,Center for Mathematical Studies in Business and Economics, University of Chicago
Graduate School of Business.)

### Study Concepts 

The above examples illustrate several key concepts related to scientific studies.

 - Research question - There is always a reason researchers go to the trouble of collecting and analysing data; they have an important question they want to answer.  For example, what can sailors do to prevent scurvy?
 - Experimental units/Subjects - the research question usually references people, things, animals, or some other entity that can be studied in order to answer the question. When these are observed and measured then they are called experimental units or subjects.  
 - Data - we are inundated with information, numbers, figures, and graphs in our everyday lives.  Is this data?  Anything information gathered to answer a particular research question can be considered data.  Relevancy to a research question is key.  
 -  Intervention - When James Lind gave different foods to sick sailors he was making an intervention, and his goal was to study the effect of his interventions on the sailors well-being.  Experiments include one or more interventions, whereas observational studies do not feature any interventions on the part of the researcher.
 - Randomization - When researchers intervene, they should apply their interventions randomly with respect to subjects.  In experiments the experimental units are the entities that are randomized and given interventions.
 - Response/outcome variables - studies often measure multiple variables and study relationships between them.  Typically the researchers expect one variable is affected by another.  The response, or outcome---like the health of sailors, or pay of workers---is the variable beign effected by the intervention in an experiment or by another, independent variable in an observational study.
 - Control - Researchers should try to limit the effects of variables on the response that are not of interest to the study.  For example, in the gender pay study, the researchers studied only entry-level workers.  They *controlled* for prior experience to better isolate potential sex effects on pay. 


### Randomization, control, and causation

In experiments the researcher performs one or more interventions---such as giving patients cider versus citrus fruits in Lind's scurvy trial.  The principle of {\em randomization} asserts that interventions in experiments should be assigned to experimental units randomly.  When experimental units are heterogeneous---not all the same---it stands to reason that some of their differences apart from the intervention may impact the response to the experiment recorded by the researcher.  Randomization is a way to even out these heterogeneities between groups receiving different interventions.  That way, it is the intervention, rather than some other difference, which is responsible for substantially different outcomes.  Randomization systematically accounts for heterogeneities, but the extent to which it works depends on the number of experimental units, the number of groups being randomized, and the presence of one or more important heterogeneities.  For examples, consider the following:
1. Suppose in ten experimental units there is one unobserved, dichotomous trait that affects the experimental response.  Three of the ten have version "0" of the trait and 7 have version "1".  Randomly split the ten into two groups of five, each group to receive a different intervention.  The chance all three end up in the same group is 1/6, not ignorably small... 
2. On the other hand, suppose there are 100 experimental units, half have trait "0" and half trait "1".  The chance at least 35 of either trait type end up in the same one half random split is $\approx 0$.  
Generally when an intervention is randomized over experimental units we interpret any significant difference in outcome/response between intervenion groups as having been {\em caused} by the intervention itself, as opposed to some other unobserved characteristic---these are sometimes called {\em lurking variables} or {\em confounding variables}.  But, randomization is not foolproof; small sample sizes (few experimental units) and/or the presence of many confounding variables can reduce the effectiveness of randomization.


When the researcher knows about potential confounders ahead of time, the principle of *blocking* says experimental units should be representatively divided with respect to intervention across values of this variable.  For example, if experimental units are humans both young and old, then the different interventions should be applied to equal numbers of young and old people.  One way to accomplish this is to let age group be a *blocking factor*.  In the case of a dichotomous intervention this means half of the old people will be randomly assigned to one intervention and half of the young people will be randomly assigned to one intervention---as opposed to randomly assigning half of all the experimental units to one intervention.  

The principle of *control* states that intervention groups should be made as homogeneous as possible.  When experiments are well-controlled researchers often assume that they can determine *causation*, and any observed differences in experimental outcome between intervention groups can be attributed to the intervention.  Of course, as mentioned above, the ability of an experiment to determine causation is not all or nothing; rather, it depends on unknowns. Nevertheless, stronger controls make the results of experiments more trustworthy, and less likely to be caused by confounding variables.

Non-interventional, observational studies are not used to establish causative relationships.  Rather, we say such studies establish *associations* between variables.  For example, in the Framingham study, the researchers did not randomly assign individuals to groups of tobacco-users and non-users.  Even though these days the evidence is quite strong that tobacco use causes heart disease, the bar for such a claim is much higher when the variable of interest---tobacco use---cannot be randomly assigned to experimental units.  That's not to say elements of control cannot be used.  For instance, if enough data is collected, it is possible to compare tobacco-users and non-users with nearly the same ages, sexes, incomes, education, living in the same zip codes, etc.  The more potential confounders are explicitly controlled, the closer such an observational study comes to a randomized experiment.  

### Populations and scope of inference

Whether conducting and experiment or collecting observational data, the units/subjects have to come from somewhere.  *Sampling* describes how subjects are obtained for observation.  *Random sampling* is any scheme involving selecting a subset of subjects from a larger group in some random fashion.  A *simple random sample* of size $n$ is obtained when every subset of $n$ subjects from a total group is equally likely to be selected.  Other types of random selection are possible, but we won't often consider these:
  - *stratified random sampling* obtains when simple random samples from separate groups/strata are combined, e.g., a 50/50 random sample stratified by male/female can be formed by taking a simple random sample of ten males and a simple random sample of 10 females from a group of 50 males and 50 females
  - *cluster random sampling* obtains when a larger group is subdivided into smaller groups and subgroups are selected at random, e.g., a cluster random sample of Iowa high schoolers can be obtained by choosing all high schoolers who attend one of a simple random sample of Iowa high schools.
  
When subjects are randomly sampled from a larger group we call that larger group the *population*.  Generally, conclusions about subjects in the study---whether it is an experiment or an observational study---may be assumed to hold for the wider population as a whole when the subjects are chosen randomly.  The details of this *generalizability* of results depend on the type of random sampling conducted; we'll focus on the case of simple random sampling specifically. On the other hand, when subjects are not randomly sampled from the population, study results cannot be generalized back to the population.  The reason is that the lack of randomness in selection implies some subsets of the population are more or less likely to be present in the sample of subjects observed, hence, that sample is not necessarily *representative* of the population.  For an extreme example, consider a population of both young and old people and an experiment studying the effects of Covid-19.  It's well-known Covid-19 is much more harmful to old people compared to young people.  So this is a potential confounder.  If we select only young people to study, then we certainly cannot claim the results would be similar had we studied both young and old people.  

Non-random sampling schemes are quite common because they are usually easier and cheaper to implement than random sampling schemes.  A *convenience sample* is just what it sounds like---a rule that selects subjects that are easy to select---such as conducting a poll of your closest friends.  When a non-random sample is used, remember that the results cannot be interpreted beyond the group subjects that were observed.

Sometimes a researcher intends to study one population, but possese data from another population.  This mismatch is important to identify as it can cause bias---which simply means the answer to the researcher's question is different for the population for which data is observed compared to the intended population.  As in the extreme example above, effects of Covid-19 are different in old and young populations, so the results from an experiment studying only the young are biased when viewed from the perspective of the population of old and young combined.  

## Statistical Inference in randomly sampled, randomized experiments

In this section we will cover some familiar statistical methods for inference in one- and two-sample problems.  *Inference* refers to generalizing results from a sample of experimental units/subjects to a population while properly accounting for *statistical uncertainty*.  The latter is the variability in the experimental outcome/observation that naturally follows from the fact the subjects were chosen randomly from the population.  


### One- and Two-sample inference for a population mean

Let $P$ denote a population for which the mean $\theta(P):=E(X), \,X\sim P$ and second moment $E(X^2)$ exist i.e., $E(X^2)<\infty$.  Let $X_1, \ldots, X_n$ denote a simple random sample (independent, and identically distributed collection) from $P$.

The sample mean $\overline{X}$ is *unbiased* for $\theta$, that is, its expectation is 
\[E(\overline X) = E\left(\tfrac1n \sum_{i=1}^n X_i\right) = \tfrac1n \sum_{i=1}^n E(X_i) = \tfrac1n \sum_{i=1}^n \theta = \theta\]
is equal to $\theta$.

Since $X\sim P$ has two finite moments it has a finite variance, denoted $\sigma^2$.  The variance of $\overline X$ is
\[V(\overline X) = V\left(\tfrac1n \sum_{i=1}^n X_i\right) = \tfrac{1}{n^2}\left(\sum_{i=1}^n V(X_i) + \sum_{i=1, j>i}^n 2Cov(X_i, X_j)\right)\]
\[ = \tfrac{1}{n^2}\left(\sum_{i=1}^n V(X_i)\right) = \sigma^2/n.\]

The central limit theorem (CLT) says that
\[\sqrt n(\overline{X} - \theta) \stackrel{D}{\rightarrow} N(0, \sigma^2)\]
where $V(X) =\sigma^2$ and ``$\stackrel{D}{\rightarrow}$" denotes convergence in distribution.  Reminder: convergence in distribution means pointwise convergence of cumulative distribution functions; so the scaled, centered sample mean $\sqrt n(\overline{X} - \theta)$ has a distribution function (that depends on the sample size $n$) that converges pointwise to the normal distribution function with variance $\sigma^2$.

A $100(1-\alpha)\%$ confidence interval (CI) for $\theta$ is a random interval $(\ell, u)$ such that
\[P(\ell \leq \theta \leq u) \geq 1-\alpha.\]
Let $z_\alpha$ denote the $\alpha$ quantile of the standard normal distribution, i.e., if $Z$ is a standard normal r.v. then $z_\alpha$ satisfies $P(Z\leq z_\alpha) = \alpha$.  Suppose $\sigma^2$ is known and let $\ell := \overline{X} + z_{\alpha/2}\sigma/\sqrt{n}$ and $u := \overline{X} + z_{1-\alpha/2}\sigma/\sqrt{n}$.  Then, $(\ell, u)$ is a $100(1-\alpha)\%$ CI for $\theta$.

Interpretation of CIs: You likely have heard an interpretation of the ``confidence property" before. It goes something like the following: imagine repeating the random sampling of data from $P$ many, many times, say 1000 times, and for each set of data computing a, say, $95\%$ CI for $\theta$.  Confidence means about 950 of these 1000 intervals contain the true $\theta$.  There's nothing wrong with this understanding, except many people don't understand it.  I prefer a simpler explanation: *a confidence interval is a set of plausible values for $\theta$* where the degree of plausibility obviously is determined by $\alpha$.  This simple explanation is theoretically grounded in *possibility theory* but a discussion of such topics is, unfortunately, beyond our present scope.

Let $\Theta_0$ be a subset of $\mathbb{R}$.  Define the null hypothesis as the assertion $H_0: \theta\in \Theta_0$; correspondingly, the alternative hypothesis is $H_a: \theta\in \Theta_0^c$.  A testing rule $g$ says $H_0$ is retained/not rejected if $g>t$ for some value $t$ and is rejected otherwise.  There are four possibilities:
1. $H_0$ is true and is retained
2. $H_0$ is false and is rejected
3. $H_0$ is true and is rejected, a Type 1 error
4. $H_0$ is false and is retained, a Type 2 error.
Typically, hypotheses are defined such that a Type 1 error is worse than a Type 2 error.  Therefore, we try to minimize the chance of committing a Type 1 error.

Define 
\[T(\theta_0) = \frac{\overline X - \theta_0}{\sigma/\sqrt{n}}, \quad \theta_0 \in \Theta_0.\]
Let $g:= \sup_{\theta_0} P(Z > |T(\theta_0)|)$.  Then, $g$ is *p-value* with respect to $H_0$ and if we set $t = \alpha$ then the CLT implies (for large $n$)
\[P(\text{Reject }H_0\text{ when }H_0\text{ true}) \leq 1-\alpha.\]
The most common form of this test is the *point-null* test in which $\Theta_0 = \{\theta_0\}$ is a singleton set (and the supremum in the p-value definition is unnecessary).























## Exercises










