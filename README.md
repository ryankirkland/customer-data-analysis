# YTD Order Analysis for Rechargeable Battery Company

## Overview

The following is an investigation into customer order data from a recently launched rechargeable battery brand's Amazon Seller Central account. In addition to EDA to uncover trends in purchase behavior, the goal of this analysis is determine whether or not the current rate of products being returned as defective warrants an investigation into the current manufacturer.

### Technologies Used

Pandas, Numpy, SciPy, Matplotlib, Plotly, Seaborn, 

## The Data

The data analyzed is from the 1257 orders since going live on Amazon in January, in addition to exports from Seller Central detailing returns and requests for replacement.

## EDA

<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/defect-vs-quality.png" width=45% />
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/return-reason.png" width=45% />
</div>

<img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/Screen%20Shot%202020-07-23%20at%202.56.37%20PM.png" width=100% />
<img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/state-heatmap.png" width=100% />

## Hypothesis Testing: Frequentist Approach

#### One-Tailed Hypothesis Test to Determine with 95% Confidence Battery Company Does Not Need to Investigate Manufacturer

 - In the early stages, the battery company's tolerance for defect is higher as they work out kinks with the manufacturer and battery design. They are consistently researching defective product to fix known issues, but in the early stages are willing to manage defects up to 5%, as which stage they would request a formal investigation into the manufacturer to determine whether or not the root cause of defect is a result of manufacturing practices as opposed to a design flaw.
 
 
 - The null hypthesis in this test would be that the defective rate for these batteries will fall at or below 5% as future orders come through based on the current sample of orders and customer returns/defects.
 
<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/binomial.png"/>
</div>

## Hypothesis Test: Bayesian Approach

#### Beta Distribution for determining the probability that 5% or more units will be defective in future shipments

I want to build a probability distribution for the value of p, which is the proportion of units that will turn out to be defective in future shipments. We will define success as locating a defective and failure as no defects found.

<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/total-beta.png"/>
</div>

#### Beta Distributions of Defect Rates by Product

- Even though analysis at the product level yielded a close to 0 probability of defect rates rising over 5% based on current data, there is still a possibility that the defect rate at the individual product level is >5%, and a single product is responsible dragging up the overall defect rate.

<div align='center'>
    <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/aa-aaa-beta.png"/>
</div>
            
## Conclusion

## Future Work
