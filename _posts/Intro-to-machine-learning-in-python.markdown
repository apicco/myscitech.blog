---
layout: post
title: Introduction to machine learning in python
date: 2023-01-25
published: false
excerpt: "Explore the diffrent libraries and get confortable to establish your first machine learning project"
 ---

# Gathering darta

Gather data: private, ask permission
Data preparation: format the data
Darta Wrangling: masage your data, normalise them, add missing data,
Analyse Data: That might be teh most straighforward part with the tools available on python

You need to understand your data. Are they consistent, are there outliers of wrongly annotated datapoints, are there missing values, or biasses. Are they properly normalised?

Data normalisation or variance stabilisation. HUber et al Bionformatics 2002
Missing values. Simplest solution is to discard them or to impute them (replace the mv by something meaningful). 
Refs doders et al, J. Clin. Epi., 2006 
Feature Selection (in data wrangling). ML usually works better in lower dimentions. If high dimention feature vectors, reduce them removing unecessary features
Turning categorical values into numerical values (ex. red-> 1 , green->0, etc). However, ML can then determine ordering as 1 is closer to 0 tahn 2. ML can then infer unexisting features. hot encoding

[ a a b ] -> ( [ 1 1 0 ] [ 0 0 1 ] ) col a and b, categories are orthogonal now. 

Then tarrget encoding f_c = E(y|c_i = c), it doesnn't blow up feature space as much as hot encoding.

Reproducibility in machine learning. Credibility relies on riproducibility. 

# sklearn

Is the main library that contains many ML tools. It is the standard in the ML comunity. Check Sklearn website. Is good to explain the tools, not why you want to use them. 

we create a naive objet, fit it with data, and then predict, fit, and transform
Data can have a large dynamic range (with few values being very high compared to the other).
Data with high dynamic range (heteroscaleelasticity) are usually log transformed (x + 1) +1 to avoid zero explsion

account for missing values: 
    throuw out anything htat has missing values
    impute all missing values with the mean of all the other rows that aren't 0 In[9]

sklearn expect that you have features in columbs and samles in row, if not transpose for computation and transpose back for output (In[9])

It could be that data is missing because low, hence we can impute it with half of the min value in the row (insetad of the mean) . half of the min, because the min is assumed to be detected. 

----------------

Data visualisation
hist, scatterplot, 
if data separrate, then feature is significative.


features as matrix NxM with M cinditions and N samples
zero center columns to have varager of feature to 0. 

#techniques
## PCA 
principal component analysis
try to find the direction wlong which the data has the largest variation -> finding heigenvectors and eigenvalues of correlation matrix

in low dimentions PCA can fail, Independent component Analysis (ICA) my work better here then. try to find the direction along which dthe data deviates the mosst from a Gaussian distribution. Not recommended for high dimentional data, beacuse too time consuming. 

PCA can be reduction method.

Multidimensional scaling (MDS)
visual reresentation of distances between sets of objects, it can serve as dimension reduction tedhnique for high dimentional data 
t-SNE t-distributed Stodhastic Neighbor Embedding
(Maaten and Hinton, JMLR 2008)
t-SNE perplexity parameter, reflects the number of neighbour datapoints athta are taken into account. 
t-SNE quite slow doesn;t scale well for large dataset
t-SNE for visualisation purpose

UMAP Uniform Manifold Approximation and projection. works better on larger dartaset. 
see Nikolay Oskolkov's blog


#pca exercise
pca doesn t do the scaling in20
important to do scalig before In[24]
in[31] specify the n_components that we want to extract. k


# unsupervised algorimtms
\K-means, clusters data based on distances from k centers of mass. BEcause of random initial interetions can lead to false means, it is better to rerun the random algorims multiple timesk    

PCA orthogonalise the dataset to maximise the meaning of k-means distances, whose space dimentions would not be necessarily orthogonals and thus meaningful.

PCA are ordered so that the most variants are first.
Importat to have normalised data when working with PCA

T-SNE is a dimensiont reduction algorithm. 
Stochastic method. Launch it a couple of time to be sure that the solution is a real minima. 

The siluette score to find the optimal K

there are some data whose complexity cannot be represented in a 2D plot. dd
i


Hierarchical clustering -

siluette score on hierarchical cluseting
2 clusters instead of 3


DBSCAN finds points in the center and then expands to characterise teh tails of the distribution.

algorithm eliminates some points, that then need not to be considered, otherwise we will geet a false good score (they cluser as rejected?)
unsupervised learning, try to understnad structure of data without prior knowledge.



redundant variables might separate class 1 from 2 and 3 and 3 from 1 and 2, making hte classification easier, even if variables are genarally correlated (penguin example)


- recap of 12.06

clustering
-k means
-hierarchical clustering
-sklearn different algorithms comparison based on the data shape


- 19.06

If variable are too correlated I can remove one because they do not add ore info. 


The problem of leaking

scaling on training dataset, and applied to test

expected size of test dataset can be indicatively computed here: https://wnarifin.github.io/ssc/sssnsp.html

penguin exercise on chapter 3, ln ~44 is very insteresting for KNN classifier

- now logistic regression
sklearn classifier comparison, circular versus linear boundary; classifiers 
reacts differently to

stop training when test and training performance align, otherwise overtraining after


logic regression assumes that hthe separation between the two classes is linear
higher teh weight, more steep the transition between classses 

Support verctor machine

best line that separate data is the one with the biggest distance
hypeplanes tha thave the highest marging


- 26/06/23 recap

cross validation with pipeline
knn logistic or svm
you can use the algorithm as an hyperparameter
train n different algorithms and then compare their cross validated metric to see hwch performs teh better, or put them all together( ?)


cancer correction exercise

classifier super powerful, define the methods to use and theirparameters, see the execrise correction for the "cancer correction"

scoring done with roc_auc
roc curve compares cost of error acceptance


#chapter 4
random forest

generate trees that detail teh classification steps

three does not need to scale because features are accounted independently.
the tree can be shown to detail the classification steps.

in the hierachical clustering we are based on the distance, and unsupervised learnig
in decision tree we are doing classification we have a feature that we want to fit, thus supervised learning, the approach is very mecchanical and applies to each feature independely.

random forest: forest is a collection of trees.

