---
title: "My first R Markdown project"
author: "Arturo Carrión"
date: "30 de enero de 2020"
output: "html_document"
---

<div class="chunk" id="setup"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">knitr</span><span class="hl opt">::</span><span class="hl std">opts_chunk</span><span class="hl opt">$</span><span class="hl kwd">set</span><span class="hl std">(</span><span class="hl kwc">echo</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
</pre></div>
</div></div>

###Loading and processing the data

First we will load the data from the working directory (previously downloaded).

<div class="chunk" id="data"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">#Reading the data</span>
<span class="hl std">data</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl str">&quot;C:/Users/arturo/Desktop/curso de coursera/Data Science/Reproducible Research/activity.csv&quot;</span><span class="hl std">,</span><span class="hl kwc">sep</span> <span class="hl std">=</span> <span class="hl str">&quot;,&quot;</span><span class="hl std">,</span><span class="hl kwc">header</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>

<span class="hl com">#Transforming date into date class object</span>
<span class="hl std">data</span><span class="hl opt">$</span><span class="hl std">date</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.Date</span><span class="hl std">(data</span><span class="hl opt">$</span><span class="hl std">date)</span>
<span class="hl kwd">head</span><span class="hl std">(data)</span>
</pre></div>
<div class="output"><pre class="knitr r">##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
</pre></div>
</div></div>

###What is the mean of the total number of steps taken per day?

For this part of the assignment, you can ignore the missing values in the dataset.

First we will Calculate the total number of steps taken per day.

