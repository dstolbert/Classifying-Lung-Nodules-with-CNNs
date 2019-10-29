# Classifying-Lung-Nodules-with-CNNs
Classifying Lung Nodules with CNNs on Lung CT Scans originally from the Cancer Imaging Archives

** If you are looking for a quick overview, I would suggest viewing the 'Demo' and 'Model evaluation' notebooks! These show the final models I developed and give a summary of how I approached the problem. **

### Dataset and problem statement

Can deep learning be used to classify and label nodules in the lungs of patients following a CT scan?

The modelling problem was divided into classifying images with or without nodules; and then labelling positively identified nodules with a second model.


Data were collected form the Luna16 Challenge website https://luna16.grand-challenge.org/ 

### Preprocessing

Data included three-dimensional images moving along the transverse axis of patients. Labeling of nodules was done by four radiologists and included if they were >3mm and accepted by at least 3 of the 4 radiologists. The original dataset contained approximately 118GB of data.

While much work has been done processing these files in three-dimensions, I wanted to determine the viability of simpler two-dimensional convolutional neural networks. Using the annotated nodules, I created two-dimensional patches of each nodule. To create negative patches for classification, I took randomized slices that were offset from the center of nodules in patients who only had one lung nodule. This was done to reduce the likelihood of obtaining a lung nodule in the negative patches.

### Modeling

Both the classification and localization models followed a similar architecture. The models had 3 convolutional/pooling layers, a flatten layer, a dense layer, and a final dense output layer. I also utilized transfer learning with the VGG16 model, a very deep CNN that has performed quite well on the ImageNet dataset. Surprisingly the simpler CNNs outperformed the VGG16 models. 

Data augmentation and dropout were utilized to regularize the models.

### Summary and future steps

The best performing classification model had 90% accuracy and a F1 score of 91% in the test set. The best performing localization model was able to locate nodules with a mean absolute error of 2.5 pixels (roughly 2mm in most images) in the X, Y directions and predict the diameter within 2.4 pixels of what the radiologists had labeled. 

These models could theoretically be used in a data pipeline which divides CT scan images into 65x65 slices. Positively identified slices could then be passed to the localization model and scores could be aggregated to identify the slices with the greatest likelihood of having nodules (similar to a RNN algorithm). While this is feasible, it is likely not the best approach to the problem. 3D CNNs that have been submitted in the Luna16 challenge were able to perform better than the models I have created.

Further improvements with the models I have created may come from increasing the complexity of the architectures and having access to more data.

Please reach out to me if you have any questions on the project!
