Data generated within this folder utilizes two new methods described below in order to generate a detailed dataset of the demographics,
energy usage, and energy costs for simulated samples of each household within each tract in Colorado.

# HOUSEHOLD SAMPLING:
Within the detailed household data set, `CO_estimates_all_households.zip`, demographics are roughly constrained by the ACS 2015-2019 counts of households within each income bracket, counts of households with each heating type, counts of households by number of members in their household (from 1 to 7+), counts of owners versus renters, and counts by home type (mobile, single detached unit, apartment, etc...). Importantly, correlations between these variables are preserved. For example, we would not expect a high income household with 6 members to live in a mobile home. To keep relationships between these variables, we build this list of households by sampling from households within the RECS 2015 survey of household demographics and energy use from the EIA. We then merge this dataset with variables that are shared across a given census tract (e.g. climate or urban/rural status). Further deomgraphic variables of median or average values across a tract may also been merged. Some these can, in principle, also be broken down but, due to limited data from the RECS survey, are included only as averages. For example, each household may have a value for the _fraction_ of individuals identifying as non-Hispanic White. This value is not intended to describe that household but the average value of the tract in which that household resides.

One goal of this analysis is to align incomes to federal poverty levels which depend on the household size. Since incomes are reported in income brackets (e.g. $0k-$20k), I assign incomes to households using a uniform random distribution with the bracket. This leads to two important notes. First, the highest income bracket has no upper limit, so the incomes for all housheholds within that bracket are $140,001. Second, for the lowest bracket, incomes can be as low as $0 which can lead to energy cost burdens much greater thatn 100\%. To adress that, I set incomes in the lowest income bracket to within $5k-$20k instead.

# ENERGY ESTIMATES:
The second new method involves a modification of the model used to make energy estimates. This is less important to understand for
data use since it acts in a similar way to a method previously used (Zeke's paper). The first step is to build a predictive
model from the RECS household survey that can estimate how much energy is being used of each fuel type (for Colorado we consider natural gas,
electricity, liquid propane, and wood) and for each end use (space heating, space cooling, water heating, and other/appliances). Then, we simply
apply that model to predict the energy use by type for each of the households in the dataset described above. Whereas previous work used an
econometric linear model to make such predictions, we use here a random forest algorithm to make such predictions. We find this method
typically provides greater accuracy and is less sensitive to the structure of the model and thus more reliable and robust.

# DATASETS:
For each, I have aggreagted data at three different scales. The smallest scale is the household data. From this dataset, I then take averages
and sums, where appropriate to the tract scale and to the county scale. Following is a guide to this columns available for each dataset.

## HOUSEHOLD SCALE COLUMNS:
Columns that are sampled from the RECS household data are presented here as binary True/False type columns. Detailed lists are to come.