<div class="chunk" id="steps"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(dplyr)</span>
<span class="hl std">steps_taken</span> <span class="hl kwb">&lt;-</span> <span class="hl std">data</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">group_by</span><span class="hl std">(date)</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">summarise</span><span class="hl std">(</span><span class="hl kwc">steps</span> <span class="hl std">=</span> <span class="hl kwd">sum</span><span class="hl std">(steps,</span><span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">))</span>
<span class="hl kwd">print</span><span class="hl std">(steps_taken)</span>
</pre></div>
<div class="output"><pre class="knitr r">## # A tibble: 61 x 2
##    date       steps
##    &lt;date&gt;     &lt;int&gt;
##  1 2012-10-01     0
##  2 2012-10-02   126
##  3 2012-10-03 11352
##  4 2012-10-04 12116
##  5 2012-10-05 13294
##  6 2012-10-06 15420
##  7 2012-10-07 11015
##  8 2012-10-08     0
##  9 2012-10-09 12811
## 10 2012-10-10  9900
## # ... with 51 more rows
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">rows</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">nrow</span><span class="hl std">(steps_taken)</span>
</pre></div>
</div></div>

We can see that there is **<code class="knitr inline">61</code>** days in the dataset corresponding to october and november.

Now we will make a histogram with the total number of steps taken each day

<div class="chunk" id="histogram"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(ggplot2)</span>
<span class="hl com">#Creating a pallete of colors</span>
<span class="hl std">BB</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">colorRampPalette</span><span class="hl std">(</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;Blue&quot;</span><span class="hl std">,</span><span class="hl str">&quot;black&quot;</span><span class="hl std">),</span><span class="hl kwc">space</span><span class="hl std">=</span><span class="hl str">&quot;Lab&quot;</span><span class="hl std">)</span>

<span class="hl com">#Plotting</span>
<span class="hl std">g</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ggplot</span><span class="hl std">(</span><span class="hl kwc">data</span> <span class="hl std">= steps_taken,</span><span class="hl kwd">aes</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=steps))</span>
        <span class="hl std">p</span> <span class="hl kwb">=</span> <span class="hl std">g</span> <span class="hl opt">+</span> <span class="hl kwd">geom_histogram</span><span class="hl std">(</span><span class="hl kwc">bins</span> <span class="hl std">=</span> <span class="hl num">5</span><span class="hl std">,</span><span class="hl kwc">fill</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl kwd">BB</span><span class="hl std">(</span><span class="hl num">5</span><span class="hl std">)))</span> <span class="hl opt">+</span>
                <span class="hl kwd">ggtitle</span><span class="hl std">(</span><span class="hl str">&quot;Total of steps taken each day&quot;</span><span class="hl std">)</span> <span class="hl opt">+</span>
                <span class="hl kwd">xlab</span><span class="hl std">(</span><span class="hl str">&quot;Steps taken&quot;</span><span class="hl std">)</span> <span class="hl opt">+</span> <span class="hl kwd">theme_bw</span><span class="hl std">()</span>
<span class="hl kwd">print</span><span class="hl std">(p)</span>
</pre></div>
<div class="rimage default"><img src="figure/histogram-1.png" title="plot of chunk histogram" alt="plot of chunk histogram" class="plot" /></div>
</div></div>

We can see that the max amount of steps taken in all days is around the ten thounsand. Let's see what is the mean and the median of the total number of steps taken each day.

<div class="chunk" id="mean and median"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">#Calulating the mean</span>
<span class="hl std">steps_mean</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">(steps_taken</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
        <span class="hl std">steps_mean_round</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.numeric</span><span class="hl std">(</span><span class="hl kwd">round</span><span class="hl std">(steps_mean,</span><span class="hl num">0</span><span class="hl std">))</span>

<span class="hl com">#Calculating the median</span>
<span class="hl std">steps_median</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">median</span><span class="hl std">(steps_taken</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">)</span>
</pre></div>
</div></div>

So, the mean of the steps taken each day is **<code class="knitr inline">9354</code>** and the median is **<code class="knitr inline">10395</code>**, like we saw before with the histogram, they are around the ten thousand of steps each day.

###What is the average daily activity pattern?

Let's make a time series plot of the average number of steps taken across all days.

So, First we calculate the average of steps taken each day.

<div class="chunk" id="average steps"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">steps_average</span> <span class="hl kwb">&lt;-</span> <span class="hl std">data</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">group_by</span><span class="hl std">(interval)</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">summarise</span><span class="hl std">(</span><span class="hl kwc">average</span> <span class="hl std">=</span> <span class="hl kwd">mean</span><span class="hl std">(steps,</span><span class="hl kwc">na.rm</span> <span class="hl std">=</span> <span class="hl num">TRUE</span><span class="hl std">))</span>
<span class="hl kwd">print</span><span class="hl std">(steps_average)</span>
</pre></div>
<div class="output"><pre class="knitr r">## # A tibble: 288 x 2
##    interval average
##       &lt;int&gt;   &lt;dbl&gt;
##  1        0  1.72  
##  2        5  0.340 
##  3       10  0.132 
##  4       15  0.151 
##  5       20  0.0755
##  6       25  2.09  
##  7       30  0.528 
##  8       35  0.868 
##  9       40  0     
## 10       45  1.47  
## # ... with 278 more rows
</pre></div>
</div></div>

Then we make the time series plot

<div class="chunk" id="time series plot"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">T</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ggplot</span><span class="hl std">(steps_average,</span><span class="hl kwd">aes</span><span class="hl std">(interval,average))</span>
       <span class="hl std">t</span> <span class="hl kwb">=</span> <span class="hl std">T</span> <span class="hl opt">+</span> <span class="hl kwd">geom_line</span><span class="hl std">(</span><span class="hl kwc">colour</span> <span class="hl std">=</span> <span class="hl kwd">BB</span><span class="hl std">(</span><span class="hl num">288</span><span class="hl std">))</span> <span class="hl opt">+</span>
               <span class="hl kwd">ggtitle</span><span class="hl std">(</span><span class="hl str">&quot;Average of steps taken by interval of 5-minutes&quot;</span><span class="hl std">)</span><span class="hl opt">+</span>
               <span class="hl kwd">xlab</span><span class="hl std">(</span><span class="hl str">&quot;Interval of 5-minutes&quot;</span><span class="hl std">)</span><span class="hl opt">+</span><span class="hl kwd">ylab</span><span class="hl std">(</span><span class="hl str">&quot;Average of steps taken&quot;</span><span class="hl std">)</span><span class="hl opt">+</span> <span class="hl kwd">theme_bw</span><span class="hl std">()</span>

<span class="hl kwd">print</span><span class="hl std">(t)</span>
</pre></div>
<div class="rimage default"><img src="figure/time series plot-1.png" title="plot of chunk time series plot" alt="plot of chunk time series plot" class="plot" /></div>
</div></div>

Which interval has the maximun amount of steps taken across all days?

<div class="chunk" id="Max interval steps"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">#Finding the Max steps into the interval and the amount of that max steps</span>
<span class="hl std">Max_interval</span> <span class="hl kwb">&lt;-</span> <span class="hl std">steps_average</span><span class="hl opt">$</span><span class="hl std">interval[</span><span class="hl kwd">which.max</span><span class="hl std">(steps_average</span><span class="hl opt">$</span><span class="hl std">average)]</span>
<span class="hl std">Max_steps</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">round</span><span class="hl std">(steps_average</span><span class="hl opt">$</span><span class="hl std">average[</span><span class="hl kwd">which.max</span><span class="hl std">(steps_average</span><span class="hl opt">$</span><span class="hl std">average)],</span><span class="hl num">0</span><span class="hl std">)</span>

<span class="hl com">#Creating the hour to putting the inline code</span>
<span class="hl kwd">library</span><span class="hl std">(lubridate)</span>
<span class="hl std">min</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">minutes</span><span class="hl std">(Max_interval)</span>
<span class="hl std">per</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">period_to_seconds</span><span class="hl std">(min)</span>
<span class="hl std">hour_day</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">seconds_to_period</span><span class="hl std">(per)</span>
<span class="hl std">hour_day2</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.POSIXct</span><span class="hl std">(hour_day,</span><span class="hl kwc">origin</span> <span class="hl std">=</span> <span class="hl str">&quot;2020-02-02 UTC&quot;</span><span class="hl std">,</span><span class="hl kwc">tz</span><span class="hl std">=</span><span class="hl str">&quot;GMT&quot;</span><span class="hl std">)</span>
<span class="hl std">hour_day3</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">format.POSIXct</span><span class="hl std">(hour_day2,</span><span class="hl kwc">format</span> <span class="hl std">=</span> <span class="hl str">&quot;%r&quot;</span><span class="hl std">)</span>
</pre></div>
</div></div>

So, the Interval with the maximun amount of steps taken across all days is **<code class="knitr inline">835</code>** which means that is at **<code class="knitr inline">01:55:00 p.m.</code>** Having an average of **<code class="knitr inline">206</code>** steps taken across all days.

###Imputing missing values

Now we will calculate and report the total number of missing values in the data set.

<div class="chunk" id="Missing values"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">miss_steps</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(data</span><span class="hl opt">$</span><span class="hl std">steps))</span>
</pre></div>
</div></div>

We can see that there's **<code class="knitr inline">2304</code>** missing values.

Now, we need to fill those missing values. To do that we will take the average of steps taken in each interval and then fill the data with those values.

<div class="chunk" id="filling the data"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">fill_NA</span> <span class="hl kwb">&lt;-</span> <span class="hl std">data</span>

<span class="hl kwa">for</span> <span class="hl std">(i</span> <span class="hl kwa">in</span> <span class="hl num">1</span><span class="hl opt">:</span><span class="hl kwd">nrow</span><span class="hl std">(fill_NA)) {</span>
        <span class="hl kwa">if</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(fill_NA</span><span class="hl opt">$</span><span class="hl std">steps[i])) {</span>
<span class="hl com">#Find the index value for when the interval marches the average</span>
                <span class="hl std">find_NA</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">which</span><span class="hl std">(fill_NA</span><span class="hl opt">$</span><span class="hl std">interval[i]</span> <span class="hl opt">==</span> <span class="hl std">steps_average</span><span class="hl opt">$</span><span class="hl std">interval)</span>
<span class="hl com">#Assign the value to replace the NA</span>
                <span class="hl std">fill_NA</span><span class="hl opt">$</span><span class="hl std">steps[i]</span> <span class="hl kwb">&lt;-</span> <span class="hl std">steps_average[find_NA,]</span><span class="hl opt">$</span><span class="hl std">average</span>
        <span class="hl std">}</span>
<span class="hl std">}</span>

