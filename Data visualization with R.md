Data visualization with R

Ronald Campbell 
Data editor
NBC Owned Television Stations
ron.campbell@nbcuni.com 
818 232-2585

This handout explains R guru Hadley Wickham's ggplot2 package, which
revolutionized graphics in R. It is based on a "grammar of graphics". 

That sounds more complicated than it should. The idea is really simple. Here it
is:

Every graphic must have at least three elements:
1. Data (well, d'oh)
2. Aesthetics -- a fancy way of saying a scale or scales on which the graphic is
drawn
3. Geometries -- visual elements

Graphics may have other elements -- everyone has seen maps with embedded charts
or inset locators -- but those first three elements are vital. Once you
understand the grammar, you can figure out the rest.

For this exercise you must install and load each of the following packages:

* ggplot2
* dplyr
* RColorBrewer
* nycflights13

Reminder: To install a package, in the console
> install.packages("ggplot2")
To load a package, in the console
> library(ggplot2)
Use quote marks in the install.packages() statement; do not use in the library()
statement. 

SCATTER PLOTS

Scatter plots represent points on an X-Y graph. They're good for demonstrating
relationships, for example between height and weight.

We'll start by exploring the relationship between heat and humidity in the
weather data frame, part of the nycflights13 package. Here's a basic scatter:

> ggplot(weather, aes(x=humidity, y = temp)) + geom_point()

Notice the elements: 1) data = weather, the name of the data frame; aesthetics,
shortened to "aes", with the x and y axes assigned to variables in the data
frame; geometries, shortened to "geom" and specified as a "point". As you'll
see, we can specify other kinds of geoms.

The default color in ggplot2 is black, but we can change that.

> ggplot(weather, aes(x=humidity, y = temp)) + geom_point(color="red")

All the dots turn red. Nothing else changes.

However, we also can use color to add information to the graphic. Suppose, for
example, that we want to add a value to represent weather conditions in addition
to temperature and humidity; we can color the dots to do so.

> ggplot(weather, aes(x=humidity, y = temp, col = conditions)) + geom_point()

HISTOGRAMS

Histograms are a sophisticated means of graphically organizing and counting
values. In statistical terms, a histogram analyzes the frequency distribution of
a variable. With ggplot we can set the boundaries of the frequencies (known as
"bins"); we also can overlay one value on top of another for comparison.

We'll open one of the datasets that comes with ggplot, msleep. This is a record
of average sleep time and average REM time for 83 mammals. Here's a basic
histogram:

> ggplot(msleep, aes(x=sleep_total)) + geom_histogram()

Since we are just counting frequencies, there is no y-axis to the histogram. We
are simply counting occurrences of values along the x-axis. 

But we can get more information in the histogram. For example, the underlying
data frame contains a categorical variable, vore, that tells us whether each
animal is a plant-eater, meat-eater, insect-eater or omnivore. Maybe that has a
relationship to sleep patterns. We easily can chart it.

> ggplot(msleep, aes(x=sleep_total, fill=vore)) +
geom_histogram(position="identity", alpha=0.4)

We're doing three new things here. First we're using "fill" - in this context,
color - to represent the categorical variable vore. Second we're telling ggplot
to start each of the four vores at the base line (zero) instead of stacking them
as the software normally would do. Finally, that "alpha=0.4" sets the opacity at
40%, which is why you can see through some of the bars -- and a good thing since
they all start at the bottom. 

BOX PLOTS

Box plots display the distribution of values with respect to the median. Half of
the values, everything between the 25th and 75th percentile, are within the box.
The median itself is represented by a thick horizontal line. The range between
the 25th and 75th percentiles is known as the Inter-Quartile Range, or IQR. The
vertical lines extending above and below the box are known as "whiskers", and
they extend 1.5 times the IQR. Any values above or below the whiskers are, by
definition, outliers. So the box plot tells you at a glance what's normal and
what's an outlier.

To make a box plot we need a categorical variable and a continuous variable.

Let's look at one of the practice datasets in the ggplot package, midwest. This
contains several stats for Midwest counties.

> View(midwest)

