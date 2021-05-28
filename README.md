# Physical-exercise
<html>

<head>
Predicting the level of exercise quality
</head>

<body>

<p>Loads the necessary packages:</p>

<div class="chunk" id="unnamed-chunk-1"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(data.table)</span>
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'data.table' was built under R version 4.0.4
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(caret)</span>
</pre></div>
<div class="message"><pre class="knitr r">## Loading required package: lattice
</pre></div>
<div class="message"><pre class="knitr r">## Loading required package: ggplot2
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'ggplot2' was built under R version 4.0.3
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(e1071)</span>
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'e1071' was built under R version 4.0.5
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(ggplot2)</span>
<span class="hl kwd">library</span><span class="hl std">(corrplot)</span>
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'corrplot' was built under R version 4.0.5
</pre></div>
<div class="message"><pre class="knitr r">## corrplot 0.88 loaded
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(dplyr)</span>
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'dplyr' was built under R version 4.0.4
</pre></div>
<div class="message"><pre class="knitr r">## 
## Attaching package: 'dplyr'
</pre></div>
<div class="message"><pre class="knitr r">## The following objects are masked from 'package:data.table':
## 
##     between, first, last
</pre></div>
<div class="message"><pre class="knitr r">## The following objects are masked from 'package:stats':
## 
##     filter, lag
</pre></div>
<div class="message"><pre class="knitr r">## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(</span><span class="hl str">'randomForest'</span><span class="hl std">)</span>
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'randomForest' was built under R version 4.0.3
</pre></div>
<div class="message"><pre class="knitr r">## randomForest 4.6-14
</pre></div>
<div class="message"><pre class="knitr r">## Type rfNews() to see new features/changes/bug fixes.
</pre></div>
<div class="message"><pre class="knitr r">## 
## Attaching package: 'randomForest'
</pre></div>
<div class="message"><pre class="knitr r">## The following object is masked from 'package:dplyr':
## 
##     combine
</pre></div>
<div class="message"><pre class="knitr r">## The following object is masked from 'package:ggplot2':
## 
##     margin
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(rpart)</span>
<span class="hl kwd">library</span><span class="hl std">(rpart.plot)</span>
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'rpart.plot' was built under R version 4.0.5
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(rattle)</span>
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'rattle' was built under R version 4.0.5
</pre></div>
<div class="message"><pre class="knitr r">## Loading required package: tibble
</pre></div>
<div class="warning"><pre class="knitr r">## Warning: package 'tibble' was built under R version 4.0.5
</pre></div>
<div class="message"><pre class="knitr r">## Loading required package: bitops
</pre></div>
<div class="message"><pre class="knitr r">## Rattle: A free graphical interface for data science with R.
## Version 5.4.0 Copyright (c) 2006-2020 Togaware Pty Ltd.
## Geben Sie 'rattle()' ein, um Ihre Daten mischen.
</pre></div>
<div class="message"><pre class="knitr r">## 
## Attaching package: 'rattle'
</pre></div>
<div class="message"><pre class="knitr r">## The following object is masked from 'package:randomForest':
## 
##     importance
</pre></div>
</div></div>

<p>loads the data</p>

