# Quora Question Similarity Problem

I have used the Siamese Model to solve the above Problem

### Siamese Model
A Siamese network is a neural network which uses the same weights while working in tandem on two different input vectors to compute comparable output vectors.The Siamese network I have implemented looks like this :


<p align="center">
  <img src="https://zhangruochi.com/Question-duplicates/2020/08/23/siamese.png" width="500" height="500">


It is called a Siamese network as both the branches are idenctical and only one set of weights are updated which reflects the changes in both the branches 


One Model uses <b> Triplet Loss </b> while the other model uses <b> Contrastive Loss </b> as the Loss Function
### Triplet Loss

The triplet loss makes use of a baseline (anchor) input that is compared to a positive (truthy) input and a negative (falsy) input. The distance from the baseline (anchor) input to the positive (truthy) input is minimized, and the distance from the baseline (anchor) input to the negative (falsy) input is maximized. In math equations, you are trying to maximize the following. 
  <p align = "center">
  <img src="https://latex.codecogs.com/gif.latex?\large&space;L(A,P,N)=max(||f(A)-f(P)||^{2}-||f(A)-f(N)||^{2}&plus;\alpha,0)" title="\large L(A,P,N)=max(||f(A)-f(P)||^{2}-||f(A)-f(N)||^{2}+\alpha,0)" />
  
  </p>
    
A is the anchor input, for example q11, P the duplicate input, for example, q21, and N the negative input (the non duplicate question), for example q22.  α is a margin; you can think about it as a safety net, or by how much you want to push the duplicates from the non duplicates.

To make this  loss function a bit more effective we will use <b> Hard Negative Mining </b>

#### Hard Negative Mining

 <p align = "center">
<img src="https://latex.codecogs.com/gif.latex?\large&space;Loss_{1}(A,P,N)=max(-cos(A,P)&plus;mean_{neg}&plus;\alpha&space;,0)" title="\large Loss_{1}(A,P,N)=max(-cos(A,P)+mean_{neg}+\alpha ,0)" /> </p>


 <p align = "center">
<img src="https://latex.codecogs.com/gif.latex?\large&space;Loss_{2}(A,P,N)=max(-cos(A,P)&plus;closest_{neg}&plus;\alpha&space;,0)" title="\large Loss_{2}(A,P,N)=max(-cos(A,P)+closest_{neg}+\alpha ,0)" /> </p>


 <p align = "center">
<img src="https://latex.codecogs.com/gif.latex?\large&space;Loss(A,P,N)=mean(Loss_{1}&plus;Loss_{2})" title="\large Loss(A,P,N)=mean(Loss_{1}+Loss_{2})" />
</p>

This can be easily explained using an example:

<img src= "https://zhangruochi.com/Question-duplicates/2020/08/23/C3_W4_triploss1.png" width="700" height="400" >
