## The Problem

[The Eclipse Usage Data Collector (UDC)](https://www.eclipse.org/org/usagedata/). Huge possibilities, right? There's lots of interesting research that uses it, from estimates of [how often people use refactoring tools](http://people.engr.ncsu.edu/ermurph3/papers/tse11a.pdf) to [extracting process models](http://hal.archives-ouvertes.fr/docs/01/01/07/56/PDF/Thesis.pdf).

Since the data is [freely available](http://archive.eclipse.org/projects/usagedata/), the sky's the limit, eh? Not so fast. Have you considered that:
* It'll take you a while to download and process the data. There's about 200GB of CSVs.
* The data's a bit dirty, with unclosed quotes, for instance.
* Once you've got the data set up, is your database going to be able to run queries fast enough? In the past, we've set up a MySQL database with the data, optimized the indices, and it still took hours to complete some queries.

## The Solution

We've put up the entire public data set on [Google BigQuery](https://developers.google.com/bigquery), which enables you to query and process this data quickly using Google's storage and compute infrastructure. This document gives a couple of examples of how you can access and use this data.

## Prerequisites

You'll have to have a Google account and then [create](https://console.developers.google.com) an empty BigQuery-enabled project.

> ### Warning
> BigQuery by default limits the amount of free data processing you can do. At some point, you may run up against that limit; if you do, you've got two choices. First, Google has generously donated copious data processing time in 2014 and 2015 for you to experiment with UDC; just [send an email](mailto:emerson@csc.ncsu.edu?Subject=Request for UDC Data Processing Access) from the associated Google email address. Second, you can buy Google compute time by enabling billing on your account. It's not expensive -- we've always spent less than $5 per month.

## Example 1: The BigQuery Web Interface

The easiest way to run some quick queries is to go straight to the [web interface](https://bigquery.cloud.google.com/table/udc-data:udc.dated_commands). Then, click "Compose Query", and type in

> select count(*) from [udc-data:udc.all];

You should get "2323233101" -- that's about 2.3 billion records. Let's take a quick look at what the data actually looks like:

> select * from [udc-data:udc.all] LIMIT 1;

> Row| userId | what    |kind  |bundleId               |bundleVersion       | description            | time
> --- | --- | --- | --- | --- | --- | --- | ---
> 1  | 335213 | started |bundle|org.eclipse.equinox.app|1.1.0.v20080421-2006| org.eclipse.equinox.app| 239957498477

 You can run quite advanced queries -- check out the [query syntax](https://developers.google.com/bigquery/query-reference). And there are a few other tables and views there to explore:

* 2009_01
* 2009_02
* 2009_03
* 2009_04
* 2009_05
* 2009_06
* 2009_07
* 2009_08
* 2009_09
* 2009_10
* 2009_11
* 2009_12
* 2009_all
* 2010_01
* 2010_02
* 2010_03
* 2010_04
* 2010_05
* 2010_06
* 2010_07
* 2010_08
* 2010_all
* all

dated_commands

## Example 2: Tableau 8.1

If you want to do some serious data analysis, you'll need a serious tool. One such tool you might use is [Tableau](http://www.tableausoftware.com) with the [BigQuery connector](http://www.tableausoftware.com/solutions/google-bigquery) installed. Once you've got that, log in to your BigQuery account and make an (empty) project. Then, load up the sample .twb file in this repository. Check out some of the example visualizations:

***

![](https://github.com/DeveloperLiberationFront/UsageDataCollectorOnBigData/blob/master/example1.png)

***

![](https://github.com/DeveloperLiberationFront/UsageDataCollectorOnBigData/blob/master/example2.png)

***

![](https://github.com/DeveloperLiberationFront/UsageDataCollectorOnBigData/blob/master/example3.png)

***

## Other Solutions

There are a [variety of other](https://developers.google.com/bigquery/third-party-tools) tools for accessing BigQuery, including an Excel plugin and a Java connector.

## Acknowledgements

Thanks to Wayne Beaton for his work on the UDC project and Mohsen Vakilian for the idea to make UDC more easily accessible. Thanks to the Laboratory for Analytic Sciences for seed funding in Google BigData, and to Google for donating the compute power for a year.

If you use this BigData in your research, please cite the following forthcoming book chapter:

`@inbook{snipesASD,`
`author = {Will Snipes and Emerson Murphy-Hill and Thomas Fritz and Mohsen Vakilian and Kostadin Damevski and Anil R. Nair and David Shepherd},`
`publisher = {Morgan Kaufmann},`
`chapter= {A Practical Guide to Analyzing IDE Usage Data},`
`title = {Analyzing Software Data},`
`year = 2014`
`}`