One variable, inmetro, contains a 0 for rural counties and a 1 for urban
counties. We need to convert that variable to a factor, a categorical variable
in R. We can do that on the fly. And then we'll see if there's a difference in
percentages of college grads between rural and urban Midwestern counties:

> ggplot(midwest, aes(x=factor(inmetro), y=percollege)) + geom_boxplot()

And there is a difference. But note the large number of outliers, particularly
among rural counties.

A box plot can quickly alert you to outliers, at once the most fascinating and
potentially misleading data points around.

BAR CHARTS

We're going to look at another ggplot dataset: economics.

> View(economics)
This is a collection of monthly economic data from 1967 to 2007. We want to look
at a snapshot of annual numbers: just the June data from every year. To do that
we'll filter the data using the wonderful dplyr package.

> library(dplyr)
> juneecon <- economics %>%  # shift return
	filter(month==6)
> View(juneecon)

Now we have a new data frame consisting just of economic data collected every
June from 1967 through 2007. 

> ggplot(juneecon, aes(x=year, y=unemploy)) + geom_bar(stat="identity")

The command geom_bar(stat="identity") tells the bar chart to take the values (or
identities) that we've assigned.

We can change the colors of the bar easily.

> ggplot(juneecon, aes(x=year, y=unemploy)) + geom_bar(stat="identity",
fill="lightblue", color="black")

We can also add labels for the axes.

> ggplot(juneecon, aes(x=year, y=unemploy)) + geom_bar(stat="identity",
fill="lightblue", color="black") + xlab("Year") + ylab("Unemployed (000)")

Let's return to the Midwestern county data. Take another look at it.

> View(midwest)

We have the percent in poverty by county. Does the poverty rate differ for rural
and urban counties? Does it differ substantially for rural and urban counties by
state? We can find out with a special kind of bar chart.

> ggplot(midwest, aes(x=state, y=percbelowpoverty, fill=factor(inmetro))) +
+     geom_bar(stat="identity", position=position_dodge()) +
+     xlab("State") + ylab("Percent in Poverty")

We've already encountered the command factor(). It tells R to convert a variable
to a categorical variable. In this case the variable inmetro has one of two
values, 0 or 1 for rural or urban. But what about position=position_dodge()? The
default in ggplot is for a bar chart to stack values on top of each other; in
this case rural and urban counties in the same state would be stacked. By
specifying position_dodge, we tell R to place urban and rural bars side by side.

The bar chart tells us valuable information: In almost every state, and
particularly in Wisconsin, poverty rates are much higher in rural counties than
in urban counties.

But the color sucks. There is, however, a solution.

library(RColorBrewer)

> ggplot(midwest, aes(x=state, y=percbelowpoverty, fill=factor(inmetro))) +
+     geom_bar(stat="identity", position=position_dodge()) +
+     scale_fill_brewer(palette="Reds") +
+     xlab("State") + ylab("Percent in Poverty")

LINE CHARTS

Line charts are great for showing trends over time. Let's return to the economic
data.

> View(economics)
> ggplot(economics, aes(x=date, y=uempmed)) + geom_line()

The result is a very jagged line. We're looking at 480 months of unemployment
stats. But remember, we also created a data frame of stats for the month of June
only:

> View(juneecon)
> ggplot(juneecon, aes(x=date, y=uempmed)) + geom_line()

This is easier to read. To make it still easier, let's add points:

> ggplot(juneecon, aes(x=date, y=uempmed)) + geom_line() + geom_point() 

FACETS

With facets we can compare several graphs side by side. Let's turn back to the Midwest dataset and look at high school graduation rates for rural and urban counties by state to see how faceting works.

> View(midwest)
> ggplot(midwest, aes(x=factor(inmetro), y=perchsd)) + geom_boxplot() +
+     facet_grid(state~.)

The first part of the syntax is easy to understand -- a simple box plot comparing urban and rural counties for percentage high school graduation rates. The next part requires some explanation. "facet_grid(state~.)" creates a grid based on the variable state and then takes all the data previously gathered -- that's the meaning of the period -- and puts it in the grid.