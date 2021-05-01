Лабораторна робота № 1. Завантаження та зчитування даних.
=========================================================

1.  За допомогою download.file() завантажте любий excel файл з порталу
    <a href="http://data.gov.ua" class="uri">http://data.gov.ua</a> та
    зчитайте його (xls, xlsx – бінарні формати, тому встановить mode =
    “wb”. Виведіть перші 6 строк отриманого фрейму даних.

``` r
url = 'https://data.gov.ua/dataset/1703061d-e0c4-4393-8a29-fc154d2705fe/resource/9680825b-357c-4a32-9d5b-59e8614037c2/download/assessment_y2020_9m.csv'
d1 <- download.file(url,destfile="assessment_y2020_9m.csv",mode = "wb", encoding="cp1251" )
df1 <- read.csv("assessment_y2020_9m.csv", encoding="cp1251" )
Sys.setlocale("LC_CTYPE", "ukrainian")
```

    ## [1] "Ukrainian_Ukraine.1251"

``` r
head(df1[1:6], encoding="cp1251")
```

    ##   year period
    ## 1 2020      9
    ## 2 2020      9
    ## 3 2020      9
    ## 4 2020      9
    ## 5 2020      9
    ## 6 2020      9
    ##                                                                                                                                company_name
    ## 1                                                                         ДЕРЖАВНЕ ПІДПРИЄМСТВО "НАУКОВО-ДОСЛІДНИЙ ІНСТИТУТ ВИСОКИХ НАПРУГ"
    ## 2                                                                                                                                 ДП "ПУГР"
    ## 3                                                                                              ДРУГИЙ ВОЄНІЗОВАНИЙ ГІРНИЧОРЯТУВАЛЬНИЙ ЗАГІН
    ## 4                                                                                             Восьмий воєнізований гірничорятувальний загін
    ## 5                                                                                             ДЕСЯТИЙ ВОЄНІЗОВАНИЙ ГІРНИЧОРЯТУВАЛЬНИЙ ЗАГІН
    ## 6 ДЕРЖАВНЕ ПІДПРИЄМСТВО "ДЕРЖАВНИЙ НАУКОВО-ДОСЛІДНИЙ, ПРОЕКТНО-КОНСТРУКТОРСЬКИЙ І ПРОЕКТНИЙ ІНСТИТУТ ВУГІЛЬНОЇ ПРОМИСЛОВОСТІ "УКРНДІПРОЕКТ"
    ##   company_code company_type
    ## 1       130441           ДП
    ## 2       147921           ДП
    ## 3       159367           ДП
    ## 4       159427           ДП
    ## 5       159462           ДП
    ## 6       174125           ДП
    ##                                                                                                    company_status
    ## 1 працює; перебуває у процедурі банкрутства - розпорядження майном; Підприємство знаходиться в стадії банкрутства
    ## 2                                                                                                          працює
    ## 3                                                                                                          працює
    ## 4                                                                                                          працює
    ## 5                                                                                                          працює
    ## 6                                                                                                          працює

1.  За допомогою download.file() завантажте файл
    getdata\_data\_ss06hid.csv за посиланням
    <a href="https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv" class="uri">https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv</a>
    та завантажте дані в R. Code book, що пояснює значення змінних
    знаходиться за посиланням
    <a href="https://www.dropbox.com/s/dijv0rlwo4mryv5/PUMSDataDict06.pdf?dl=0" class="uri">https://www.dropbox.com/s/dijv0rlwo4mryv5/PUMSDataDict06.pdf?dl=0</a>

``` r
url = 'https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv'
destfile="getdata%2Fdata%2Fss06hid.csv"
d2 <- download.file(url,destfile,mode = "wb" )
df2 <- read.csv("getdata%2Fdata%2Fss06hid.csv", encoding='utf8')
head(df2[1:6])
```

    ##   RT SERIALNO DIVISION PUMA REGION ST
    ## 1  H      186        8  700      4 16
    ## 2  H      306        8  700      4 16
    ## 3  H      395        8  100      4 16
    ## 4  H      506        8  700      4 16
    ## 5  H      835        8  800      4 16
    ## 6  H      989        8  700      4 16

Необхідно знайти, скільки property мають value $1000000+

``` r
nrow(df2[df2$VAL > 1000000,])
```

    ## [1] 2076

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