<div class="chunk" id="unnamed-chunk-2"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">setwd</span><span class="hl std">(</span><span class="hl str">&quot;I://fbs//ML//assignment&quot;</span><span class="hl std">)</span>
<span class="hl std">traindb</span><span class="hl kwb">&lt;-</span><span class="hl kwd">read.csv2</span><span class="hl std">(</span><span class="hl kwc">file</span><span class="hl std">=</span><span class="hl str">&quot;pml-training.csv&quot;</span><span class="hl std">,</span> <span class="hl kwc">header</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">,</span> <span class="hl kwc">dec</span><span class="hl std">=</span><span class="hl str">&quot;.&quot;</span><span class="hl std">,</span> <span class="hl kwc">sep</span><span class="hl std">=</span><span class="hl str">&quot;,&quot;</span><span class="hl std">,</span> <span class="hl kwc">na.strings</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;NA&quot;</span><span class="hl std">,</span> <span class="hl num">NA</span> <span class="hl std">,</span><span class="hl str">&quot;&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;#DIV/0!&quot;</span><span class="hl std">))</span>
<span class="hl std">testdb</span><span class="hl kwb">&lt;-</span><span class="hl kwd">read.table</span><span class="hl std">(</span><span class="hl kwc">file</span><span class="hl std">=</span><span class="hl str">&quot;pml-testing.csv&quot;</span><span class="hl std">,</span><span class="hl kwc">header</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">,</span> <span class="hl kwc">dec</span><span class="hl std">=</span><span class="hl str">&quot;.&quot;</span><span class="hl std">,</span> <span class="hl kwc">sep</span><span class="hl std">=</span><span class="hl str">&quot;,&quot;</span><span class="hl std">)</span>
</pre></div>
</div></div>

<p>removes the first 7th columns without much information and remove the columns with too many NAs</p>
<div class="chunk" id="unnamed-chunk-3"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">traindb</span><span class="hl kwb">&lt;-</span><span class="hl std">traindb[,</span><span class="hl opt">-</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl opt">:</span><span class="hl num">7</span><span class="hl std">)]</span>
<span class="hl std">data2</span><span class="hl kwb">&lt;-</span><span class="hl std">traindb[,</span> <span class="hl opt">-</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">names</span><span class="hl std">(traindb)</span> <span class="hl opt">%in%</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;kurtosis_roll_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_picth_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;skewness_roll_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_roll_belt.1&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;max_roll_belt&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;max_picth_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;max_yaw_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;min_roll_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;min_pitch_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;min_yaw_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_roll_belt&quot;</span><span class="hl std">,</span>    <span class="hl str">&quot;amplitude_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;amplitude_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_total_accel_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;avg_roll_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_roll_belt&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;var_roll_belt&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;avg_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_pitch_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;avg_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_yaw_belt&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;var_yaw_belt&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;avg_pitch_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;avg_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_yaw_belt&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;avg_pitch_arm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;stddev_pitch_arm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;var_pitch_arm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;avg_yaw_arm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_yaw_arm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_yaw_arm&quot;</span> <span class="hl std">,</span>
                                              <span class="hl str">&quot;skewness_pitch_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;max_roll_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;max_picth_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;max_yaw_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;min_roll_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;min_pitch_forearm&quot;</span> <span class="hl std">,</span>
                                              <span class="hl str">&quot;kurtosis_roll_forearm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;kurtosis_picth_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;skewness_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_roll_forearm&quot;</span><span class="hl std">,</span>   <span class="hl str">&quot;kurtosis_picth_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_roll_forearm&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;avg_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_yaw_forearm&quot;</span> <span class="hl std">,</span>  <span class="hl str">&quot;var_yaw_forearm&quot;</span> <span class="hl std">,</span>
                                              <span class="hl str">&quot;avg_roll_forearm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;stddev_roll_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_roll_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;avg_pitch_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;stddev_pitch_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;var_pitch_forearm&quot;</span> <span class="hl std">,</span>
                                              <span class="hl str">&quot;kurtosis_roll_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_picth_arm&quot;</span> <span class="hl std">,</span>  <span class="hl str">&quot;kurtosis_yaw_arm&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;vg_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_roll_forearm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;var_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;avg_pitch_forearm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;stddev_pitch_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_pitch_forearm&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;skewness_roll_arm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;skewness_pitch_arm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_roll_arm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;max_picth_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_yaw_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_accel_forearm&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;min_roll_arm&quot;</span><span class="hl std">,</span>   <span class="hl str">&quot;min_pitch_arm&quot;</span><span class="hl std">,</span>      <span class="hl str">&quot;min_yaw_arm&quot;</span><span class="hl std">,</span>    <span class="hl str">&quot;amplitude_roll_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_pitch_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_yaw_arm&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;kurtosis_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_picth_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_roll_dumbbell&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;max_picth_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;min_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;min_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;min_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_yaw_dumbbell&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;min_yaw_forearm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;amplitude_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_pitch_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_yaw_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_dumbbell&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;kurtosis_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_accel_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;avg_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_roll_dumbbell&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;avg_pitch_dumbbell&quot;</span><span class="hl std">,</span>
                                              <span class="hl str">&quot;stddev_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;avg_yaw_dumbbell&quot;</span><span class="hl std">,</span>   <span class="hl str">&quot;stddev_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_accel_arm&quot;</span><span class="hl std">,</span>     <span class="hl str">&quot;avg_roll_arm&quot;</span><span class="hl std">,</span>     <span class="hl str">&quot;stddev_roll_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_roll_arm&quot;</span>  <span class="hl std">))]</span>
</pre></div>
</div></div>

