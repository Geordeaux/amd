---
id: overview
title: Overview
---

The Polaris system will gives you a score, called **polaris score**, which shows the overall Performance of your applications, including the desktop, mobile and web. The polaris score helps developers explore to further improve their application performance, even certain version application performance. The poalris score is from 0 to 100, the bigger the score is, the better user experience is. A "perfect" score of 100 is extremely challenging to achieve and not expected.

Only `performance metrics` contribute to the polaris score, that is the score is a weighted average of the `metric scores`. Naturally, more heavily weighted metrics have a bigger effect on your polaris score. You can self-define the weight of every `performance metrics`, and it can be fixed value or calculated value from raw website performance data. 

The `metric scores` is from 0 to 100 and decided by the real website performance data on [HTTP Archive](https://httparchive.org/) and the log-normal distribution call `logitnormal function`. The logitnormal function transfer the raw website performance data to the `metric scores` and it makes sure the score is between 0 and 100 and not includes the worst score 0 and the perfect score 100. 
 
To display the different performance of applications, we use the colore to show the poalris score, Generally, 
- 0 to 49, red, poor
- 50 to 89, orange, need improvement
- 90 to 100, green , good user experience

Of course, you can customize your colored display.

Our Polaris system read the performance data from ES index, then use the performance data to calculate the metric scores, and calculate the polaris score by the weighted metric scores.
