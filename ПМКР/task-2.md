Зчитайте xml файл за посиланням
<a href="http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml" class="uri">http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml</a>
Скільки ресторанів мають zipcode 21231?

``` r
library("XML")
```

    ## Warning: package 'XML' was built under R version 4.0.5

``` r
doc <-  xmlTreeParse("http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml",useInternal = TRUE)
# Exract the root node form the xml file.
rootnode <- xmlRoot(doc)
sum(xpathSApply(rootnode,"//zipcode",xmlValue) == 21231)
```

    ## [1] 127
