## Evaluating the Impact of Enrollment Prompts on User Engagement: A Data-Driven A/B Testing

Note: I attached a more detailed report to this repository, or you can check it [here](https://github.com/bayoxx/Evaluating-the-Impact-of-Enrollment-Prompts-A-Data-Driven-A-B-Testing/blob/main/A_B%20Test%20for%20Udacity_.pdf).

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

The standard deviation formula is given as **σ= √(p(1−p)/n)**
The solution was carried out in the spreadsheet linked above. 

As calculated, the standard deviation from each evaluation metric is given below:
- Gross conversion: Standard deviation = 0.0072
- Retention: Standard deviation = 0.0194
- Net Conversion: Standard deviation = 0.0055 

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

I used the [Evan Miller A/B testing calculator](https://www.evanmiller.org/ab-testing/sample-size.html) to solve this.


The results are given below.


Again, the metrics have been calculated in the referenced sheet above. You can also check [here](https://docs.google.com/spreadsheets/d/1MYreBw6wxBCLsgTJqhO9g4Ppf3dQbX7HFfIwWkMvmjE/edit?gid=103790292#gid=103790292)

You can also check the table below

| Metric           | Probability | Standard Dev | dmin  | Pageview Power per Group | Total Pageviews for Both Groups |
|------------------|-------------|--------------|-------|---------------------------|---------------------------------|
| Gross conversion | 0.2063      | 0.0072       | 0.0100| 507                       | 1,014                           |
| Retention        | 0.5300      | 0.0194       | 0.0100| 1,032                     | 2,064                           |
| Net Conversion   | 0.1093      | 0.0055       | 0.0075| 539                       | 1,078                           |
| **Total**        |             |              |       |                           | **4,156**                       |

​

To adequately power the experiment, we need to ensure we have enough pageviews for each metric. A total of 4,156 pageviews are needed.

 #### Duration

To calculate the % of traffic to divert to the experiment, we need to focus on those who click "Start free trial” (40,000 cookies)" The total number of pageviews we need for both groups is 4,156.

**% of traffic to divert** = no of pageviews required for both groups/  ique cookies to view course overview page per day 

= 10.39%


**Risk consideration** 

The change in the experiment doesn’t seem too risky (it’s a change to messaging about time commitment), so there’s no strong reason to avoid diverting all traffic. 

#### Exposure 
With 10.39% of traffic diverted to the experiment, it would take less than a day to collect enough data for the sample size based on the analytic estimates of variance.
Thus, the experiment would run very quickly, and there is no need to reconsider any earlier decisions regarding the sample size or traffic diversion percentage.

---
---

### Experiment Analysis
#### Sanity Checks
This [spreadsheet](https://docs.google.com/spreadsheets/d/1O-4VeCNMU8Wi08FjWFRujJvtmhayW2MYb_rALHOvpQM/edit?gid=743309786#gid=743309786) shows the cleaned data needed to compute the above metrics, broken down day by day. This includes both the control and experimental groups.

The descriptions of the columns are provided below

- Pageviews: Number of unique cookies to view the course overview page that day.
- Clicks: Number of unique cookies to click the course overview page that day.
- Enrollments: Number of user-ids to enroll in the free trial that day.
- Payments: Number of user-ids who who enrolled on that day to remain enrolled for 14 days and thus make a payment. (Note that the date for this column is the start date, that is, the date of enrollment, rather than the date of the payment. The payment happened 14 days later. Because of this, the enrollments and payments are tracked for 14 fewer days than the other columns.)

Conducting sanity checks for the invariant metrics (page views) at a 95% confidence interval for the control and experiment groups. The result below can be found in the [spreadsheet](https://docs.google.com/spreadsheets/d/1O-4VeCNMU8Wi08FjWFRujJvtmhayW2MYb_rALHOvpQM/edit?gid=743309786#gid=743309786).


---

### Sanity Check on Pageviews (Invariant metric):

First, I conducted a sanity check on the pageviews to verify if randomization between the control and experiment groups was successful. I used a binomial test for this.

**Steps for Sanity Check:**
Note that all these calculations was carried out in the [spreadsheet](https://docs.google.com/spreadsheets/d/1O-4VeCNMU8Wi08FjWFRujJvtmhayW2MYb_rALHOvpQM/edit?gid=743309786#gid=743309786). I included the calculations here to make it easy to follow.

| Metric    | Total pageviews | Average pageviews | Probability (random) | Standard error | Margin of error | CI- lower      | CI- upper      | Sanity check |
|-----------|-----------------|------------------|----------------------|----------------|-----------------|----------------|----------------|--------------|
| Pageviews | 690203           | 345101.5         | 0.5                  | 0.0006         | 0.0012          | 344273.2564    | 345929.7436    | Pass         |


1. **Null Hypothesis**: The pageviews should be split equally (50/50) between the control and experiment groups.
2. **Calculate Total Pageviews**:
                     - Control Group Total Pageviews: 345,543
                     - Experiment Group Total Pageviews: 344,660
`                    - Combined Total Pageviews: 345,543+344,660= 690,203
3. **Expected Pageviews per Group:**
 Since the split should be random, the expected pageviews for each group are: 690,203 / 2  = 345,101.5


4. Standard Error (SE): The standard error is based on the binomial distribution:
			= √p * (1-p)/  Combined Total Pageviews

Since the cookies are randomly selected, p =0.5.
			Thus, SE = 0.0006


5. Margin of Error (ME): For a 95% confidence level, the margin of error is:
ME=1.96×SE
=1.96×0.0006  = 0.0012

6. Confidence Interval for Pageviews: CI=345,101.5±(0.0012×690,203) = 345,101.5±828.24

So, the confidence interval is approximately: [344,273 to  345,929]

7. Observed Values:
Control Group Pageviews: 345,543
Experiment Group Pageviews: 344,660
The control group's pageviews (345,543) fall within the confidence interval [344,273 to  345,929], so the **sanity check passes.**

*However, this is not sufficient enough to launch the features. Further analysis needs to be conducted on the evaluation metrics to guide our actions.*

---
### Check for Practical and Statistical Significance (Evaluation metrics)

Evaluation metrics used: 
- Gross Conversion;
- Retention; and 
- Net Conversion.

These three metrics align with the hypothesis and goals of the experiment. They measure user interest their level of commitment and the likelihood of succeeding in the course.
The calculations that I’ll be using have been carried out in the [spreadsheet](https://docs.google.com/spreadsheets/d/1O-4VeCNMU8Wi08FjWFRujJvtmhayW2MYb_rALHOvpQM/edit?gid=743309786#gid=743309786).


**Definition:**
In this case, a metric is **statistically significant** if the confidence interval does not include 0 (that is, one can be confident there was a change), and it is practically significant if the confidence interval does not include the **practical significance boundary** (that is, one can be confident there is a change that matters to the business).

The spreadsheet below shows the calculated confidence interval (CI) for the three evaluation metrics.

| Metrics           | Metric differences | P pool | Standard Error | Margin of error | CI- lower | CI- upper | d-min  | Statistically Significant? | Practically Significant? |
|-------------------|--------------------|--------|----------------|-----------------|-----------|-----------|--------|----------------------------|--------------------------|
| Gross conversion   | -0.0125            | 0.1271 | 0.0028         | 0.0055          | -0.0180   | -0.0070   | 0.0100 | Yes                        | Yes                      |
| Net conversion     | -0.0030            | 0.0702 | 0.0021         | 0.0042          | -0.0072   | 0.0012    | 0.0075 | No                         | Yes                      |
| Retention          | -1.2936            | 0.5519 | 0.0117         | 0.0230          | -1.3166   | -1.2706   | 0.0010 | Yes                        | Yes                      |



### Sign test

| Metric            | No of trials | No of successes | Two-tail P value | Two-tail P value < α (0.05) |
|-------------------|--------------|-----------------|------------------|-----------------------------|
| Gross conversion   | 23           | 4               | 0.0026           | Yes                         |
| Net conversion     | 23           | 10              | 0.6776           | No                          |
| Retention          | 23           | 13              | 0.6776           | No                          |



### Recommendation

#### Metrics Overview:

**Gross Conversion:**
Statistically significant (both in the original test and the sign test).
Practically significant.

**Net Conversion:**
Statistically not significant, but practically significant.
Sign test also confirms no statistical significance (p-value > α).

**Retention:**
Statistically and practically significant in the original analysis.
However, Sign test indicates no statistical significance (p-value > α).

#### Recommended Action: 

- Digging Deeper and Running a Follow-Up Experiment:
While gross conversion is a strong positive signal, the uncertainty around net conversion and retention suggests that further investigation and refinement of the experiment are needed before a full launch.

- Digging Deeper:
Focus on retention and net conversion: Both are practically significant but lack strong statistical significance. Investigating why retention and net conversion didn't reach statistical significance could reveal insights about user behavior.
Different user segments can be analyzed (e.g., users who enrolled vs. those who accessed free materials) or periods within the experiment to see if certain groups were more impacted.

- Running a Follow-Up Experiment:
Given that **net conversion** didn’t reach statistical significance but is practically significant, running a follow-up experiment could help to gather more data. Increasing the sample size or extending the experiment duration could provide clearer results on whether the net conversion will improve and whether retention shows more definitive significance.
This follow-up could also refine messaging or UX elements to improve user decisions at key stages like checkout and post-trial conversion.



Note: I attached a more detailed report to this repository, or you can check it [here](https://github.com/bayoxx/Evaluating-the-Impact-of-Enrollment-Prompts-A-Data-Driven-A-B-Testing/blob/main/A_B%20Test%20for%20Udacity_.pdf).

Task Source: Udacity



