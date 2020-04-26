## Recreating the Gapminder Plot in PowerBI
### Will COVID19 Alter the Trend for Year 2020?



During the past weeks, I've seen a lot of visualizations on the impact of COVID19 to global health and economy. More than an arm's race to find a cure for the disease, COVID19 has spurred a lot of interests to visualizations as it applies to fighting the pandemic.

And I'm going to jump in the bandwagon. 

For a beginner, I think the perfect for this is the famous Gapminder graph by Professor Hans Rosling. Professor Rosling has done a fantastic job in telling a story using this plot and since then it has become the defacto beginner data visualization project.

youtube here

Without further ado, let's do the plot!


### Preparing Data
First, we have to get the data to be used in the visualization. You can download the data from the Gapminder website here https://www.gapminder.org/data/ or my github repo TODO

The data pertains to the following

* Life Expectancy in years
* Income per person (GDP/capita, PPP$ inflation-adjusted)
* Population, Total
* Regions of the World

Opening up the CSV files, you would notice that they are in the wide format (except for the Regions) like this

life_expectancy_wide.png

We have to convert our data to long or the tidy format in order to create the Gapminder plot.

Let's start with the Life Expectancy
1. Create a new PowerBI file and go to Home > Get Data > Text/CSV. Choose the Life Expectancy CSV file
2. In the PowerQuery editor, promote the first row as column headers.
3. To turn into tidy format, choose the geo column and click on Transform > Unpivot Other Columns. Basically, we want the years and the values to form new columns.
4. Rename the columns to Country, Year, and Life Expectancy respectively.
5. Trim the Country for trailing spaces

The data should now look like as follows

life_expectancy_long.png

Repeat the above procedures for the Income and Population Data. After doing so, the Income and Population Data should like as follows

income_per_person_long.png

population_long.png

The Region data is already in the long or tidy format. After importing, do the following
1. Rename the name column to Country
2. Drop the other columns except for the Country, region and sub-region columns. 
3. The Antarctica does not have a region or sub-region indicated. We just assign Antarctica to these columns by creating conditional columns.
    For example, 
    
    conditional_col.png

    Drop the old columns afterwards

4. Trim the Country for trailing spaces

The Regions should now look like this

### Combining the Data
The next step is to combine the Life Expectancy, Income, and Population data. We have to do this for three reasons

1. Our data does not have the same number of rows. The Life Expectancy data comes with 186 rows, Income data with 193 rows, and the Population with 195 rows.

2. We'll limit the Years column to multiples of 5. This is to make the trend more noticeable once we animate the data.

Let's start with combining our data. 
1. Create a new merge query (New Query > Combine > Merge Queries as New)
2. Merge the Life Expectancy and Income data as follows. Make sure that Join Kind is Left Outer

merge.png

We choose the Life Expectancy first as it has the lowest number of rows. Buy choosing Left Join, the countries with no matching life expectancy will be removed in the resulting merge. This is to make sure that all countries have the life expectancy, income, and population once merging is done.

3. Expand the columns to include the Income per Person data.
expand_col.png

Repeat the above process for the Population data. The data should look like as follows in the end

merge_end.png

### Filtering the Data
In order to make the trend more noticeable once we animate the data, let's filter our Years column to 5 year interval. We can do this with the Number.Mod

1. Create a new column as follows

div5col.png

2. Filter the custom column to include only 0 or the years divisible by 5. Remove the custom column afterwards

div5.png

The resulting data should now like as follows

filtered.png

Rename the combined data to Gapminder

### Visualizing the Data
We're now finally ready to create our visualization. To create the Gapminder plot.
1. Drag and drop the Scatter to our PowerBI window. Resize as appropriate.
scattter.png
2. Drag the fields to appropriate axes in our plot

    X-axis - Gapminder[Income Per Person]
    Y-axis - Gapminder[Life Expectancy]
    Size - Gapminder[Population]
    Play Axis - Gapminder[Year]
    Details - Gapminder[Country]

    field_axes1.png

3. Let's also color code our Countries based on the Subregion. To do this, let's create a relationship first between our Gapminder (merged) data and the Regions data. Go to Modelling > Manage Relationships. Create relationship as follows

rel.png

Note that I chose to utilize relationships instead of merging the data as it's not as messy as the other data. The primary key for both data now is the Country column only.

4. Let's format our chart to make it cleaner
    
    X-axis - make it a logarithmic scale, max of 120,000
    Y-axis - summarize by using Average Life Expectancy
    Legend - put in the Right
    Category Labels - turn it on to show Country names
    Title - put "How Does Income Relate to Life Expectancy?"

The final plot now looks like this 


gapminder_final.png


---

Now, I would like to point out the plot during 1910, 1915, & 1920


1910.png
1915.png
1920.png

A few dots have moved lower in 1920 compared to 1910 and 1915. This is of course due to the Spanish Influenza that last 36 months from January 1918 to December 1920 infecting 500 million people - about a third of the world's population at the time.

Will the COVID19 do the same for the year 2020?  

We can't really tell and I'm hoping not. However, I'm excited to do recreate this plot once data for year 2020 comes out. 