<p>check the distribution of the outcome variable: more A than other classes, maybe a weigtht will have to be considered
to take this bias into account. The correlation table of the remaining variables suggests that many features are correlated. A PCA could be considered.
Let us check with the trees which features are the most relevant to predict the quality of exercises</p>
<div class="chunk" id="unnamed-chunk-4"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">table</span><span class="hl std">(data2</span><span class="hl opt">$</span><span class="hl std">classe)</span>
</pre></div>
<div class="output"><pre class="knitr r">## 
##    A    B    C    D    E 
## 5580 3797 3422 3216 3607
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">dbnum</span><span class="hl kwb">&lt;-</span><span class="hl std">data2</span> <span class="hl opt">%&gt;%</span> <span class="hl std">dplyr</span><span class="hl opt">::</span><span class="hl kwd">select</span><span class="hl std">(</span><span class="hl kwd">where</span><span class="hl std">(is.numeric))</span>
<span class="hl std">corrMat</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">cor</span><span class="hl std">(dbnum)</span>
<span class="hl kwd">corrplot</span><span class="hl std">(corrMat,</span> <span class="hl kwc">method</span> <span class="hl std">=</span> <span class="hl str">&quot;color&quot;</span><span class="hl std">,</span> <span class="hl kwc">type</span> <span class="hl std">=</span> <span class="hl str">&quot;lower&quot;</span><span class="hl std">)</span>
</pre></div>
</div><div class="rimage default"><img src="figure/unnamed-chunk-4-1.png" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" class="plot" /></div></div>

<p>Splits the sample into a testing and training subsets in order to validate on the 20 unlabelled, applies a 60 vs 40% ratio between training and testing subsets<p>
<div class="chunk" id="unnamed-chunk-5"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">trainIndex</span> <span class="hl kwb">=</span> <span class="hl kwd">createDataPartition</span><span class="hl std">(data2</span><span class="hl opt">$</span><span class="hl std">classe,</span> <span class="hl kwc">p</span> <span class="hl std">=</span> <span class="hl num">0.60</span><span class="hl std">,</span><span class="hl kwc">list</span><span class="hl std">=</span><span class="hl num">FALSE</span><span class="hl std">)</span>
<span class="hl std">trainingset</span> <span class="hl kwb">=</span> <span class="hl std">data2[trainIndex,]</span>
<span class="hl std">testingset</span> <span class="hl kwb">=</span> <span class="hl std">data2[</span><span class="hl opt">-</span><span class="hl std">trainIndex,]</span>
<span class="hl std">trainingset</span><span class="hl opt">$</span><span class="hl std">classe</span><span class="hl kwb">&lt;-</span><span class="hl kwd">as.factor</span><span class="hl std">(</span><span class="hl kwd">as.character</span><span class="hl std">(trainingset</span><span class="hl opt">$</span><span class="hl std">classe))</span>

<span class="hl kwd">set.seed</span><span class="hl std">(</span><span class="hl num">12345</span><span class="hl std">)</span>
</pre></div>
</div></div>


