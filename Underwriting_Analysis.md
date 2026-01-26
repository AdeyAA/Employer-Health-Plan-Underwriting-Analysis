# Project Overview

This project simulates the alternate funding underwriting evaluation process for an employer-sponsored health plan using Microsoft Excel.

The goal is to determine whether a single-company employee group exhibits sufficient claims predictability, demographic balance, and financial stability to be considered eligible for an alternate-funded health plan (e.g., Kaiser Permanente).

# Dataset Description
The dataset represents employees working for the same company. Each row corresponds to one employee. The dataset includes 650 rows of employees.

    # Column_name
    Age - Employee age
    Gender - Employee gender
    BMI - Body Mass Index
    Children - Number of dependents
    Region - Geographic region
    Expenses - Annual healthcare claims
    Premium - Annual premium contribution

The dataset was downloaded from Kaggle and loaded into Excel as a structured table named Data, enabling consistent formulas, PivotTables, and dashboards.

<img width="474" height="510" alt="Screenshot 2026-01-23 at 7 08 14 PM" src="https://github.com/user-attachments/assets/0508503c-78d7-4984-a62a-ae74e9ecf225" />
...

# Step 1: Data Preparation & Structure

Before analysis, I converted the data into an Excel table, `Ctrl + T`
Then I began cleaning the data, and I summed up how many rows or employees we have in the dataset using the `COUNT` function. Then removed irrelevant categories like discount eligibility. And also made sure there were no missing age or expense values, and that expenses and premiums were numeric.

# Step 2: Demographic Risk Analysis

Age Distribution

I calculated the average age amongst the group. Then I grouped employees into age bins. 18-29, 30-39, 40-49, 50-59, 60+. Then, in the summary sheet, I calculated the percentage of employees that fit into each bin using these formulas, in order to see the age distribution. And I want to flag if 25-30% of individuals in the group are 55+ years old. This is because claims variance increases, so the alternate funding risks shift to the employer:

`=(COUNTIFS(Data!A:A,">=18",Data!A:A,"<30")) / $B$1`

`=(COUNTIFS(Data!A:A,">=30",Data!A:A,"<39")) / $B$1`

`=(COUNTIFS(Data!A:A,">=40",Data!A:A,"<50")) / $B$1`

`=(COUNTIFS(Data!A:A,">=50",Data!A:A,"<60")) / $B$1`

`=(COUNTIFS(Data!A:A,">=60")) / $B$1`

`=(COUNTIFS(Data!A:A,">=55")) / $B$1`

<img width="306" height="107" alt="Screenshot 2026-01-23 at 7 22 14 PM" src="https://github.com/user-attachments/assets/0a37db26-e408-4254-ab79-39702e2654d6" />
<img width="303" height="18" alt="Screenshot 2026-01-23 at 7 23 14 PM" src="https://github.com/user-attachments/assets/83b598d4-f62f-4013-92fb-b328172325be" />

# Step 3: Health Risk Proxy
-Because clinical data are unavailable, BMI was used as a health risk proxy. I categorized individuals into 3 groups: Normal, Overweight, or Obese.
`=IF([@bmi] < 25, "Normal", IF([@bmi] < 30, "Overweight", "Obese"))`

-Higher obesity concentration correlates with chronic condition prevalence, higher average claims, and less predictable cost patterns.
-Included in the dashboard sheet, I added pivot tables to summarize employee count by BMI category and total and average expenses per category.

<img width="571" height="510" alt="Screenshot 2026-01-23 at 7 09 56 PM" src="https://github.com/user-attachments/assets/0c9397ff-94d5-46cd-b006-e9520623dc47" />

<img width="305" height="61" alt="Screenshot 2026-01-23 at 7 21 30 PM" src="https://github.com/user-attachments/assets/939f1948-e0d0-450f-b165-752c11521259" />

# Step 4: Financial Performance Analysis
This step was used to determine whether an employer group is economically viable under an alternate-funded health plan. 

-Total and Average Claims
Metrics:
Annual Claims = SUM(EmployeeData[Expenses])
Monthly Claims = Annual Claims / 12

