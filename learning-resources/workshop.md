# Data Workshop

### Introduction
This Data Workshop forms part of the Empty Housing Innovation Lab. Student Data Labs has produced two R tutorials and one QGIS tutorial for the purposes of the Workshop. The tutorials are designed to be quite practical and project-orientated. In R, you'll learn how to explore, visualise and model open data. With QGIS, you'll create a choropleth map to show the percentage of housing stock that's empty across local authorities in London.

## Workshop - Analysing Empty Housing Data using R
R is a an open source statistical programming language that's extremely popular with professionals who make use of data. R is considered fairly difficult for beginners but it's great for performing data visualisation, cleaning, mapping and machine learning. However, while these workshops and projects explore some core concepts in R, they are best used as a springboard for more learning and projects.

In order to carry out this analysis, you'll need RStudio. The computer labs at the Empty Housing Lab have RStudio ready installed but you can download it [here](https://www.rstudio.com/products/rstudio/download/). Once you've opened up RStudio, you'll notice four panes - the top left pane is the text editor, the top right is the workspace, the bottom left is the console and the bottom right the workspace where charts are displayed.

Student Data Labs has prepared two R tutorials for the Data Workshop. The first, Exploring Empty Housing Data, focuses on exploratory data analysis and data visualisation using R. The second is more advanced and looks at correlation, regression and visualisation. You'll need to copy the code from the workshops and paste into the console to get the most out of them. Access the R workshops by following the links below:

