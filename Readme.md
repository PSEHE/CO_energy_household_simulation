Data generated within this folder utilizes two new methods described below in order to generate a detailed dataset of the demographics,
energy usage, and energy costs for simulated samples of the 2 million+ households in Colorado. To see example analysis using this dataset, view the [notebook](https://github.com/PSEHE/CO_energy_household_simulation/blob/main/sample_household_plots.ipynb).  

# Sampling households from household survey to align with ACS population counts:
Within the detailed household data set, `CO_estimates_all_households.zip`, household variables are roughly constrained by the ACS 2015-2019 counts of...
1. households within each income bracket, 
2. households with each heating type, 
3. households by number of members in their household (from 1 to 7+), 
4. owners versus renters
5. home type (mobile, single detached unit, apartment, etc...). 

Importantly, correlations between these variables are preserved. For example, we would not expect a wealthy household with 6 members to live in a mobile home and use propane heat. To keep relationships between these variables, we build this list of households by sampling from households within the RECS 2015 survey of household demographics and energy use from the EIA. We then merge this dataset with variables that are shared across a given census tract (e.g. climate or urban/rural status). Further deomgraphic variables of median or average values across a tract can also been merged. Further variables present in both the ACS and RECS data such as race or educational attainment can also be broken down to individual households such but, due to limited data from the RECS survey, we omit those for now although they may be including for further analysis. For example, each household may have a value for the _fraction_ of individuals identifying as non-Hispanic White. This value is not intended to describe that household but instead the average value of the tract in which that household resides.

One goal of this analysis is to align incomes to federal poverty levels which depend on the household size. Since incomes are reported in income brackets (e.g. $0k-$20k), I assign incomes to households using a uniform random distribution with the respective bracket for each household. This leads to two important notes. First, the highest income bracket has no upper limit, so the incomes for all housheholds within that bracket are $140,001. Second, for the lowest bracket, incomes can be as low as $0 which can lead to energy cost burdens much greater thatn 100%. To adress that, I set incomes in the lowest income bracket to within $5k-$20k instead.

# ENERGY ESTIMATES:
The second new method involves a modification of the model used to make energy estimates. This is less important to understand for
data use since it aims for the same goal as the previously used method (Zeke's paper). In short, the first step is to build a predictive
model from the RECS household survey that can estimate how much energy is being used of each fuel type (for Colorado we consider natural gas,
electricity, liquid propane, and wood) and for each end use (space heating, space cooling, water heating, and other/appliances). Then, we simply
apply that model to predict the energy use by type for each of the households in the dataset described above. Whereas previous work used an
econometric linear model to make such predictions, we use here a random forest algorithm to do so. We find this method
typically provides greater accuracy and is less sensitive to the structure of the model and thus more reliable and robust.

# DATASETS:
I have aggreagted data at the county scale to match that of the Fisher, Sheehan, and Colton analysis. I simply take sums within each county and within households in each poverty bracket (e.g. between 50-100% of the federal poverty level). 

## HOUSEHOLD SCALE COLUMNS:
Columns that are sampled from the RECS household data are presented here as binary True/False type columns. Detailed lists are to come.
