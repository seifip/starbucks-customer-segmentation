_Udacity Data Science Nanodegree project by [Philip Seifi](https://www.seifi.co/)._

_You can find the accompanying blog post on [Medium](https://medium.com/@seifip/starbucks-offers-advanced-customer-segmentation-with-python-737f22e245a4)._

# Starbucks offers: Advanced customer segmentation with Python

A small startup can afford to target users based on broad-stroke rules and rough demographics.

Once a company grows to the size of Starbucks, with millions of daily customers, and $1.6B in credit stored on loyalty cards, they have got to graduate to a more sophisticated method to target their marketing.

One such approach, cluster analysis, uses mathematical models to discover groups of similar customers based on variations in their demographics, purchasing habits, and other characteristics.

Below, I will explore a customer transaction and marketing offer dataset graciously provided by Starbucks.

I will then use Principal Component Analysis (PCA) and the k-means unsupervised Machine Learning algorithm to group these customers into clusters that can be used to automate an effective outreach campaign.

## One month of transaction data
The dataset includes one month of simulated customer data, including their purchasing habits, and interactions with promotional offers.

Each person in the simulation has hidden traits that influence their purchasing patterns, and that are associated with their observable traits.
The dataset consists of three separate JSON files:

1. **Customer profiles ** -  their age, gender, income, and date of becoming a member.

2. **Portfolio ** -  Offers sent during the 30-day test period, via web, email, mobile or social media channels, or a combination thereof. The offers have varying levels of difficulty (minimum spend) and reward, and fall into one of three categories: Discount, Buy-one-get-one (BOGO), Informational

3. **Transcript ** -  A list of offer interactions (receive/view/complete), and all other transactions during the test period.

## Data wrangling
Before I could visualize and model the data, I've had to do some preprocessing both outside, and in Python.

Among others, I have:

1. Removed empty lines in transcript.json using search \n\n & replace \n in VSCode.
2. Imputed empty income values with the mean, and added a separate column that tracks missing income values with 1s
3. Engineered a new column tracking the year when the user became a member
4. One-hot-encoded channels using the MultiLabelBinarizer
5. One-hot-encoded offer types, genders, years joined and event types using get_dummies
6. Dropped outliers (people with age > 99)
7. Engineered first receipt, view and completion time columns (a customer can receive and interact with the same offer multiple times)
8. Dropped misattributions (completion without view, completion before view, or view before receipt)
9. Added RFM Recency and Frequency scores.
10. Engineered view and conversion rate columns for each offer type
11. Merged all data into one dataframe grouped by customers, including means and sums for all available data, as well as additional columns for the average number of exposures per offer-type.

## Customer clusters identified
### Segment 1
Customers in this segment receive regular BOGO offers, and practically no discount offers. These BOGO offers involve more valuable rewards than for customers in other segments.

Their frequency and average order value are not unusual, which suggests these customers are conditioned to BOGOs, and we might have to continue sending them regular offers to keep their patronage.
BOGOs convert really well with customers in this segment, so this is a great lever in times when we need to quickly generate additional sales.

### Segment 2
Customers in this segment receive a higher than average number of offers, and convert really well for both BOGOs and discounts. Demographically, a higher than average share of these customers selected their gender as "Other".

Their average order value is not unusual, in line with the average, but their frequency is above average, probably as a result of the regular offers they receive and act on.

This is another segment we can target to quickly generate additional sales.

### Segment 3
Customers in this segment receive no BOGO offers. They do get occasional discount offers, on which they convert about average, as well as slightly more informational messages than other customers.

These customers have about average frequency and average order value, and would likely continue to frequent Starbucks even if we stopped sending them offers.

### Segment 4
Customers in this segment receive regular offers, which they open, but never convert.

Demographically, they are predominantly male, and lower than average income. They also visit Starbucks less frequently, and make smaller average purchases.

Given the low LTV and low conversion rates for this group, we would be best to avoid targeting them in our marketing.

## Acknowledgements
Dataset by [Starbucks](https://www.starbucks.com/).