<p> exploring with rpart and tree <p>
<div class="chunk" id="unnamed-chunk-6"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">model</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">rpart</span><span class="hl std">(classe</span> <span class="hl opt">~</span><span class="hl std">.,</span> <span class="hl kwc">data</span> <span class="hl std">= trainingset)</span>
<span class="hl std">model</span>
</pre></div>
<div class="output"><pre class="knitr r">## n= 11776 
## 
## node), split, n, loss, yval, (yprob)
##       * denotes terminal node
## 
##     1) root 11776 8428 A (0.28 0.19 0.17 0.16 0.18)  
##       2) roll_belt&lt; 130.5 10793 7454 A (0.31 0.21 0.19 0.18 0.11)  
##         4) pitch_forearm&lt; -33.95 946    6 A (0.99 0.0063 0 0 0) *
##         5) pitch_forearm&gt;=-33.95 9847 7448 A (0.24 0.23 0.21 0.2 0.12)  
##          10) magnet_dumbbell_y&lt; 438.5 8311 5960 A (0.28 0.18 0.24 0.19 0.11)  
##            20) roll_forearm&lt; 124.5 5165 3065 A (0.41 0.18 0.18 0.17 0.061)  
##              40) magnet_dumbbell_z&lt; -25.5 1820  605 A (0.67 0.21 0.014 0.077 0.03)  
##                80) roll_forearm&gt;=-136.5 1529  345 A (0.77 0.18 0.016 0.029 0.0059) *
##                81) roll_forearm&lt; -136.5 291  175 B (0.11 0.4 0.0069 0.33 0.16) *
##              41) magnet_dumbbell_z&gt;=-25.5 3345 2428 C (0.26 0.16 0.27 0.22 0.078)  
##                82) accel_dumbbell_y&gt;=-40.5 2937 2054 A (0.3 0.18 0.19 0.24 0.084)  
##                 164) yaw_belt&gt;=169.5 384   49 A (0.87 0.055 0 0.073 0) *
##                 165) yaw_belt&lt; 169.5 2553 1862 D (0.21 0.2 0.21 0.27 0.097)  
##                   330) pitch_belt&lt; -42.95 302   56 B (0.023 0.81 0.086 0.05 0.026) *
##                   331) pitch_belt&gt;=-42.95 2251 1575 D (0.24 0.12 0.23 0.3 0.11)  
##                     662) roll_belt&gt;=125.5 511  211 C (0.37 0.029 0.59 0.014 0.0039)  
##                      1324) magnet_belt_z&lt; -326.5 156    3 A (0.98 0 0.0064 0 0.013) *
##                      1325) magnet_belt_z&gt;=-326.5 355   56 C (0.096 0.042 0.84 0.02 0) *
##                     663) roll_belt&lt; 125.5 1740 1071 D (0.2 0.15 0.13 0.38 0.14)  
##                      1326) yaw_belt&lt; -85.4 1100  833 A (0.24 0.21 0.14 0.22 0.19)  
##                        2652) accel_dumbbell_z&lt; 25.5 667  416 A (0.38 0.14 0.22 0.23 0.03)  
##                          5304) yaw_forearm&gt;=-95.15 502  251 A (0.5 0.18 0.23 0.056 0.028)  
##                           10608) magnet_forearm_z&gt;=-158 317   72 A (0.77 0.13 0.035 0.041 0.025) *
##                           10609) magnet_forearm_z&lt; -158 185   79 C (0.032 0.28 0.57 0.081 0.032) *
##                          5305) yaw_forearm&lt; -95.15 165   37 D (0 0.024 0.16 0.78 0.036) *
##                        2653) accel_dumbbell_z&gt;=25.5 433  241 E (0.037 0.31 0.012 0.2 0.44)  
##                          5306) roll_dumbbell&lt; 36.70066 117   19 B (0.026 0.84 0.043 0.017 0.077) *
##                          5307) roll_dumbbell&gt;=36.70066 316  133 E (0.041 0.11 0 0.27 0.58) *
##                      1327) yaw_belt&gt;=-85.4 640  213 D (0.14 0.045 0.11 0.67 0.039) *
##                83) accel_dumbbell_y&lt; -40.5 408   38 C (0.0049 0.02 0.91 0.032 0.037) *
##            21) roll_forearm&gt;=124.5 3146 2098 C (0.08 0.17 0.33 0.23 0.18)  
##              42) magnet_dumbbell_y&lt; 290.5 1830  931 C (0.097 0.13 0.49 0.15 0.14)  
##                84) magnet_forearm_z&lt; -245.5 142   27 A (0.81 0.056 0 0.07 0.063) *
##                85) magnet_forearm_z&gt;=-245.5 1688  789 C (0.037 0.13 0.53 0.15 0.15) *
##              43) magnet_dumbbell_y&gt;=290.5 1316  863 D (0.055 0.24 0.11 0.34 0.25)  
##                86) accel_forearm_x&gt;=-105.5 849  569 E (0.046 0.31 0.16 0.15 0.33)  
##                 172) roll_dumbbell&lt; 40.15117 173   33 B (0.052 0.81 0.012 0.04 0.087) *
##                 173) roll_dumbbell&gt;=40.15117 676  411 E (0.044 0.19 0.2 0.18 0.39) *
##                87) accel_forearm_x&lt; -105.5 467  140 D (0.073 0.11 0.026 0.7 0.092) *
##          11) magnet_dumbbell_y&gt;=438.5 1536  743 B (0.031 0.52 0.041 0.22 0.19)  
##            22) total_accel_dumbbell&gt;=5.5 1099  376 B (0.044 0.66 0.056 0.019 0.22)  
##              44) roll_belt&gt;=-0.58 930  207 B (0.052 0.78 0.066 0.023 0.083) *
##              45) roll_belt&lt; -0.58 169    0 E (0 0 0 0 1) *
##            23) total_accel_dumbbell&lt; 5.5 437  120 D (0 0.16 0.0046 0.73 0.11) *
##       3) roll_belt&gt;=130.5 983    9 E (0.0092 0 0 0 0.99) *
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">model</span><span class="hl opt">$</span><span class="hl std">variable.importance</span>
</pre></div>
<div class="output"><pre class="knitr r">##            roll_belt           pitch_belt         accel_belt_z 
##          1623.237934           900.037924           826.686568 
##        pitch_forearm     accel_dumbbell_y    magnet_dumbbell_y 
##           758.021478           635.093849           632.855463 
##             yaw_belt         roll_forearm total_accel_dumbbell 
##           610.565850           601.332480           530.234481 
##     total_accel_belt        roll_dumbbell    magnet_dumbbell_z 
##           512.319955           491.733230           469.477863 
##         accel_belt_y        magnet_belt_z      accel_forearm_x 
##           445.457566           439.410861           423.981426 
##     accel_dumbbell_x     magnet_forearm_z        magnet_belt_x 
##           290.576146           279.186915           239.129122 
##         accel_belt_x        magnet_belt_y     magnet_forearm_y 
##           232.893299           230.238478           228.668874 
##      accel_forearm_z      accel_forearm_y          yaw_forearm 
##           185.693713           163.405847           151.199064 
##     accel_dumbbell_z         yaw_dumbbell         gyros_belt_z 
##           142.133799           138.619920           119.774435 
##       pitch_dumbbell              yaw_arm     magnet_forearm_x 
##           101.883757            95.801662            84.417811 
##    magnet_dumbbell_x     gyros_dumbbell_x         gyros_belt_y 
##            80.773502            62.704166            39.349289 
##          gyros_arm_x            pitch_arm          gyros_arm_y 
##            38.934588            38.471163            29.713238 
##     gyros_dumbbell_y          accel_arm_z         magnet_arm_y 
##            19.161209             8.166739             8.115700 
##         magnet_arm_x 
##             6.639065
</pre></div>
</div></div>