Total claims represent the actual medical cost burden of the employee population. This establishes the baseline level of funding the plan must support.
We break the claims into a monthly equivalent because premiums are typically collected monthly. This could also help us determine whether claims align with employer budget constraints. 

-Loss Ratio
Loss Ratio = Total Claims/ Total Premium

The loss ratio measures financial efficiency so that we see how much of the premium revenue is consumed by medical claims. So if the ratio is <80% that would be favorable, 80-90% is marginal, 90% is High Risk.

<img width="303" height="31" alt="Screenshot 2026-01-23 at 7 18 40 PM" src="https://github.com/user-attachments/assets/920c6379-a840-44ad-b5c3-9471af3715f2" />



-Expense Volatility
`=STDEV.P(Data!F:F) / AVERAGE(Data!F:F)`

<img width="302" height="28" alt="Screenshot 2026-01-23 at 7 19 13 PM" src="https://github.com/user-attachments/assets/eac88230-94ff-41f4-9725-51b761af7309" />

This directly helps us to determine predictability; low variability indicates a stable and predictable claims pattern. The metrics that were used were <0.50 being low, 0.5-0.70 being moderate, and 0.70 being high volatility.


Alternate funding is viable only when all three align. When costs are reasonable, premiums cover claims, and variability is controlled.




# Step 6: Cost Distribution & Risk 

In this step, I analyzed the distribution of annual expenses.
I created a histogram using a pivot table using expense ranges in bins, and employee counts per range.
The visualization revealed whether most employees cluster near the mean. And whether a small number of high-cost claims make up the total spend of the organization. This distribution of expenses could indicate to us a very high risk because of the concentration near the end of the chart.


<img width="431" height="259" alt="Screenshot 2026-01-26 at 10 01 56 AM" src="https://github.com/user-attachments/assets/1297c872-12b7-4c7f-9102-c89ca6ba0cec" />


# Summary of Key Findings

1. Demographic stability
    -Total Employees: 650
    -Average Age: 39.3 years
    -Employees Aged 55+: 18.3%
The age distribution is suited for alternate funding. Groups with 25-30% or more of members who are age 55+ are flagged because of higher claim severity and unpredictability. The specific group analyzed here is below that threshold, indicating balance and manageable age-related risk.

<img width="545" height="289" alt="Screenshot 2026-01-26 at 8 55 01 AM" src="https://github.com/user-attachments/assets/d5896bda-39ca-4522-bab8-55261f79a96c" />

<img width="557" height="286" alt="Screenshot 2026-01-26 at 8 55 21 AM" src="https://github.com/user-attachments/assets/572ac394-6552-49e0-9931-956aee3b74ff" />

3. Health Risk Proxy (BMI Distribution)
     - 49.38% Obese
     - 32.15% Overweight
     - 18.46% Normal
The BMI distribution in this group showed a big portion of the group as overweight or obese, specifically 81.53%. While elevated BMI increases the baseline claims risk, it does not automatically disqualify a group.

<img width="542" height="289" alt="Screenshot 2026-01-26 at 8 55 38 AM" src="https://github.com/user-attachments/assets/7b7f9c0d-b990-4ef2-9f1d-61d7edff7802" />

4. Financial Performance
Total & Monthly Claims
     -Annual Claims: $8.05M
     -Monthly Claims: $671K
These numbers make up the baseline medical cost burden that the employer must fund.

5. Loss Ratio
     -Observed loss ratio: 50.18
We observed a loss ratio of 5018%. This dataset shows claims exceeding premium contributions by a big margin, letting us know that premium funding is severely insufficient to cover medical expenses.

6. Predictability
     -Observed coefficient of variation is 0.91

   A coefficient of variation near 1 indicated irregular claims. The level of predictability isn't suitable for alternate funding, the employer would have to assume claim risk.


The final eligibility conclusion was that this group was not eligible for alternate funding. The group fails two critical reqiurements becasue they have an unsustainable loss ratio and high claims unpredictability. These tell us that claims are too large and unperdictable fot the employer to be able to assume risk under an alternate-funded health plan. This group would be better suited for a fully-insured plan because the risk and unpredictability of claims would be transferred to the carrier, which would give the employer a chance at predictable costs and financial stability.

