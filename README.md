# Improve a Fixed Model the Data-Centric Way!

Link: Walter Reade, Sohier Dane, Ashley Chow. (2023). Improve a Fixed Model the Data-Centric Way!. Kaggle. https://kaggle.com/competitions/playground-series-s3e21

This is my first DS competition on Kaggle. The goal of this contest is clean the dataset for training random forest regressor for molecular oxygen concentration prediction.
We can't change model in this competition, train dataset only. There're some techniques for outlier detection, such as isolation forest, one-class SVM etc. We can clean the data by deleting observed outliers, but some of these points can be real. Moreover, 'woody' models can't extrapolate, so we can't just drop too high and too low values of features and target. We'd like to save most of information from outliers and try to correct wrong values. I suggest using TheilSenRegressor for values correction. We can use weighted sum of original values and regressor predictions for outlier correction. Weights of outliers must be less than inliers. We can make weights of regressor predictions proportional to euclidean distance between original values and predictions. I used this formula:

$w = \frac{(y_{obs} - y_{pred})^2}{(y_{obs} - y_{pred})^2_{0.97}}\times \alpha,$

where $y_{obs}$ - original value; <br>
      $y_{pred}$ - predicted value; <br>
      $\alpha$ - smoothing factor (hyperparameter).
      
Our data contains outliers so it's reasonable to use 97 % percentile as a normalization constant. All weights more 1 must be clipped to 1.
This technique doesn't make significant changes for inliers and correct outlier values.

Corrected values can be calculated using this equation:

$y_{corrected}=y_{pred} \times w + y_{obs} \times (1 - w)$

Full version of my solution is in molecular-oxygen-concentration.ipynb.