1. [Exploration of Empty Housing Data](http://rpubs.com/StudentDataLabs/Empty-Housing-Lab-1)

2. [Modelling of Empty Housing Data](http://rpubs.com/StudentDataLabs/Empty-Housing-Lab-2)

Volunteers are encouraged to follow along point by point but also to explore the data for themselves. To learn more about R, the following books are useful: [R for Everyone](https://www.amazon.co.uk/Everyone-Advanced-Analytics-Graphics-Addison-Wesley/dp/0321888030), [Practical Data Science with R](https://www.amazon.co.uk/Practical-Data-Science-Nina-Zumel/dp/1617291560/ref=sr_1_1?s=books&ie=UTF8&qid=1477691407&sr=1-1&keywords=practical+data+science+with+r) and [R in Action](https://www.amazon.co.uk/Action-Data-Analysis-Graphics/dp/1617291382/ref=sr_1_1?s=books&ie=UTF8&qid=1477691440&sr=1-1&keywords=r+in+action). 

## Workshop - Choropleth Mapping with QGIS
In this very brief introductory tutorial you’ll learn how to create a choropleth map using QGIS. You’ll use open data to create a thematic map displaying empty housing as a percentage of housing stock in London’s local authorities in 2015. Everyone loves a map.

QGIS is a fantastic open source tool for GIS. The Empty Housing Lab takes place in a computer lab that already has QGIS installed but you can download it from the [QGIS site](http://www.qgis.org/en/site/forusers/download.html).

### Open Data
You’ll be using open data on empty housing percentages in 2015 - empty_london_percentage.csv - from 2015 and geographic boundary files - lad_london_2011 - for this workshop. You can download the open data for this workshop from this [Dropbox folder](https://www.dropbox.com/sh/446pg6rxdao1o2u/AAA7aGhH5zL35JA1k_rKnRSVa?dl=0). The datasets have been sourced from the DCLG the [UK Data Service](https://census.edina.ac.uk/bds.html). They have been pre-processed for the purposes of this workshop. It combines open data from two sources: official empty housing data and housing stock data by local authority in London.

### The Workflow
Download our data. Go to the [Dropox folder](https://www.dropbox.com/sh/446pg6rxdao1o2u/AAA7aGhH5zL35JA1k_rKnRSVa?dl=0) and you'll find several datasets. From here, select each file and 'Download'.

Open up our Empty Housing Innovation Lab folder and drag the <b>london_lad_2011.shp</b> file you've dowinto QGIS. You’ll see a colourful empty map showing England’s local authority boundaries. You may need to [change the projection](http://docs.qgis.org/2.2/en/docs/user_manual/working_with_projections/working_with_projections.html) to Google Mercator, depending on your QGIS settings.

![](https://studentdatalabs.files.wordpress.com/2016/10/screen-shot-2016-10-21-at-02-51-08.png)

Open up the empty_london_percentage.csv in Excel and take a look. You’ll notice the following columns:
+ <b>Local_Authority</b>: The name of the local authority
+ <b>New_ONS</b>: The geographic code assigned to the local authorities
+ <b>ALL_Empty_2015_Percent</b>: The percentage of housing stock in a given local authority that is empty
+ <b>LT_Empty_2015_Percent</b>: The percentage of housing stock in a local authority that has been empty for over 6 months

Next, since QGIS doesn’t recognise some character formats, you’ll need to make sure it will be able to read our data. Go ahead and open up a text editor. I use Sublime Text for this purpose but any text editor will do. Given that the first two columns contain mostly strings and the last two integers, you’ll simply need to type the following into the editor:
```
string, string, integer, integer
```
Save this as <u>empty_london_percentage.csvt</u> in the same location as the empty_london_percentage.csv file.

Open up QGIS. Along the top bar you’ll notice Layer. From there, select Add Layer > Add Delimited Text Layer. A new window will appear. Click browse and navigate to our empty_london_percentage.csv file. Don’t use the .csvt file. Make sure ‘CSV (Comma Separated Values)’, ‘First record has field names’ and ‘No geometry (attribute table only)’ have been checked. Following this, you’ll see this file on the left-hand panel.

![](https://studentdatalabs.files.wordpress.com/2016/10/screen-shot-2016-10-21-at-03-11-16.png)

We need to join the two datasets in QGIS. First, select the london_lad_2011.shp and ’Open Attribute Table’, an icon that looks like a table. Since they contain the same values, we'll need to join the ‘New_ONS’ column with the shapefile’s ‘code’ column. Double click on the london_lad_2011.shp to open the Layer Properties. Select ‘Join’ on the left and then the green plus sign along the bottom. The ‘Add vector join’ window will come up. Select <b>New_ONS</b> under ‘Join field’ and <b>code</b> under ’Target field’. Uncheck the ‘Cache join layer in virtual memory’ box. Select ‘OK’. Go back to Open Attribute Table to make sure the join has worked.

![](https://studentdatalabs.files.wordpress.com/2016/10/screen-shot-2016-10-21-at-02-52-49.png)

Let’s design our map. 

1. Along the left hand side of Layer Properties go to ’Style’
2. Select ‘Graduated’ from the drop-down menu
3. Choose our field: empty_london_percentage_ALL_Empty_2015_Percent
4. Choose your colour scale with ‘color ramp’. You have a few options here but we’re going to stick with the oranges scale
5. Below, you’ll see ‘method’. Usually, I use Natural Breaks (Jenks), which classifies the values into the most accurate available groups, or Pretty Breaks, which is highly popular. The methods have their advantages and disadvantages so do some research first. We’ll go with the Natural Breaks (Jenks) method here
6. Under ‘classes’ you can choose how many ramps. We’ll go for 5. Select ‘Classify’ and ‘OK’

![](https://studentdatalabs.files.wordpress.com/2016/10/screen-shot-2016-10-21-at-02-55-39.png)

Great, after a quick zoom we now have a nice choropleth map. There are many things we can do from here to make the map look a bit nicer - change the boundary colours, the colour scale etc. However, since this is a quick tutorial we’ll stick with the basics. We’ll next create a print map with a legend.

Back in QGIS, select ‘New Print Composer’. Leave the title blank. You’ll see a blank page. Along the left, select ‘Add new map’, drag to indicate the size of the map and let go. Along the left, select ‘Add new legend’ and place it below or next to the choropleth map. You can edit the map legend by using the Items panel on the right. To save the map, select ‘Export as image’ at the top and give your map a name. Again, there's lots more you an do from here - add a title, a table, labels etc.

![](https://studentdatalabs.files.wordpress.com/2016/10/screen-shot-2016-10-21-at-03-28-38.png)

### Summary of QGIS Workshop
There you have it, a nice choropleth map showing the percentage of empty homes by local authority in London. As another exercise, try bringing in and mapping other open empty housing data using the same workflow.

In summary, creating a choropleth map is one of the easiest and most common mapping tasks. QGIS can be used to perform many other awesome GIS techniques. If you’re interested in finding out more about QGIS, I would recommend reading the QGIS By Example [book](https://www.amazon.co.uk/QGIS-Example-Alexander-Bruy/dp/1782174672) as well as QGIS Tutorials and Tips [site](http://www.qgistutorials.com/). Thanks to [Steven Bernard](https://www.youtube.com/channel/UCrBM8Ka8HhDAYvQY1VX2P0w/videos), an Interactive Design Editor at the Financial Times, for providing the inspiration for this workshop with two great Youtube videos - you can access [part one](https://www.youtube.com/watch?v=rG6UphZGmg4) and [part two](https://www.youtube.com/watch?v=TN_ltYfQorE).