<span class="hl com">#Making sure that the date variable is still a date class object</span>
<span class="hl std">fill_NA</span><span class="hl opt">$</span><span class="hl std">date</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.Date</span><span class="hl std">(fill_NA</span><span class="hl opt">$</span><span class="hl std">date)</span>

<span class="hl com">#testing that the result has no NA's</span>
<span class="hl std">test_fill_NA</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">is.na</span><span class="hl std">(fill_NA)</span>
<span class="hl kwd">summary</span><span class="hl std">(test_fill_NA)</span>
</pre></div>
<div class="output"><pre class="knitr r">##    steps            date          interval      
##  Mode :logical   Mode :logical   Mode :logical  
##  FALSE:17568     FALSE:17568     FALSE:17568
</pre></div>
</div></div>

We can see that there is no more NA's in the dataset. 

Let's see how are the steps taken each day in the new data.

<div class="chunk" id="steps fill"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(dplyr)</span>
<span class="hl std">steps_taken_fill</span> <span class="hl kwb">&lt;-</span> <span class="hl std">fill_NA</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">group_by</span><span class="hl std">(date)</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">summarise</span><span class="hl std">(</span><span class="hl kwc">steps</span> <span class="hl std">=</span> <span class="hl kwd">sum</span><span class="hl std">(steps))</span>
<span class="hl kwd">print</span><span class="hl std">(steps_taken_fill)</span>
</pre></div>
<div class="output"><pre class="knitr r">## # A tibble: 61 x 2
##    date        steps
##    &lt;date&gt;      &lt;dbl&gt;
##  1 2012-10-01 10766.
##  2 2012-10-02   126 
##  3 2012-10-03 11352 
##  4 2012-10-04 12116 
##  5 2012-10-05 13294 
##  6 2012-10-06 15420 
##  7 2012-10-07 11015 
##  8 2012-10-08 10766.
##  9 2012-10-09 12811 
## 10 2012-10-10  9900 
## # ... with 51 more rows
</pre></div>
<div class="source"><pre class="knitr r"><span class="hl std">rows_fill</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">nrow</span><span class="hl std">(steps_taken_fill)</span>
</pre></div>
</div></div>

