## YTD Order Analysis for Rechargeable Battery Company

### Overview

The following is an investigation into customer order data from a recently launched rechargeable battery brand's Amazon Seller Central account. In addition to EDA to uncover trends in purchase behavior, the goal of this analysis is determine whether or not the current rate of products being returned as defective warrants an investigation into the current manufacturer.

### Technologies Used

Pandas, Numpy, SciPy, Matplotlib, Plotly, Seaborn, 

### The Data

The data analyzed is from the 1257 orders since going live on Amazon in January, in addition to exports from Seller Central detailing returns and requests for replacement.

### EDA



### Hypothesis Testing: Frequentist Approach

#### One-Tailed Hypothesis Test to Determine with 95% Confidence Battery Company Does Not Need to Investigate Manufacturer

 - In the early stages, the battery company's tolerance for defect is higher as they work out kinks with the manufacturer and battery design. They are consistently researching defective product to fix known issues, but in the early stages are willing to manage defects up to 5%, as which stage they would request a formal investigation into the manufacturer to determine whether or not the root cause of defect is a result of manufacturing practices as opposed to a design flaw.
 
 
 - The null hypthesis in this test would be that the defective rate for these batteries will fall at or below 5% as future orders come through based on the current sample of orders and customer returns/defects.
 
 <img src="https://github.com/ryankirkland/customer-data-analysis/blob/master/images/binomial.png"/>

#### Bayesian Approach

### Conclusion

### Future Work