<div class="chunk" id="unnamed-chunk-7"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">plot</span><span class="hl std">(model)</span>
<span class="hl kwd">text</span><span class="hl std">(model,</span> <span class="hl kwc">digits</span> <span class="hl std">=</span> <span class="hl num">3</span><span class="hl std">)</span>
</pre></div>
</div><div class="rimage default"><img src="figure/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" class="plot" /></div></div>


<p>caret -- plots the main features delineating the classes. Check of the prediction accuracy and prediction on the validating sample without labels.
Almost 50perc accuracy overall, performances differ across classes, better on A and E. Maybe introducing a weight would help in reducing the bias coming
from the overrepresentation of labels A in the sample.
<p>
<div class="chunk" id="unnamed-chunk-8"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">modFit</span><span class="hl kwb">&lt;-</span><span class="hl kwd">train</span><span class="hl std">(classe</span> <span class="hl opt">~</span> <span class="hl std">.,</span> <span class="hl kwc">method</span><span class="hl std">=</span><span class="hl str">&quot;rpart&quot;</span><span class="hl std">,</span> <span class="hl kwc">data</span><span class="hl std">=trainingset)</span>
<span class="hl kwd">print</span><span class="hl std">(modFit</span><span class="hl opt">$</span><span class="hl std">finalModel)</span>
</pre></div>
<div class="output"><pre class="knitr r">## n= 11776 
## 
## node), split, n, loss, yval, (yprob)
##       * denotes terminal node
## 
##  1) root 11776 8428 A (0.28 0.19 0.17 0.16 0.18)  
##    2) roll_belt&lt; 130.5 10793 7454 A (0.31 0.21 0.19 0.18 0.11)  
##      4) pitch_forearm&lt; -33.95 946    6 A (0.99 0.0063 0 0 0) *
##      5) pitch_forearm&gt;=-33.95 9847 7448 A (0.24 0.23 0.21 0.2 0.12)  
##       10) magnet_dumbbell_y&lt; 438.5 8311 5960 A (0.28 0.18 0.24 0.19 0.11)  
##         20) roll_forearm&lt; 124.5 5165 3065 A (0.41 0.18 0.18 0.17 0.061) *
##         21) roll_forearm&gt;=124.5 3146 2098 C (0.08 0.17 0.33 0.23 0.18) *
##       11) magnet_dumbbell_y&gt;=438.5 1536  743 B (0.031 0.52 0.041 0.22 0.19) *
##    3) roll_belt&gt;=130.5 983    9 E (0.0092 0 0 0 0.99) *
</pre></div>
</div></div>

