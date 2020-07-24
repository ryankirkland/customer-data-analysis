# YTD Order Analysis for Rechargeable Battery Company

## Overview

The following is an investigation into customer order data from a recently launched rechargeable battery brand's Amazon Seller Central account. In addition to EDA to uncover trends in purchase behavior, the goal of this analysis is to determine whether or not the current rate of products being returned as defective warrants an investigation into the current manufacturer.

### Technologies Used

Pandas, Numpy, SciPy, Matplotlib, Plotly

## The Data

The data analyzed is from the 1257 orders and 1371 units shipped since going live on Amazon in January, in addition to exports from Seller Central detailing returns and requests for replacement.

## EDA

- Clean product purchase date data and convert to two columns - one for time of day and one for standard date. The left visualization represents the frequency of purchase during each hour of the day, where the stated hour "9 AM" contains orders placed between 9:00 and 9:59:59. The right visualization is frequency of orders by date over time:

<div align="center">
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/Screen%20Shot%202020-07-23%20at%202.56.37%20PM.png" width=75%/>
</div>
<div align="center">
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/orders-over-time.png" />
</div>

- In another exercise, I chose to clean the order ship address data to allow for grouping by state to create a visualization of the regions that have placed the most orders YTD:

<div align="center">
   <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/orders-by-state.png" width=70%>
</div>

The following charts break out the return reasons across all return and replacement requests, then a visualization of the total shipped product that was marked as defective by customers compared to those that shipped in expected working condition, but may have been returned for reasons outside of defect:

<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/defect-vs-quality.png" width=45% />
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/return-reasons.png" width=45% />
</div>

## Hypothesis Testing: Frequentist Approach

#### One-Tailed Hypothesis Test to Determine with 95% Confidence Battery Company Does Not Need to Investigate Manufacturer

 - In the early stages, the battery company's tolerance for defect is higher as they work out kinks with the manufacturer and battery design. They are consistently researching defective product to fix known issues, but in the early stages are willing to manage defects up to 5%, at which stage they would request a formal investigation into the manufacturer to determine whether or not the root cause of defect is a result of manufacturing practices as opposed to a design flaw.
 
 
 - <b>The null hypthesis</b> in this test would be the defective rate for these batteries will be less than or equal to 5%.
 
```
alpha = 0.05
n = sum(order_status_counts['quantity-shipped'])
prob_null = 0.05
prob_alt = (order_status_counts.iloc[0,1]/n)

binomial_dist = stats.binom(n, prob_null)
binom_mean = n * prob_null
binom_var = n * prob_null * (1-prob_null)
normal_approx = stats.norm(binom_mean, np.sqrt(binom_var))

p_value = 1 - normal_approx.cdf(1371 * prob_alt)
```
 
<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/binomial.png"/>
</div>

##### Results:

With a p-value of 0.999 and alpha of 0.05, I cannot reject the null hypothesis that the defect rate will be less than or equal to 5%. These results indicate there is no need to investigate our manufacturer.

## Hypothesis Test: Bayesian Approach

#### Beta Distribution for determining the probability that 5% or more units will be defective

Here, I want to build a probability distribution for the value of p, which is the proportion of units that will turn out to be defective in future shipments. We will define success as locating a defective and failure as no defects found.

```
probs = np.linspace(0,1,10000)

defects_a = 21
quality_a = 1350

beta_dist_a = stats.beta(defects_a+1, quality_a+1)

beta_dist_a.rvs(10**6) > 0.05

lower_bound = beta_dist_a.ppf(0.025)
upper_bound = beta_dist_a.ppf(0.975)
```

<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/beta-dist-total.png"/>
</div>

##### Results:

- Probability of 5% of inventory being defective: ~ 0.0
- 95% probability that the defect rate of all products will fall between 1.01% and 2.33%.

#### Beta Distributions of Defect Rates by Product

- Even though analysis at the product level yielded a close to 0 probability of defect rates rising over 5% based on current data, there is still a possibility that the defect rate at the individual product level is >5%, and a single product is responsible dragging up the overall defect rate.

```
aa_total = order_status_count_by_product.iloc[1,1]
aaa_total = order_status_count_by_product.iloc[2,1]

aa_defects_count = order_status_count_by_product.iloc[1,3]
aa_quality = aa_total - aa_defects_count
aaa_defects_count = order_status_count_by_product.iloc[2,3]
aaa_quality = aaa_total - aaa_defects_count

probs = np.linspace(0,1,10000)

aa_beta = stats.beta(aa_defects_count+1, aa_quality+1)
aaa_beta = stats.beta(aaa_defects_count+1, aaa_quality+1)

aa_lower_bound = aa_beta.ppf(0.025)
aa_upper_bound = aa_beta.ppf(0.975)
aaa_lower_bound = aaa_beta.ppf(0.025)
aaa_upper_bound = aaa_beta.ppf(0.975)

aa_samples = aa_beta.rvs(10**6)
aaa_samples = aaa_beta.rvs(10**6)
```

<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/credible_interval.png"/>
</div>

- Probability of 5% or greater defect rate for AA: 0.0
- Probability of 5% or greater defect rate for AAA: 0.0
- Probability of 2% or greater defect rate for AA: 0.1303
- Probability of 2% or greater defect rate for AAA: 0.5639

##### Credible Interval

- 95% Credible Interval of AA Defect Rates: (0.57%, 2.6%)
- 95% Credible Interval of AAA Defect Rates: (1.22%, 3.27%)
            
## Results

- The results of the hypothesis test lead me to conclude, based on the current data, the defect rate at the total product level and individual product level does not warrant an investigation into manufacturing practices, as I was unable to reject my null hypothesis in the Frequentist approach to testing whether or not the defect rate for the batteries would land at or below 5% in future shipments. The Bayesian approach also showed an almost 0 probability of a 5% or greater defect rate.

##### Assuming 10,000 unit order to satisfy demand:
- Potential Retail Sales Lost from Defective AA: ($1,700.29, $7,808.9)
- Potential Retail Sales Lost from Defective AAA: ($3,051.07, $8,168.25)

## Future Work

-  I would like to build on this to leverage Bayesian Decision Making taking in defect rates, demand levels, wholesale price, retail sales prices, and costs associated to selling to determine the most profitable choice in manufacturer through modeling future months of performance.
