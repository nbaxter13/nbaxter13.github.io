---
title: "Combined Models for CRC Diagnosis"
author: "Niel"
layout: post
date: "December 10, 2014"
output:
  html_document:
    keep_md: yes
---

Now that I've developed models based on the [microbiome](http://nbaxter13.github.io/2014/11/07/Updated-Models-for-CRC-using-Microbiome-Data.html) and [patient metadata](http://nbaxter13.github.io/2014/11/19/Models-with-Metadata.html), I'll combine them into models with FIT results, and compare them to FIT alone.



<img src="/../figs/carcinoma-1.png" title="center" alt="center" style="display: block; margin: auto;" />

The figure above shows ROC curves for 4 different models for distinguishing normal individuals from those with carcinomas.  The "Microbiome" model uses the relative abundance of 10 OTUs. The "Metadata" model uses the patients' age, gender, and whether they smoke.  The "FIT" model uses the **F**ecal **I**mmunochemical **T**est results.  The "Combined" model is all three tests combined into a single model.  Although FIT is better than using only microbiome data or metadata, combining all three is significantly better than using FIT alone (p=0.00661).

<img src="/../figs/adenoma-1.png" title="center" alt="center" style="display: block; margin: auto;" />

The plot above shows the models for normal vs adenoma.  Again the combined model is significantly better than FIT alone (p=3.9e-17).  This time however, the models with only the microbiome or only metadata are significantly better than FIT alone (p=0.00082, 0.0032)




<img src="/../figs/lesion-1.png" title="center" alt="center" style="display: block; margin: auto;" />

The last plot is for distinguishing normal from either type of lesion (adenoma or carcinoma).  Models with FIT, microbiome data, or metadata alone perform similarly.  Combining tests is significantly better than any one test by itself (p<1e-10 for all three comparisons).


