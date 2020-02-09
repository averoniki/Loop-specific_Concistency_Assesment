# Loop-specific Concistency Assesment Method

This function evaluates consistency within all first-order and second-order closed loops in a network formed by multiple treatments. 


#### Prerequisites

<b>You need to download and install the R packages 'combinat' and 'metafor' </b>*


## USAGE 

`ifplot.fun (id, theta, var.theta, tt1, tt2, plot = T, common.tau=F, method="DL")`


###### REQUIRED ARGUMENTS

The function must contain at least five arguments, with the following variables:

**id:**		This is the study id.

**Studies:** 	with more than two treatments must have ALL possible comparisons (e.g. a study reporting A, B, and C treatment 			must have all three AB, AC and BC contrasts) and should have the same id.

**theta:**		A symmetric effect size (eg. logOR, MD)

**var.theta:**	The variance of the effect size 

**treatA:**	The code of the first treatment 

**treatB:**		The code of the second treatment


###### OPTIONAL ARGUMENTS: 

**plot:** 		To plot the loop inconsistencies as forest plot (the default is ‘T’).
**common.tau**	To estimate inconsistency with a common heterogeneity within each loop (the default is ‘F’).

**method:**		To specify the method for estimating the heterogeneity variance to be used in the pairwise random effect meta-			analyses (the default is ‘DL’)


The available heterogeneity estimators that can be specified via the method argument are the following
- **HS**: Hunter-Schmidt estimator [2].
- **HE**: Hedges estimator [3, 4].
- **DL**: DerSimonian-Laird estimator (Method of Moments) [4, 5].
- **SJ**: Sidik-Jonkman estimator [6].
- **ML**: Maximum-likelihood estimator [4, 7].
- **REML**: Restricted maximum-likelihood estimator [4, 8].
- **EB**: Empirical Bayes estimator [9, 10].
 
###### Supporting sub-routines

.needed.vectors .needed.vectors.q .possible.loops .possible.loops.q  combn  nCm new.meta
  
  
## DETAILS 
The function evaluates the property of consistency within all first-order (triangles) and second-order (quadrilaterals) closed loops in the network of treatments. Each triangular loop includes three different comparisons, whereas each quadrilateral loop encompasses four different comparisons.  Within each loop a random-effects subgroup meta-analysis is performed, where each comparison is a different subgroup. The between-study variance is estimated with one of the various estimators that have been suggested in the literature, including the Hunter-Schmidt estimator (HS) [2], the Hedges estimator (HE) [3, 4], the DerSimonian-Laird estimator (DL) [4, 5], the Sidik-Jonkman estimator (SJ) [6], the maximum-likelihood estimator (ML) [4, 7], the restricted maximum-likelihood estimator (REML) [4, 8], or the empirical Bayes estimator (EB) [9, 10]. It is possible to perform each subgroup meta-analysis under the common within-loop  heterogeneity [11] (common.tau=T) or under each comparison’s estimated heterogeneity (common.tau=F). Inconsistency is estimated as the difference between direct and indirect comparison for a randomly chosen contrast within each closed loop.


## COMMENTS 
This is not a formal test and confidence intervals shall not be strictly interpreted as usually to draw conclusions regarding the consistency of the entire network. The tests are not independent as the loops share studies and entire meta-analyses; a possible mistake in the data entry in one study could cause inconsistency in all loops that include this study. The plot provides a rather intuitive and graphical approach that can direct the identification and location of sources of information that disagree. Once a loop (or more) appear to be highly deviant from the null, further investigation of the causes for this apparent inconsistency is required. 
When the option ‘common.tau=T’ is selected, the within-loop heterogeneity may be different for the same comparison in different loops. The current process may introduce heterogeneity even in comparisons that are evaluated in a single study. This may lead to wider confidence intervals (CI) of summary effects and decrease the chances to identify statistically significant inconsistencies. 
	To minimize correlations induced by multi-arm trials included in a loop, the most frequent comparison in terms of studies is excluded from the multi-arm study. In such a case the function reports ‘The study with id=_ has more than two treatments in this loop (out is row nr _ for comparison _)’. If a loop is formed by a single multi-arm study then you should get the message ‘The loop _ is described by only one multi-arm trial. There is no inconsistency for the loop _’. 
INPUT: 
	The five necessary arguments (id, theta, var.theta, tt1, tt2) for the function have been produced after the Head to Head meta-analysis.