<div class="chunk" id="unnamed-chunk-9"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">fancyRpartPlot</span><span class="hl std">(modFit</span><span class="hl opt">$</span><span class="hl std">finalModel,</span> <span class="hl kwc">cex</span><span class="hl std">=</span><span class="hl num">.6</span><span class="hl std">)</span>
</pre></div>
</div><div class="rimage default"><img src="figure/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" class="plot" /></div></div>

<div class="chunk" id="unnamed-chunk-10"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">predm1</span><span class="hl kwb">&lt;-</span><span class="hl kwd">predict</span><span class="hl std">(modFit,</span> <span class="hl kwc">newdata</span><span class="hl std">=testingset)</span>
<span class="hl std">testingset</span><span class="hl opt">$</span><span class="hl std">classe</span><span class="hl kwb">&lt;-</span><span class="hl kwd">as.factor</span><span class="hl std">(</span><span class="hl kwd">as.character</span><span class="hl std">(testingset</span><span class="hl opt">$</span><span class="hl std">classe))</span>
<span class="hl std">Cfit</span><span class="hl kwb">&lt;-</span><span class="hl kwd">confusionMatrix</span><span class="hl std">(predm1, testingset</span><span class="hl opt">$</span><span class="hl std">classe)</span>
<span class="hl kwd">print</span><span class="hl std">(Cfit)</span>
</pre></div>
<div class="output"><pre class="knitr r">## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2040  647  649  582  206
##          B   39  500   46  234  196
##          C  148  371  673  470  383
##          D    0    0    0    0    0
##          E    5    0    0    0  657
## 
## Overall Statistics
##                                           
##                Accuracy : 0.4932          
##                  95% CI : (0.4821, 0.5044)
##     No Information Rate : 0.2845          
##     P-Value [Acc &gt; NIR] : &lt; 2.2e-16       
##                                           
##                   Kappa : 0.3371          
##                                           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9140  0.32938  0.49196   0.0000  0.45562
## Specificity            0.6288  0.91862  0.78821   1.0000  0.99922
## Pos Pred Value         0.4947  0.49261  0.32910      NaN  0.99245
## Neg Pred Value         0.9484  0.85097  0.88019   0.8361  0.89073
## Prevalence             0.2845  0.19347  0.17436   0.1639  0.18379
## Detection Rate         0.2600  0.06373  0.08578   0.0000  0.08374
## Detection Prevalence   0.5256  0.12937  0.26064   0.0000  0.08437
## Balanced Accuracy      0.7714  0.62400  0.64008   0.5000  0.72742
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">res1</span><span class="hl kwb">&lt;-</span> <span class="hl std">Cfit</span><span class="hl opt">$</span><span class="hl std">overall[</span><span class="hl num">1</span><span class="hl std">]</span>
<span class="hl std">res1</span>
</pre></div>
<div class="output"><pre class="knitr r">## Accuracy 
## 0.493245
</pre></div>
</div></div>


