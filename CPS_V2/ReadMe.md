# Adaptive Loss Weight for Cross Pseudo Supervision Loss

## Key Concept:
Main idea: The models are initially not accurate, and even after several epochs, the acuuracy fluctuates significantly. Hence, to use their outputs for cross training, especially when the number of unlabelled data is larger than that of labelled data (which means more iterations of training for unlabelled data), will hinder their learning and may reduce the models validation accuracy. 

Therefore, we propose an adaptive loss weight that multiplies the dice score from the supervised learning in that epoch to scale the unlabelled training loss using the accuracy of the model and hence, to reduce the gradients if the model is inaccurate (and vice-versa). 

Training Flow per epoch: 
  
  Supervised:
  train with labelled data
  use segmentation loss for model A and model B, and cps loss with fixed weight (e.g. 0.02)
  
  Unsupervised:
  train with unlabelled data
  use only cps loss between predictions of model A and model B
  cps loss has a fixed weight (e.g. 0.001), and multiply this with the average dice score from the supervised training in this epoch
  
## Final Accuracy Results:

Testing Method: Test with 48 unseen 3D MRIs, stride (18,18,4)

### Models:
#### A. Baseline (only 16 Labelled 3D MRIs, without CPS)
#### B. CPS (16 Labelled and 64 Unlabelled 3D MRIs)
#### C. CPS with Adaptive Loss Weight (16 Labelled and 64 Unlabelled 3D MRIs)

### Results:

|Model|Dice Accuracy (%)|Jaccard (%)|Average Surface Distance|95 Hausdorff Distance|
|-----|-----|-----|-----|-----|
|A|78.3|69.0|22.5|9.88|
|B|86.4|76.6|3.42|12.1|
|**C**|**88.4**|**79.5**|**2.91**|**9.56**|

Using our proposed Adaptive Weight Loss can increase the final model test accuracy. 

Original Cross Pseudo Supervision Paper: https://arxiv.org/abs/2106.01226
