# Quora Question Similarity Problem

I have used the Siamese Model to solve the above Problem

## Siamese Model
A Siamese network is a neural network which uses the same weights while working in tandem on two different input vectors to compute comparable output vectors.The Siamese network I have implemented looks like this :


<p align="center">
  <img src="https://zhangruochi.com/Question-duplicates/2020/08/23/siamese.png" width="500" height="500">


It is called a Siamese network as both the branches are idenctical and only one set of weights are updated which reflects the changes in both the branches 


One Model uses <b> Triplet Loss </b> while the other model uses <b> Contrastive Loss </b> as the Loss Function
## Triplet Loss

The triplet loss makes use of a baseline (anchor) input that is compared to a positive (truth) input and a negative (false) input. The distance from the baseline (anchor) input to the positive (truth) input is minimized, and the distance from the baseline (anchor) input to the negative (false) input is maximized. In math equations, we are trying to maximize the following. 
  <p align = "center">
  <img src="https://latex.codecogs.com/gif.latex?\large&space;L(A,P,N)=max(||f(A)-f(P)||^{2}-||f(A)-f(N)||^{2}&plus;\alpha,0)" title="\large L(A,P,N)=max(||f(A)-f(P)||^{2}-||f(A)-f(N)||^{2}+\alpha,0)" />
  
  </p>
    
A is the anchor input, for example q11, P the duplicate input, for example, q21, and N the negative input (the non duplicate question), for example q22.  α is a margin; you can think about it as a safety net, or by how much you want to push the duplicates from the non duplicates.

To make this  loss function a bit more effective we will use <b> Hard Negative Mining </b>

#### Hard Negative Mining

We have done Hard Negative Mining to make the Loss function a bit more Effective.
 
 <p align = "center">
<img src="https://latex.codecogs.com/gif.latex?\large&space;Loss_{1}(A,P,N)=max(-cos(A,P)&plus;mean_{neg}&plus;\alpha&space;,0)" title="\large Loss_{1}(A,P,N)=max(-cos(A,P)+mean_{neg}+\alpha ,0)" /> </p>

Loss 1  is defined  as to be the max of the mean negative minus the similarity of A and P plus alpha and 0. The change between the formulas for Triplet Loss and Loss 1 is the replacement of similarity of A and N. With the mean negative, this helps the model converge faster during training by reducing noise. It reduces noise by training on just the average of several observations, rather than training the model on each of these off-diagonal examples.

 <p align = "center">
<img src="https://latex.codecogs.com/gif.latex?\large&space;Loss_{2}(A,P,N)=max(-cos(A,P)&plus;closest_{neg}&plus;\alpha&space;,0)" title="\large Loss_{2}(A,P,N)=max(-cos(A,P)+closest_{neg}+\alpha ,0)" /> </p>

Loss 2 will be the max of the closest negative minus the similarity of A and P plus alpha and 0. The difference between the formulas this time is the replacement of similarity of A and N.With the closest negative, this helps create a slightly larger penalty by diminishing the effects of the otherwise more negative similarity of A and N that it replaces.You can think of the closest negative as finding the negative example that results in the smallest difference between the two cosine similarities. If you had that small difference to alpha, then you're able to generate the largest loss among all of the other examples in that row. By focusing the training on the examples that produce higher loss values, you make the model update its weights more.


 <p align = "center">
<img src="https://latex.codecogs.com/gif.latex?\large&space;Loss(A,P,N)=mean(Loss_{1}&plus;Loss_{2})" title="\large Loss(A,P,N)=mean(Loss_{1}+Loss_{2})" />
</p>

Then the Final Loss would be the mean of these two Losses


## Contrastive Loss

Contrastive Loss uses a Similarity Metric (eg: Distance between the two vectors) ,and tries to make the distance between similar vectors minimum and dissimilar vectors maximum.
In the mathematical equation we are trying to minimize the following:

<p align = "center">
<img src="https://latex.codecogs.com/gif.latex?\large&space;Loss&space;=&space;Y*D^{2}&plus;(1-Y)*max(margin-D,0)^{2}" title="\large Loss = Y*D^{2}+(1-Y)*max(margin-D,0)^{2}" />
 </p>
 
 In the Above Equation the variables are :
 
 Y is the Label, 1 if  similar and 0 if dissimilar.
 D is the Euclidean Distance between the Two Vectors.
 margin is a constant that we use to enforce minimum distance.