We can see that there's still **<code class="knitr inline">61</code>** days in the dataset corresponding to october and november.

Now we will make a histogram.

<div class="chunk" id="histogram fill"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(ggplot2)</span>
<span class="hl com">#Creating a pallete of colors</span>
<span class="hl std">GB</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">colorRampPalette</span><span class="hl std">(</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;green&quot;</span><span class="hl std">,</span><span class="hl str">&quot;black&quot;</span><span class="hl std">),</span><span class="hl kwc">space</span><span class="hl std">=</span><span class="hl str">&quot;Lab&quot;</span><span class="hl std">)</span>

<span class="hl com">#Plotting</span>
<span class="hl std">N</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ggplot</span><span class="hl std">(</span><span class="hl kwc">data</span> <span class="hl std">= steps_taken_fill,</span><span class="hl kwd">aes</span><span class="hl std">(</span><span class="hl kwc">x</span><span class="hl std">=steps))</span>
        <span class="hl std">n</span> <span class="hl kwb">=</span> <span class="hl std">N</span> <span class="hl opt">+</span> <span class="hl kwd">geom_histogram</span><span class="hl std">(</span><span class="hl kwc">bins</span> <span class="hl std">=</span> <span class="hl num">5</span><span class="hl std">,</span><span class="hl kwc">fill</span> <span class="hl std">=</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl kwd">GB</span><span class="hl std">(</span><span class="hl num">5</span><span class="hl std">)))</span> <span class="hl opt">+</span>
                <span class="hl kwd">ggtitle</span><span class="hl std">(</span><span class="hl str">&quot;Total of steps taken each day&quot;</span><span class="hl std">)</span> <span class="hl opt">+</span>
                <span class="hl kwd">xlab</span><span class="hl std">(</span><span class="hl str">&quot;Steps taken&quot;</span><span class="hl std">)</span> <span class="hl opt">+</span> <span class="hl kwd">theme_bw</span><span class="hl std">()</span>
<span class="hl kwd">print</span><span class="hl std">(n)</span>
</pre></div>
<div class="rimage default"><img src="figure/histogram fill-1.png" title="plot of chunk histogram fill" alt="plot of chunk histogram fill" class="plot" /></div>
</div></div>

We can see that the amount of steps taken each day is around the ten thounsand steps each day. Let's see what is the mean and the median of the total number of steps taken each day.

<div class="chunk" id="mean and median fill"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">#Calulating the mean</span>
<span class="hl std">steps_mean_fill</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">mean</span><span class="hl std">(steps_taken_fill</span><span class="hl opt">$</span><span class="hl std">steps)</span>
        <span class="hl std">steps_mean_round_fill</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.numeric</span><span class="hl std">(</span><span class="hl kwd">round</span><span class="hl std">(steps_mean_fill,</span><span class="hl num">0</span><span class="hl std">))</span>

<span class="hl com">#Calculating the median</span>
<span class="hl std">steps_median_fill</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">median</span><span class="hl std">(steps_taken_fill</span><span class="hl opt">$</span><span class="hl std">steps)</span>
        <span class="hl std">steps_median_round_fill</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">as.numeric</span><span class="hl std">(</span><span class="hl kwd">round</span><span class="hl std">(steps_median_fill,</span><span class="hl num">0</span><span class="hl std">))</span>
</pre></div>
</div></div>

So, the mean of the steps taken each day is **<code class="knitr inline">10766</code>** and the median is **<code class="knitr inline">10766</code>**, like we saw before with the histogram, they are around the ten thousand of steps each day.

Looks like there's a slightly difference between the first data and the filled one. But let's make the comparison.

<div class="chunk" id="data plot comparison"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">#Let's see the plots</span>
<span class="hl kwd">print</span><span class="hl std">(p)</span>
</pre></div>
<div class="rimage default"><img src="figure/data plot comparison-1.png" title="plot of chunk data plot comparison" alt="plot of chunk data plot comparison" class="plot" /></div>
<div class="source"><pre class="knitr r"><span class="hl kwd">print</span><span class="hl std">(n)</span>
</pre></div>
<div class="rimage default"><img src="figure/data plot comparison-2.png" title="plot of chunk data plot comparison" alt="plot of chunk data plot comparison" class="plot" /></div>
</div></div>

There is a difference in the first bar but the the max amount of steps taken in all days is still around the ten thousand.

The mean and median of the first data are **<code class="knitr inline">9354</code>**,  **<code class="knitr inline">10395</code>**, and the mean and median of the filled one are **<code class="knitr inline">10766</code>**,**<code class="knitr inline">10766</code>**. Either the mean and the median of the filled one are higher than the original.

###Are there differences in activity patterns between weekdays and weekends?

Let's see the trend of the steps taken by days but, this time we will make difference between weekdays and weekends.

First, let's create a variable to identify every day of the week

<div class="chunk" id="weekdays and weekends"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">fill_NA</span><span class="hl opt">$</span><span class="hl std">weekdays</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">weekdays</span><span class="hl std">(fill_NA</span><span class="hl opt">$</span><span class="hl std">date)</span>
<span class="hl kwd">head</span><span class="hl std">(fill_NA)</span>
</pre></div>
<div class="output"><pre class="knitr r">##       steps       date interval weekdays
## 1 1.7169811 2012-10-01        0    lunes
## 2 0.3396226 2012-10-01        5    lunes
## 3 0.1320755 2012-10-01       10    lunes
## 4 0.1509434 2012-10-01       15    lunes
## 5 0.0754717 2012-10-01       20    lunes
## 6 2.0943396 2012-10-01       25    lunes
</pre></div>
</div></div>

Now that we know the weekdays of every day, let's separate them into weekdays and weekend.

<div class="chunk" id="weekend variable"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl com">#First categorize the normal weekdays</span>
<span class="hl std">fill_NA</span><span class="hl opt">$</span><span class="hl std">wknd</span> <span class="hl kwb">&lt;-</span> <span class="hl std">fill_NA</span><span class="hl opt">$</span><span class="hl std">wknd</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;Weekday&quot;</span>

<span class="hl com">#Now, determine the weekend days</span>
<span class="hl std">fill_NA</span><span class="hl opt">$</span><span class="hl std">wknd[fill_NA</span><span class="hl opt">$</span><span class="hl std">weekdays</span> <span class="hl opt">%in%</span> <span class="hl kwd">c</span><span class="hl std">(</span><span class="hl str">&quot;sábado&quot;</span><span class="hl std">,</span><span class="hl str">&quot;domingo&quot;</span><span class="hl std">)]</span> <span class="hl kwb">&lt;-</span> <span class="hl str">&quot;Weekend&quot;</span>
</pre></div>
</div></div>

Now that we have the days of the week categorized, let's see the average between them.~~(sorry for the spanish, don't know why that appear like that)~~

