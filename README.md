[![Logo Optimus](https://github.com/ironmussa/Optimus/blob/master/images/logoOptimus.png)](https://hioptimus.com)  
  
[![PyPI version](https://badge.fury.io/py/optimuspyspark.svg)](https://badge.fury.io/py/optimuspyspark) [![Build Status](https://travis-ci.org/ironmussa/Optimus.svg?branch=master)](https://travis-ci.org/ironmussa/Optimus) [![Documentation Status](https://readthedocs.org/projects/optimus-ironmussa/badge/?version=latest)](http://optimus-ironmussa.readthedocs.io/en/latest/?badge=latest)  
 [![built_by iron](https://img.shields.io/badge/built_by-iron-FF69A4.svg)](http://ironmussa.com) [![Updates](https://pyup.io/repos/github/ironmussa/Optimus/shield.svg)](https://pyup.io/repos/github/ironmussa/Optimus/)  
 [![GitHub release](https://img.shields.io/github/release/ironmussa/optimus.svg)](https://github.com/ironmussa/Optimus/) [![Codacy Badge](https://api.codacy.com/project/badge/Grade/e01572e2af5640fcbcdd58e7408f3ea0)](https://www.codacy.com/app/favio.vazquezp/Optimus?utm_source=github.com&utm_medium=referral&utm_content=ironmussa/Optimus&utm_campaign=badger) [![StackShare](https://img.shields.io/badge/tech-stack-0690fa.svg?style=flat)](https://stackshare.io/iron-mussa/devops)  
[![Platforms](https://img.shields.io/badge/platform-Linux%20%7C%20Mac%20OS%20%7C%20Windows-blue.svg)](https://spark.apache.org/docs/2.2.0/#downloading) [![Code Health](https://landscape.io/github/ironmussa/Optimus/develop/landscape.svg?style=flat)](https://landscape.io/github/ironmussa/Optimus/develop) [![Coverage Status](https://coveralls.io/repos/github/ironmussa/Optimus/badge.svg?branch=master)](https://coveralls.io/github/ironmussa/Optimus?branch=master) [![Mentioned in Awesome Data Science](https://awesome.re/mentioned-badge.svg)](https://github.com/bulutyazilim/awesome-datascience)  
[![Join the chat at https://gitter.im/optimuspyspark/Lobby](https://badges.gitter.im/optimuspyspark/Lobby.svg)](https://gitter.im/optimuspyspark/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)  

Optimus is the missing framework to profile, clean, process and do ML in a distributed fashion using Apache Spark(PySpark).

## Installation (pip):  
  
In your terminal just type  `pip install optimuspyspark`
### Requirements
* Apache Spark 2.3.0  
* Python>=3.6  

## Examples

After installation go to the examples folder and found examples about data cleaning, data munging, and how to create ML and DL models.

## Documentation
  
[![Documentation](https://media.readthedocs.com/corporate/img/header-logo.png)](http://docs.hioptimus.com/en/latest/)  
  
## Feedback 
Feedback is what drive Optimus future, so please take a couple of minutes to help shape the Optimus' Roadmap:  https://optimusdata.typeform.com/to/aEnYRY  

Also If you want to know what features are the most requested or have and idea you want to see in Optimus let us know at  
https://optimus.featureupvote.com/  
 
## Info
And if you want to see some cool information and tutorials about Optimus check out our blog https://medium.com/hi-optimus  
  
### Requirements
* Apache Spark 2.3.0  
* Python>=3.6  
  
## Data loading, cleaning and processing
  
Optimus V2 was created to make data cleaning a breeze. The API was designed to be super easy to newcomers and very familiar for people that comes from Pandas.
Optimus expand the Spark DataFrame functionality adding .rows and .cols attributes.

For example you can load data from a url, transform and apply some predefined cleaning functions:

```python
from optimus import Optimus
op = Optimus()

# This is a custom function
def func(value, arg):
    return "this was a number"
    
df =op.load.url("https://raw.githubusercontent.com/ironmussa/Optimus/master/examples/foo.csv")

df\
    .rows.sort("product","desc")\
    .cols.lower(["firstName","lastName"])\
    .cols.date_transform("birth", "new_date", "yyyy/MM/dd", "dd-MM-YYYY")\
    .cols.years_between("birth", "years_between", "yyyy/MM/dd")\
    .cols.remove_accents("lastName")\
    .cols.remove_special_chars("lastName")\
    .cols.replace("product","taaaccoo","taco")\
    .cols.replace("product",["piza","pizzza"],"pizza")\
    .rows.drop(df["id"]<7)\
    .cols.drop("dummyCol")\
    .cols.rename(str.lower)\
    .cols.apply_by_dtypes("product",func,"string", data_type="integer")\
    .cols.trim("*")\
    .show()
```
You tranform this:

```
+---+--------------------+--------------------+---------+----------+-----+----------+--------+
| id|           firstName|            lastName|billingId|   product|price|     birth|dummyCol|
+---+--------------------+--------------------+---------+----------+-----+----------+--------+
|  1|                Luis|         Alvarez$$%!|      123|      Cake|   10|1980/07/07|   never|
|  2|               André|              Ampère|      423|      piza|    8|1950/07/08|   gonna|
|  3|               NiELS|          Böhr//((%%|      551|     pizza|    8|1990/07/09|    give|
|  4|                PAUL|              dirac$|      521|     pizza|    8|1954/07/10|     you|
|  5|              Albert|            Einstein|      634|     pizza|    8|1990/07/11|      up|
|  6|             Galileo|             GALiLEI|      672|     arepa|    5|1930/08/12|   never|
|  7|                CaRL|            Ga%%%uss|      323|      taco|    3|1970/07/13|   gonna|
|  8|               David|          H$$$ilbert|      624|  taaaccoo|    3|1950/07/14|     let|
|  9|            Johannes|              KEPLER|      735|      taco|    3|1920/04/22|     you|
| 10|               JaMES|         M$$ax%%well|      875|      taco|    3|1923/03/12|    down|
| 11|               Isaac|              Newton|      992|     pasta|    9|1999/02/15|  never |
| 12|              Emmy%%|            Nöether$|      234|     pasta|    9|1993/12/08|   gonna|
| 13|              Max!!!|           Planck!!!|      111|hamburguer|    4|1994/01/04|    run |
| 14|                Fred|            Hoy&&&le|      553|    pizzza|    8|1997/06/27|  around|
| 15|(((   Heinrich )))))|               Hertz|      116|     pizza|    8|1956/11/30|     and|
| 16|             William|          Gilbert###|      886|      BEER|    2|1958/03/26|  desert|
| 17|               Marie|               CURIE|      912|      Rice|    1|2000/03/22|     you|
| 18|              Arthur|          COM%%%pton|      812|    110790|    5|1899/01/01|       #|
| 19|               JAMES|            Chadwick|      467|      null|   10|1921/05/03|       #|
+---+--------------------+--------------------+---------+----------+-----+----------+--------+
```

to this:
```
+---+---------+--------+---------+-----------------+-----+----------+----------+-------------+
| id|firstname|lastname|billingid|          product|price|     birth|  new_date|years_between|
+---+---------+--------+---------+-----------------+-----+----------+----------+-------------+
| 10|    james| maxwell|      875|             taco|    3|1923/03/12|12-03-1923|      95.4355|
| 11|    isaac|  newton|      992|            pasta|    9|1999/02/15|15-02-1999|      19.5108|
| 12|     emmy| noether|      234|            pasta|    9|1993/12/08|08-12-1993|      24.6962|
| 13|      max|  planck|      111|       hamburguer|    4|1994/01/04|04-01-1994|      24.6237|
| 14|     fred|   hoyle|      553|            pizza|    8|1997/06/27|27-06-1997|      21.1452|
| 15| heinrich|   hertz|      116|            pizza|    8|1956/11/30|30-11-1956|      61.7204|
| 16|  william| gilbert|      886|             BEER|    2|1958/03/26|26-03-1958|      60.3978|
| 17|    marie|   curie|      912|             Rice|    1|2000/03/22|22-03-2000|      18.4086|
| 18|   arthur| compton|      812|this was a number|    5|1899/01/01|01-01-1899|     119.6317|
| 19|    james|chadwick|      467|             null|   10|1921/05/03|03-05-1921|       97.293|
|  7|     carl|   gauss|      323|             taco|    3|1970/07/13|13-07-1970|      48.0995|
|  8|    david| hilbert|      624|             taco|    3|1950/07/14|14-07-1950|      68.0968|
|  9| johannes|  kepler|      735|             taco|    3|1920/04/22|22-04-1920|      98.3253|
+---+---------+--------+---------+-----------------+-----+----------+----------+-------------+
```
Note that you can use Optimus functions and Spark functions(`.WithColumn()`) at the same time. To know about all the Optimus functionality please go to this notebooks

### Custom functions
Spark have multiple ways to transform your data like rdd, Column Expression ,udf and pandas udf. In Optimus we create the `apply()` and `apply_expr` which handle all the implementation complexity.

Here you apply a function to the "billingid" column. Sum 1 and 2 to the current column value. All powered by Pandas UDF
```python
from optimus import Optimus
op = Optimus()
  
df =op.load.url("https://raw.githubusercontent.com/ironmussa/Optimus/master/examples/foo.csv")

def func(value, args):
    return value +args[0] + args[1]

df.cols.apply("billingid",func,"int", [1,2]).show()
```

If you want to apply a Column Expression use `apply_expr()` like this. In this case we pasa an argument 10 to divide the actual column value

```python
from pyspark.sql import functions as F
from optimus import Optimus
op = Optimus()
  

df =op.load.url("https://raw.githubusercontent.com/ironmussa/Optimus/master/examples/foo.csv")

def func(col_name, args):
    return F.col(col_name)/20

df.cols.apply_expr("billingid", func, 20).show()
```
 
## Data profiling

Optimus comes with a powerful and unique data profiler. Besides basic and advance stats like min, max, kurtosis, mad etc, 
it also let you know what type of data has every column. For example if a string column have string, integer, float, bool, date Optimus can give you an unique overview about your data. 
Just run `df.profile("*")` to profile all the columns. For more info about the profile please go to this notebooks

## Machine Learning 

Favio optimus is calling you

## Deep Learning
 
Favio optimus is calling you
 
## Contributing to Optimus
Contributions go far beyond pull requests and commits. We are very happy to receive any kind of contributions   
including:  
  
* [Documentation](https://github.com/ironmussa/Optimus/tree/master/docs/source) updates, enhancements, designs, or   bugfixes.  
* Spelling or grammar fixes.  
* README.md corrections or redesigns.  
* Adding unit, or functional [tests](https://github.com/ironmussa/Optimus/tree/master/tests)   
* Triaging GitHub issues -- especially determining whether an issue still persists or is reproducible.  
* [Searching #optimusdata on twitter](https://twitter.com/search?q=optimusdata) and helping someone else who needs help.  
* [Blogging, speaking about, or creating tutorials](https://hioptimus.com/category/blog/)   about Optimus and its many features.  
* Helping others in our optimus [gitter channel](https://gitter.im/optimuspyspark/Lobby).    
  
## Backers  
[[Become a backer](https://opencollective.com/optimus#backer)] and get your image on our README on Github with a link to your site.  
[![OpenCollective](https://opencollective.com/optimus/backers/badge.svg)](#backers)   
  
  
## Sponsors  
[[Become a sponsor](https://opencollective.com/optimus#backer)] and get your image on our README on Github with a link to your site.  
[![OpenCollective](https://opencollective.com/optimus/sponsors/badge.svg)](#sponsors)  
  
## Optimus for Spark 1.6.x  
  
Optimus main stable branch will work now for Spark 2.2.0 The 1.6.x version is now under maintenance, the last tag release for this Spark version is the 0.4.0. We strongly suggest that you use the >2.x version of the framework because the new improvements and features will be added now on this version.
## Core Team
Argenis Leon
Favio Vazquez

## Contributors:
Here is the amazing people that make Optimus possible:
  
[![0](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/0)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/0)  
[![1](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/1)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/1)  
[![2](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/2)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/2)  
[![3](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/3)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/3)  
[![4](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/4)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/4)  
[![5](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/5)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/5)  
[![6](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/6)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/6)  
[![7](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/images/7)](https://sourcerer.io/fame/FavioVazquez/ironmussa/Optimus/links/7)  
  
[![Logo Data](https://www.bbvadata.com/wp-content/uploads/2016/07/bbvada_logo.png)](https://www.bbvadata.com)  
   
## License:  
  
Apache 2.0 © [Iron](https://github.com/ironmussa)  
  
[![Logo Iron](https://iron-ai.com/wp-content/uploads/2017/08/iron-svg-2.png)](https://ironmussa.com)  
  
<a href="https://twitter.com/optimus_data"><img src="https://www.shareicon.net/data/256x256/2015/09/01/94063_circle_512x512.png" alt="Optimus twitter" border="0" height="60"></a>