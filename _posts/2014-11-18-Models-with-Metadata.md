---
title: "Models for CRC Diagnosis Using Demographic Data"
author: "Niel"
layout: post
date: "November 19, 2014"
output:
  html_document:
    keep_md: yes
---

In an [earlier post](http://nbaxter13.github.io/2014/11/07/Updated-Models-for-CRC-using-Microbiome-Data.html) I showed models generated using only microbiome data.  In this post, I'm testing models based on patient metadata.


### Healthy vs. Carcinoma
First I'll show the models for distinguishing healthy patients from those with carcinomas.  I ran models containing 1 to 5 risk factors, and looked for the one with the lowest conditional Akaike Information Criteria (AICc). The table below shows the 10 best models with 1 to 5 risk factors. The best model used age, gender, and smoking, all of which are known risk factors for CRC ( [Gender](http://www.cancer.org/cancer/colonandrectumcancer/detailedguide/colorectal-cancer-key-statistics), [Age and Smoking](http://www.cancer.org/cancer/colonandrectumcancer/detailedguide/colorectal-cancer-risk-factors)).


|Model                                     |AICc   |
|:-----------------------------------------|:------|
|Age + Gender + Smoke                      |224.27 |
|Abx + Age + Gender + Smoke                |224.7  |
|Age + Gender + NSAID + Smoke              |224.93 |
|Abx + Age + Gender + NSAID + Smoke        |225.28 |
|Age + Gender + Hx_Fam_CRC + Smoke         |225.34 |
|Abx + Age + Gender + Hx_Fam_CRC + Smoke   |225.95 |
|Age + Gender + Hx_Fam_CRC + NSAID + Smoke |226.06 |
|Age + Diabetes_Med + Gender + Smoke       |226.16 |
|Age + Diabetic + Gender + Smoke           |226.35 |
|Age + BMI + Gender + Smoke                |226.37 |


  

### Healthy vs. Adenoma
Next I did the same thing, but for adenomas.  The best model again used age, gender and smoking, but also included BMI and diabetes medication.  That model was only slightly better than the same model without diabetes medication.  


|Model                                     |AICc   |
|:-----------------------------------------|:------|
|Age + BMI + Diabetes_Med + Gender + Smoke |288.44 |
|Age + BMI + Gender + Smoke                |289.08 |
|Age + BMI + Gender + Race + Smoke         |290.34 |
|Age + BMI + Diabetic + Gender + Smoke     |290.63 |
|Age + BMI + Gender + NSAID + Smoke        |290.97 |
|Abx + Age + BMI + Gender + Smoke          |291.05 |
|Age + BMI + Gender + Hx_Fam_CRC + Smoke   |291.09 |
|Age + BMI + Diabetes_Med + Smoke          |291.15 |
|Age + BMI + Diabetes_Med + Gender         |291.53 |
|Age + BMI + Gender                        |291.77 |

  

### Healthy vs. Lesion
Finally, I tested models for distinguishing healthy individuals from those with any type of lesion (i.e. adenomas OR caricnomas). Like the carcinoma models, the best model for detecting any type of lesion also used age, gender, and smoking.


|Model                                     |AICc   |
|:-----------------------------------------|:------|
|Age + Gender + Smoke                      |365.28 |
|Age + BMI + Gender + Smoke                |365.99 |
|Abx + Age + Gender + Smoke                |366.46 |
|Age + Gender + Hx_Fam_CRC + Smoke         |366.53 |
|Age + Diabetes_Med + Gender + Smoke       |366.81 |
|Age + Diabetic + Gender + Smoke           |367.18 |
|Abx + Age + BMI + Gender + Smoke          |367.19 |
|Age + BMI + Gender + Hx_Fam_CRC + Smoke   |367.22 |
|Age + Gender + NSAID + Smoke              |367.26 |
|Age + BMI + Diabetes_Med + Gender + Smoke |367.46 |


### ROC Curves Using Metadata Models
Next, I wanted to use these models to generate ROC curves.  Since all of the best models use age, gender, and smoking, I decided to only use those three parameters in the models. (Note: I did also try the optimal adenoma model that included BMI and diabetes meds, but the AUC of the ROC curve wasn't significantly different from the model with just age, gender, and smoking)

![center](/../figs/2014-11-18-Models-with-Metadata/roc-1.png) 

UPDATE: As Pat requested, I've added the model for Adenoma vs. Carcinoma

####Conclusion
Surprisingly, all three comparison have very similar ROC curves despite their different AICc values.  I suspect that's due to each having a different n. Now that I have models using patient metadata, my next step will be to combine them with the microbiome data and FIT results. Considering how good these models and the OTU models were, I expect have very good models when I combine them all together.