<div class="chunk" id="weekend average"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">week_average</span> <span class="hl kwb">&lt;-</span> <span class="hl std">fill_NA</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">group_by</span><span class="hl std">(wknd,interval)</span> <span class="hl opt">%&gt;%</span>
        <span class="hl kwd">summarise</span><span class="hl std">(</span><span class="hl kwc">average</span> <span class="hl std">=</span> <span class="hl kwd">mean</span><span class="hl std">(steps))</span>
<span class="hl kwd">print</span><span class="hl std">(week_average)</span>
</pre></div>
<div class="output"><pre class="knitr r">## # A tibble: 576 x 3
## # Groups:   wknd [2]
##    wknd    interval average
##    &lt;chr&gt;      &lt;int&gt;   &lt;dbl&gt;
##  1 Weekday        0  2.25  
##  2 Weekday        5  0.445 
##  3 Weekday       10  0.173 
##  4 Weekday       15  0.198 
##  5 Weekday       20  0.0990
##  6 Weekday       25  1.59  
##  7 Weekday       30  0.693 
##  8 Weekday       35  1.14  
##  9 Weekday       40  0     
## 10 Weekday       45  1.80  
## # ... with 566 more rows
</pre></div>
</div></div>

Let's make a plot to see the difference between weekdays and weekend

