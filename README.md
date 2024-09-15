### A-B-testing
Note: This project is from the
In this experiment, Udacity tested a change on the course overview page aimed at setting clearer time expectations for students enrolling in a free trial. Students who clicked "Start a free trial" were asked how much time they had to commit to the course. Those with fewer than 5 hours per week received a message suggesting they may want to access the free course materials instead. 

**Goal:**
To reduce the number of students leaving the free trial due to time constraints while maintaining the overall completion rate. 

**Unit of diversion:**
 Cookie. 
 Student progress was also tracked by user-id upon enrollment. 


#### Metric choice
**Invariant metrics**
Number of cookies: The number of unique cookies to view the course overview page. (dmin=3000)
Number of clicks: The number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is triggered). (dmin=240)

**Evaluation metrics:**
Gross conversion: The number of user-ids to complete checkout and enroll in the free trial divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
Retention: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of user-ids to complete checkout. (dmin=0.01)
Net conversion: The number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)

**Net conversion:** That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)



### Measuring variability

This [spreadsheet](https://docs.google.com/spreadsheets/d/1MYreBw6wxBCLsgTJqhO9g4Ppf3dQbX7HFfIwWkMvmjE/edit?gid=103790292#gid=103790292) shows the calculated values of the evaluation metrics.

As calculated, the standard deviation from each evaluation metric is given below:
Gross conversion: Standard deviation = 0.0072
Retention: Standard deviation = 0.0194
Net Conversion: Standard deviation = 0.0055 

The standard deviation formula is given as σ= √(p(1−p)/n)
The solution was carried out in the spreadsheet linked above. The formula is an analytical formula used to calculate the standard deviation of a binomial distribution (which applies here, since each metric involves a proportion of successes over a number of trials)
The inputs for this formula were the probabilities 
𝑝 (success rates) and n (sample sizes), which were given in the problem.
The result was obtained purely through mathematical calculation (analytically), rather than through any empirical method (like simulations or experimental data collection).

---
Question
Do you expect the analytic estimates to be accurate? That is, for which metrics, if any, would you want to collect an empirical estimate of the variability if you had time?

Response: 
The **Gross Conversion** due to its large sample size and straightforward calculation. **Retention** may benefit from empirical validation given its smaller sample size. **Net Conversion**, with a lower success probability, is more prone to variability, so collecting empirical estimates would provide a more accurate understanding of this metric's variability.

---



​
 
​
 




