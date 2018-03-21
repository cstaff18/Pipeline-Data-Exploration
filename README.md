# Pipeline-Data-Exploration


## Data

The dataset, provided by the Department of Transportation's Pipeline and hazardous materials safety administration via Kaggle.com, provides records of pipeline accidents, date, time and location of the accident and other fields like cause of accident, injuries, oil/gas spilled, etc.

## Location Mapping of Pipeline Accidents

Using python's folium library I created a heatmap of all recorded accidents. You can view the interactive maps along with the source code through NBviewer with the links below.

[All Accidents from Jan 2010 through Jan  2017](http://nbviewer.jupyter.org/github/cstaff18/Pipeline-Data-Exploration/blob/master/src/TotalAccidentsMap.ipynb)

[Monthly Accidents from Jan 2010 to Jan 2017](http://nbviewer.jupyter.org/github/cstaff18/Pipeline-Data-Exploration/blob/master/src/MonthlyAccidentsMap.ipynb)

With these graphs you can see the most common locations for spills such as Houston, New York and Oklahoma, but also how they change over time.

## Time Analysis of Pipeline Accidents

It appeared that accidents were becoming more frequent over time so I wanted to dig in deeper and figure out what was going on. Below shows the yearly break down of accident counts.

![Yearly Accident Count](https://github.com/cstaff18/Pipeline-Data-Exploration/blob/master/images/AccidentsByYear.png "Accidents By Year")

Accidents were certainly increasing but I wanted to know if the increase indicated a significant change in accident rate.  I performed a T-test to test wether the accident rate in 2016 was statistically different from 2010 with a significance level on 95%.

Results:

T-Statistic: | P-Value: | Significant Change?
--- | --- | ---
-1.085| 0.030 | Yes

#### Switch Point Analysis

Lets take a look at the daily accident count for the whole range of our data set.

![Daily Accident Count](https://github.com/cstaff18/Pipeline-Data-Exploration/blob/master/images/DailyAccidentGraph.png "Accidents By Day")

Now lets assume that the increase in accident rate happened on a single day.  With the PyMC3 library we can use a bayesian switch point analysis to find the accident rates (lambda) before and after the switch point as well as what day the switch point occurred (tau).

![Single Switch Point](https://github.com/cstaff18/Pipeline-Data-Exploration/blob/master/images/1SwitchPoint.png "Switch Point Analysis")

The results indicate that there are two distinct accident rates, around 0.95 accidents per day and 1.2 accidents per day. The switch point is around 2012 with the most likely day, day 1092, corresponding to December 27th 2012. There is another clusterings of likely switch dates that correspond to roughly to end of year 2013. This could potentially indicate that there are two switch dates. Let's see how our model reacts if we add another lambda and tau term to simulate two step changes in the frequency of accidents.

![Two Switch Point](https://github.com/cstaff18/Pipeline-Data-Exploration/blob/master/images/2SwitchPoint.png "2 Switch Points Analysis")