OUTPUT: 
This function identifies both possible and available first-order and second-order closed loops. For each available loop it returns the frequency of the included comparisons in terms of clinical trials. Within each loop the function defines the direct pooled effect sizes and the indirect estimate for a randomly chosen contrast. Then inconsistency is estimated as the difference between the direct and indirect estimates. Under the null hypothesis that there is no inconsistency the function calculates the z-scores and their p-values for every closed loop identified in the network. Then a matrix is returned with elements the names of all available closed loops and comparisons, the direct estimates along with their standard errors, the sample size of each comparison, the within-loop heterogeneity (when ‘common.tau’=T), the inconsistency factor (IF) and its standard error, and the z-scores and their p-values. The number of columns of the returned matrix is either 18 (‘common.tau=T’) or 17 (‘common.tau=F’) and the number of rows is the length of the available loops. The function provides us also with a plot of all IF along with their confidence intervals to identify any possible inconsistencies. 


## EXAMPLE:
**Explore inconsistency**
The Diabetes dataset includes 21 clinical trials comparing six antihypertensive drugs as defined below: 
Letter	a	b	c	d	e	f
Treatment	ARB	ACE inhibitor	calcium-channel blocker (CCB)	beta-blocker	Thiazide diuretic	placebo

To produce inconsistency plots and statistics under the common within-loop heterogeneity, where the amount of heterogeneity is estimated with the DerSimonian-Laird estimator, type:
> ifplot.fun(id, theta, var.theta, tt1, tt2, plot = T, common.tau = T, method="DL")

*-----  Evaluating the consistency of the network ------* 

  Nr of treatments:  6
  Nr of all possible first order loops (triangles):  60
  Nr of available first order loops (triangles):  13 
 

  Nr of all possible second order loops (quadrilaterals):  90
  Nr of available second order loops (quadrilaterals):  0 
 

 **1** : Evaluation of the loop bce
 Direct comparisons in the loop: 
	bc ce be 
 	3  2  2 

 The study with id=2 has more than two treatments in this loop (out is row nr 21 for comparison bc)
   Meta-analysis for the bc comparison
   mean(se)= -0.13(0.128)
   Meta-analysis for the ce comparison
   mean(se)= -0.198(0.077)
   Meta-analysis for the be comparison
   mean(se)= -0.416(0.079)
   Indirect summary estimate for the be comparison
   Mean(se)= -0.328(0.149)
 
   Inconsistency within the loop:  Mean(se)= 0.088(0.169)


 **2** : Evaluation of the loop bcd
 Direct comparisons in the loop: 
	bc cd bd 
 	3  5  3 

 The study with id=1 has more than two treatments in this loop (out is row nr 20 for comparison cd)
 The study with id=20 has more than two treatments in this loop (out is row nr 29 for comparison cd)
   Meta-analysis for the bc comparison
   mean(se)= -0.222(0.116)
   Meta-analysis for the cd comparison
   mean(se)= -0.247(0.08)
   Meta-analysis for the bd comparison
   mean(se)= -0.175(0.103)
   Indirect summary estimate for the bd comparison
   Mean(se)= -0.469(0.141)
 
   Inconsistency within the loop:  Mean(se)= -0.294(0.174)


   loop comp1 comp2 comp3  mean1  mean2  mean3  se1   se2   se3    n1 n2 n3 heterogeneity.loop abs(IF) seIF  z.score  p.value
1   bce   bc    ce   be    -0.13 -0.198 -0.416  0.128 0.077 0.079  3  2   2        0            0.088  0.169  0.520    0.603               


   loop comp1 comp2 comp3  mean1  mean2  mean3  se1   se2   se3   n1  n2  n3 heterogeneity.loop abs(IF) seIF  z.score  p.value
2   bcd   bc    cd   bd   -0.222 -0.247 -0.175  0.116  0.08 0.103  3  5   3      0.014          0.294   0.174  -1.685   0.092            
## Authors

Areti Angeliki Veroniki and Georgia Salanti


## Citation

Areti Angeliki Veroniki, Haris S Vasiliadis, Julian PT Higgins, Georgia Salanti, Evaluation of inconsistency in networks of interventions, International Journal of Epidemiology, Volume 42, Issue 1, February 2013, Pages 332–345, https://doi.org/10.1093/ije/dys222
