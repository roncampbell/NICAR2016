A brief intro to R

Ronald Campbell 
Data editor, NBC Owned Television Stations
ron.campbell@nbcuni.com 
818 232-2585

R is a highly flexible open source programming language. Developed for
statistical analysis, R will hold its own in that realm against costly
programs such as SPSS. But with more 4,000 add-on packages, it can do
far more, including data manipulation, charts and mapping.

This handout provides  a few survival skills for R rookies.

First, when you see a table or column name (known in R as a "data frame"
and "variable" respectively), pay attention to capitalization. They
matter.

Second, pay attention to the way commands are formatted, especially to
whether they're capitalized or not. That matters too.

Third, pay attention to this little mark here: <- 
It tells R to do something.

Remember that R is a programming language. This is both daunting and,
once you realize its implications, liberating. It means you've acquired
a power tool or maybe a whole factory.

Chances are that you'll use just a handful of those 4,000 packages that
I mentioned earlier. First you must install them on your computer. Then,
every time you want to use them, you must load them. The process works
like this:

To install a package, using ggplot2 as an example:
> install.packages("ggplot2")

To load an already installed package for immediate use:
> library(ggplot2)

Notice that I used quote marks when I installed the package but not when
I loaded it. The R grammar is full of quirks like that.

The core R language, known as "base R", and many packages come with
practice datasets. To view all the available datasets go to the console
pane and type:

> data()
Please note the small "d" in data()

If you want to actually see what's in any of those practice datasets, or
in any of the data frames that you loaded, go to the console and type:

> View(xxxx)  where xxxx is the data frame's name
Please note the capital "V" in View()

Here are some functions to help you survive your first months with R. In
each case, the name of a data frame goes in the parentheses.
* dim()	 gets number of rows, columns (in that order)
* head()	gets first six rows
* tail()	gets last six rows
* names()	gets column names
* nrow()	gets number of rows
* ncol()	gets number of columns

Suggested readings

R Graphics Cookbook
By Winston Chang
O'Reilly
ISBN-13: 978-1449316952
$30.76

I devoured the cookbook like a kid in a candy store. All becomes clear.

R for Everyone: Advanced Advanced Analytics and Graphics
By Jared P. Lander
Addison Wesley
ISBN-13: 978-0321888037
$44.99

An excellent primer on R, this book covers the field from basic
statistics to graphics.