<p>Compares the accuracy of the previous predictors and of the predictions with a random forest.
With almost 99.5% accuracy, another algorithm is less likely to provide better results.
Overall, the analysis shows that random forest performs a better job in delineating the features that represent the best predictors for the quality of the exercise.
roll_belt, pitch_belt,yaw_belt, magnet_dumbbell_y, magnet_dumbbell_z, roll_forearm, pitch_forearm are the most important predictors based on Random forest (see the graph aside named VarImp_RF).<p>
<div class="chunk" id="unnamed-chunk-11"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">modelRF</span><span class="hl kwb">=</span><span class="hl kwd">randomForest</span><span class="hl std">(classe</span><span class="hl opt">~</span><span class="hl std">.,</span> <span class="hl kwc">data</span><span class="hl std">=trainingset,</span> <span class="hl kwc">method</span><span class="hl std">=</span><span class="hl str">'class'</span><span class="hl std">)</span>
<span class="hl std">predRF</span><span class="hl kwb">=</span><span class="hl kwd">predict</span><span class="hl std">(modelRF,testingset,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">'class'</span><span class="hl std">)</span>
<span class="hl std">CitRF</span><span class="hl kwb">=</span><span class="hl kwd">confusionMatrix</span><span class="hl std">(predRF,testingset</span><span class="hl opt">$</span><span class="hl std">classe)</span>
<span class="hl std">resRF</span><span class="hl kwb">&lt;-</span> <span class="hl std">CitRF</span><span class="hl opt">$</span><span class="hl std">overall[</span><span class="hl num">1</span><span class="hl std">]</span>
<span class="hl std">resRF</span>
</pre></div>
<div class="output"><pre class="knitr r">##  Accuracy 
## 0.9951568
</pre></div>
</div></div>


<p>As a robustness checks, GBMs estimations are compared. They will continue improving to minimize all errors as well but can overemphasize outliers and cause overfitting. Cross-validation (here 10 folds with 3 repeated samples) is used to neutralize overfitting. 1,000 trees minimum and additional potential hyperparameters.</p>

<div class="chunk" id="unnamed-chunk-12"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">fitControl</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">trainControl</span><span class="hl std">(</span>
  <span class="hl kwc">method</span> <span class="hl std">=</span> <span class="hl str">&quot;repeatedcv&quot;</span><span class="hl std">,</span>
  <span class="hl kwc">number</span> <span class="hl std">=</span> <span class="hl num">10</span><span class="hl std">,</span>
  <span class="hl kwc">repeats</span> <span class="hl std">=</span><span class="hl num">3</span><span class="hl std">)</span>
<span class="hl std">GBM</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">train</span><span class="hl std">(classe</span> <span class="hl opt">~</span> <span class="hl std">.,</span> <span class="hl kwc">data</span> <span class="hl std">= trainingset,</span> <span class="hl kwc">method</span> <span class="hl std">=</span> <span class="hl str">&quot;gbm&quot;</span><span class="hl std">,</span> <span class="hl kwc">verbose</span> <span class="hl std">=</span> <span class="hl num">FALSE</span><span class="hl std">,</span> <span class="hl kwc">trControl</span><span class="hl std">=fitControl)</span>
<span class="hl std">GBM</span><span class="hl opt">$</span><span class="hl std">finalModel</span>
</pre></div>
<div class="output"><pre class="knitr r">## A gradient boosted model with multinomial loss function.
## 150 iterations were performed.
## There were 52 predictors of which 52 had non-zero influence.
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">testingset</span><span class="hl opt">$</span><span class="hl std">classe</span><span class="hl kwb">&lt;-</span><span class="hl kwd">as.factor</span><span class="hl std">(</span><span class="hl kwd">as.character</span><span class="hl std">(testingset</span><span class="hl opt">$</span><span class="hl std">classe))</span>


<span class="hl std">predGBM</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">predict</span><span class="hl std">(GBM, testingset)</span>
<span class="hl std">cfitGBM</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">confusionMatrix</span><span class="hl std">(predGBM, testingset</span><span class="hl opt">$</span><span class="hl std">classe)</span>
<span class="hl std">cfitGBM</span>
</pre></div>
<div class="output"><pre class="knitr r">## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2196   35    1    3    1
##          B   19 1431   32    2   20
##          C   13   47 1309   40   21
##          D    3    0   22 1234   10
##          E    1    5    4    7 1390
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9635          
##                  95% CI : (0.9592, 0.9676)
##     No Information Rate : 0.2845          
##     P-Value [Acc &gt; NIR] : &lt; 2.2e-16       
##                                           
##                   Kappa : 0.9539          
##                                           
##  Mcnemar's Test P-Value : 1.325e-06       
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9839   0.9427   0.9569   0.9596   0.9639
## Specificity            0.9929   0.9885   0.9813   0.9947   0.9973
## Pos Pred Value         0.9821   0.9515   0.9154   0.9724   0.9879
## Neg Pred Value         0.9936   0.9863   0.9908   0.9921   0.9919
## Prevalence             0.2845   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2799   0.1824   0.1668   0.1573   0.1772
## Detection Prevalence   0.2850   0.1917   0.1823   0.1617   0.1793
## Balanced Accuracy      0.9884   0.9656   0.9691   0.9771   0.9806
</pre></div>
</div></div>

<p>Random forests giving the most accurate prediction with an out of sample prediction of less than 1%, I use this model trained on the subset to predict the unlabelled data.</p>


<p>drops the variables in this subset to fit the prediction estimated with the training sample.</p>
<div class="chunk" id="unnamed-chunk-13"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">testdb</span><span class="hl kwb">&lt;-</span><span class="hl std">testdb[,</span><span class="hl opt">-</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl opt">:</span><span class="hl num">7</span><span class="hl std">)]</span>
<span class="hl std">testdb</span><span class="hl kwb">&lt;-</span><span class="hl std">testdb[,</span> <span class="hl opt">-</span><span class="hl kwd">which</span><span class="hl std">(</span><span class="hl kwd">names</span><span class="hl std">(testdb)</span> <span class="hl opt">%in%</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;kurtosis_roll_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_picth_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;skewness_roll_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_roll_belt.1&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;max_roll_belt&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;max_picth_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;max_yaw_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;min_roll_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;min_pitch_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;min_yaw_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_roll_belt&quot;</span><span class="hl std">,</span>    <span class="hl str">&quot;amplitude_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;amplitude_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_total_accel_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;avg_roll_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_roll_belt&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;var_roll_belt&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;avg_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_pitch_belt&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;avg_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_yaw_belt&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;var_yaw_belt&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;avg_pitch_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_pitch_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;avg_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_yaw_belt&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_yaw_belt&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;avg_pitch_arm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;stddev_pitch_arm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;var_pitch_arm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;avg_yaw_arm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;stddev_yaw_arm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_yaw_arm&quot;</span> <span class="hl std">,</span>
                                             <span class="hl str">&quot;skewness_pitch_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;max_roll_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;max_picth_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;max_yaw_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;min_roll_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;min_pitch_forearm&quot;</span> <span class="hl std">,</span>
                                             <span class="hl str">&quot;kurtosis_roll_forearm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;kurtosis_picth_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;skewness_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_roll_forearm&quot;</span><span class="hl std">,</span>   <span class="hl str">&quot;kurtosis_picth_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_roll_forearm&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;avg_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_yaw_forearm&quot;</span> <span class="hl std">,</span>  <span class="hl str">&quot;var_yaw_forearm&quot;</span> <span class="hl std">,</span>
                                             <span class="hl str">&quot;avg_roll_forearm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;stddev_roll_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;var_roll_forearm&quot;</span><span class="hl std">,</span><span class="hl str">&quot;avg_pitch_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;stddev_pitch_forearm&quot;</span> <span class="hl std">,</span><span class="hl str">&quot;var_pitch_forearm&quot;</span> <span class="hl std">,</span>
                                             <span class="hl str">&quot;kurtosis_roll_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_picth_arm&quot;</span> <span class="hl std">,</span>  <span class="hl str">&quot;kurtosis_yaw_arm&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;vg_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_roll_forearm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;var_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;avg_pitch_forearm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;stddev_pitch_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_pitch_forearm&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;skewness_roll_arm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;skewness_pitch_arm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_roll_arm&quot;</span> <span class="hl std">,</span> <span class="hl str">&quot;max_picth_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_yaw_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_accel_forearm&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;min_roll_arm&quot;</span><span class="hl std">,</span>   <span class="hl str">&quot;min_pitch_arm&quot;</span><span class="hl std">,</span>      <span class="hl str">&quot;min_yaw_arm&quot;</span><span class="hl std">,</span>    <span class="hl str">&quot;amplitude_roll_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_pitch_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_yaw_arm&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;kurtosis_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_picth_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_roll_dumbbell&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;max_picth_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;max_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;min_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;min_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;min_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_yaw_dumbbell&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;min_yaw_forearm&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;amplitude_roll_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_pitch_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;amplitude_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_yaw_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_belt&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;kurtosis_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_dumbbell&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;kurtosis_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;skewness_yaw_forearm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_accel_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;avg_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;stddev_roll_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_roll_dumbbell&quot;</span><span class="hl std">,</span>  <span class="hl str">&quot;avg_pitch_dumbbell&quot;</span><span class="hl std">,</span>
                                             <span class="hl str">&quot;stddev_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_pitch_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;avg_yaw_dumbbell&quot;</span><span class="hl std">,</span>   <span class="hl str">&quot;stddev_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_yaw_dumbbell&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_accel_arm&quot;</span><span class="hl std">,</span>     <span class="hl str">&quot;avg_roll_arm&quot;</span><span class="hl std">,</span>     <span class="hl str">&quot;stddev_roll_arm&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;var_roll_arm&quot;</span>  <span class="hl std">))]</span>

<span class="hl std">predunlab</span><span class="hl kwb">&lt;-</span><span class="hl kwd">predict</span><span class="hl std">(modelRF,</span> <span class="hl kwc">newdata</span><span class="hl std">=testdb)</span>
<span class="hl kwd">print</span><span class="hl std">(predunlab)</span>
</pre></div>
<div class="output"><pre class="knitr r">##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
##  B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
## Levels: A B C D E
</pre></div>
</div></div>

</body>
</html>
