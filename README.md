
# Houston Housing

In August 2017, Hurricane Harvey devastated the Greater Houston Metroplex, and many areas beyond. I consider myself lucky to have incurred no damage to my house. But that’s the thing – the catastrophic impact seemed to happen so randomly throughout the whole metroplex. In every neighborhood, there were houses that were hit and houses that were spared.

In the aftermath of the flood, I organized a flood relief volunteer group to help impacted homeowners in the community. We gutted houses in different areas across Houston. It was sad to see how many homes and memories were lost or destroyed, and no one expected it.

This was the inspiration for my project. With Greater Houston being so close to the gulf and so prone to heavy waters, I want to see if there is a way to forecast the changes in value and sales of homes as flooding occurs overtime.

This project so far is more heavily focused on the MLS analysis than on the impact of flooding, but it is just the beginning of my research.

## Data Sources

[HAR.com](har.com) (Houston Association of Realtors) is a comprehensive residential property search website that serves as the source of homes for sale and rentals in the state of Texas. It is widely used by realtors and real estate firms, as well as those seeking homes and home assessments throughout the state. The data gathered from this source is comprised of homes listed for sale from the year 2015 onward, as well as their outcomes. This dataset is not included in this repository for proprietary reasons.

[FEMA](fema.gov) (Federal Emergency Management Agency) acts to prepare for, prevent, respond to, and recover from national disasters. FEMA provides individual assistance to individuals and families who have sustained losses due to such disasters. Need is assessed and funds are respectively distributed. The data gathered from this site includes all applications for individual housing assistance for nationally declared disasters. The dataset will be narrowed to applications specifically related to Hurricane Harvey.

The data gathered from each of these sources includes information regarding homes in the Greater Houston Metroplex. Eight counties comprise the Greater Houston Metroplex: Brazoria, Chambers, Fort Bend, Galveston, Harris, Liberty, Montgomery, and Waller.

### Model 1: Predicting flood likelihood

Here, I built a model to determine the likelihood for flooding to occur by zip code, in light of the flood damage impacts from Hurricane Harvey. I feature engineered a column to determine general likelihood based on the number of claims that FEMA received in each zip code and whether or not each claim was confirmed to have flood damage. That information is then fed back into a model made to enhance the expectation that a zip code is likely to flood.

I generated a model that could predict a proclivity toward flooding within a zip code with 71.33% accuracy. Using that model, I feature engineered an additional column to add to the final model to predict MLS success. This column indicated whether or not the listed home was in a flood prone area. To make this column, I took in the predictions provided by my XGBoost model and filtered them through a system that eliminated zip codes that had fewer than 100 filed claims and a result of fewer than 50% confirmed cases of flooding per zip code.

### Model 2: Predicting if a listed home would sell

The final objective was to predict whether or not a listed home will sell. I took the cleaned MLS data and added the column regarding the flood likelihood depending on which zip code the home is located in from the above model.

The highest performing deep learning models ended up being the neural network model after dropping the low performing features with a test accuracy of 66.78%. The lowest performing features were determined by the XGBoost following the class balancing, and had little to no influence over the overall model performance.

The second highest performing deep learning model was the neural network model using L2 regularization, providing a testing accuracy of 66.68%. L2 regularization is also known as weight decay. This enhanced the baseline neural network model by adding the sum of the squares of all weights in the network to the cost function. This provides the network with the opportunity to learn the smaller weights that are more likely to get overlooked.

The initial XGBoost model I made gave a predictive value of 67.63% accuracy. However, I believe that model was overfit due to class imbalance of 2/3 sold as opposed to not sold results. So I used SMOTE (Synthetic Minority Over-sampling Technique) to address the class imbalance by generating synthetic data based on feature space similarities between existing instances in the minority class.

After trying different models and tuning methods, I was able to produce an XGBoost model that delivered 67.56% accuracy in predicting whether or not a home would sell. This ideal model took the XGBoost model after balancing and removed the 10 features with a weight of 0. This is the model I will use as my final model.

## Conclusions

The biggest indicators of whether or not a home sells are:

* Age of house when listed
* List price
* Lot size

My recommendations to realtors:

* Encourage sellers to list reasonably and renovate with the current trends (build up, not out!)
* Encourage buyers to invest in newer homes, in better school districts
* Take note of flood risk based off of history


## Notebook Structure

1. Cleaning MLS
2. EDA MLS
3. FEMA Registrants
4. Merging FEMA and MLS
5. Model: Flood Likelihood
6. Model: MLS Prediction