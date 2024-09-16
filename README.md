## Evaluating the Impact of Enrollment Prompts on User Engagement: A Data-Driven A/B Testing

Note: This project is from the
In this experiment, Udacity tested a change on the course overview page aimed at setting clearer time expectations for students enrolling in a free trial. Students who clicked "Start a free trial" were asked how much time they had to commit to the course. Those with fewer than 5 hours per week received a message suggesting they may want to access the free course materials instead. 

**Goal:**
To reduce the number of students leaving the free trial due to time constraints while maintaining the overall completion rate. 

**Unit of diversion:**
 Cookie. 
 Student progress was also tracked by user-id upon enrollment. 


#### Metric choice
**Invariant metrics**
1. Number of cookies: The number of unique cookies to view the course overview page. (dmin=3000)
2. Number of clicks: The number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is triggered). (dmin=240)

**Evaluation metrics:**
1. Gross conversion: The number of user-ids to complete checkout and enroll in the free trial divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
2. Retention: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of user-ids to complete checkout. (dmin=0.01)
3. Net conversion: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)





### Measuring variability

This [spreadsheet](https://docs.google.com/spreadsheets/d/1MYreBw6wxBCLsgTJqhO9g4Ppf3dQbX7HFfIwWkMvmjE/edit?gid=103790292#gid=103790292) shows the calculated values of the evaluation metrics.

As calculated, the standard deviation from each evaluation metric is given below:
Gross conversion: Standard deviation = 0.0072
Retention: Standard deviation = 0.0194
Net Conversion: Standard deviation = 0.0055 

The standard deviation formula is given as σ= √(p(1−p)/n)
The solution was carried out in the spreadsheet linked above. 

The formula is an analytical formula used to calculate the standard deviation of a binomial distribution (which applies here, since each metric involves a proportion of successes over a number of trials)

The inputs for this formula were the probabilities 𝑝 (success rates) and n (sample sizes), which were given in the problem.
The result was obtained purely through mathematical calculation (analytically), rather than through any empirical method (like simulations or experimental data collection).

---
Question
Do you expect the analytic estimates to be accurate? That is, for which metrics, if any, would you want to collect an empirical estimate of the variability if you had time?

Response: 
**Gross Conversion:**  The probability of enrollment given a click is relatively high (20.625%), and the sample size (3,200 clicks per day) is large. For gross conversion, the analytic estimate should be fairly accurate because large sample sizes tend to produce reliable standard deviations when assuming binomial distribution.

**Retention** may benefit from empirical validation given its smaller sample size. 

**Net Conversion**, with a lower success probability, is more prone to variability, so collecting empirical estimates would provide a more accurate understanding of this metric's variability.

---

### Sizing
#### Choosing the Number of Samples given Power

**Step 1:** 

For each metric, we will use the following inputs:

- Alpha (α): 0.05 (the significance level, corresponding to a 95% confidence level).
- Beta (β): 0.2 (corresponding to 80% power, or 1 - β).
- Minimum Detectable Effect (dmin):
- Gross Conversion: 0.01
- Retention: 0.01
- Net Conversion: 0.0075

**Step 2:** 

​Sample Size Formula
We can use the following formula for sample size estimation in an A/B test with two groups (control and treatment):

 n= (2(σ/2 ^2)(Z<sub>α/2</sub> + Z<sub>β</sub>)^2)/ dmin^2

Where:

- n is the sample size per group.
- σ is the standard deviation of the metric.
- Z<sub>α/2</sub> is the z-value for the significance level (α = 0.05, so Z<sub>α</sub> = 1.96)
- Z<sub>β</sub> is the z-value for the power level (β = 0.2, so Z<sub>β</sub> = 0.84)
- dmin is the minimum detectable effect size.

Again, the metrics have been calculated in the referenced sheet above. You can also check [here](https://docs.google.com/spreadsheets/d/1MYreBw6wxBCLsgTJqhO9g4Ppf3dQbX7HFfIwWkMvmjE/edit?gid=103790292#gid=103790292)
​
 To adequately power the experiment, we need to ensure we have enough pageviews for each metric. A total of 151 pageviews are needed.

 #### Duration

 To calculate the % of traffic to divert to the experiment, we need to focus on those who click "Start free trial” (3,200 cookies)" The total number of pageviews we need for both groups is 151.

**% of traffic to divert** = no of pageviews required for both groups/  unique cookies who click "Start free trial” 

= 4.73%


**Risk consideration** 

The change in the experiment doesn’t seem too risky (it’s a change to messaging about time commitment), so there’s no strong reason to avoid diverting all traffic. 

#### Exposure 
With 5% of traffic diverted to the experiment, it would take less than a day to collect enough data for the sample size based on the analytic estimates of variance.
Thus, the experiment would run very quickly, and there is no need to reconsider any earlier decisions regarding the sample size or traffic diversion percentage.



### Experiment Analysis
#### Sanity Checks
This [spreadsheet](https://docs.google.com/spreadsheets/d/1O-4VeCNMU8Wi08FjWFRujJvtmhayW2MYb_rALHOvpQM/edit?gid=743309786#gid=743309786) shows the cleaned data needed to compute the above metrics, broken down day by day. This include both the control and experimental group.

The descriptions of the columns are provided below
- Pageviews: Number of unique cookies to view the course overview page that day.
- Clicks: Number of unique cookies to click the course overview page that day.
- Enrollments: Number of user-ids to enroll in the free trial that day.
- Payments: Number of user-ids who who enrolled on that day to remain enrolled for 14 days and thus make a payment. (Note that the date for this column is the start date, that is, the date of enrollment, rather than the date of the payment. The payment happened 14 days later. Because of this, the enrollments and payments are tracked for 14 fewer days than the other columns.)