<div class="chunk" id="weekend plot"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">W</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">ggplot</span><span class="hl std">(week_average,</span><span class="hl kwd">aes</span><span class="hl std">(interval,average))</span>
        <span class="hl std">w</span> <span class="hl kwb">=</span> <span class="hl std">W</span> <span class="hl opt">+</span> <span class="hl kwd">geom_line</span><span class="hl std">(</span><span class="hl kwc">colour</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl kwd">BB</span><span class="hl std">(</span><span class="hl num">288</span><span class="hl std">),</span><span class="hl kwd">GB</span><span class="hl std">(</span><span class="hl num">288</span><span class="hl std">)))</span><span class="hl opt">+</span> <span class="hl kwd">facet_grid</span><span class="hl std">(wknd</span><span class="hl opt">~</span><span class="hl std">.)</span><span class="hl opt">+</span>
                <span class="hl kwd">ggtitle</span><span class="hl std">(</span><span class="hl str">&quot;Steps taken by weekdays&quot;</span><span class="hl std">)</span><span class="hl opt">+</span><span class="hl kwd">xlab</span><span class="hl std">(</span><span class="hl str">&quot;Steps taken&quot;</span><span class="hl std">)</span><span class="hl opt">+</span>
                <span class="hl kwd">theme_bw</span><span class="hl std">()</span>

<span class="hl kwd">print</span><span class="hl std">(w)</span>
</pre></div>
<div class="rimage default"><img src="figure/weekend plot-1.png" title="plot of chunk weekend plot" alt="plot of chunk weekend plot" class="plot" /></div>
</div></div>
        
We can see that in average the trend of the weeknds is a little bit higher than the weekdays.

#THE END
