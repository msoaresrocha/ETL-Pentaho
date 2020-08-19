# ETL-Pentaho
My work with Pentaho Data Integration
This README is and explanation of the MAXIMO x EBS Oracle integration, but will work for the ImportSys x EBS Oracle integration.


## Introduction
  This project was the first one that I worked using pentaho data integration. I started to create this because I used to make a SQL Query that was finished around 1 minute, but the ccompany stop to use exadata and the query was enden about 30 minutes. This project Is simple but was a great solution beccause of the time of response of the SQL query.

## How to do
### Transformation
First of all, you have to create the connection of  the databases that you are going to use.\
By folowing the steps you will create the connection, in this case was in a oracle database.

![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%201.png)


Then you have to go to the option desing and choose the inputs, in this case is a sql query.\
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%202.png)


Then you will choose the name of the step, connection and your sql code.
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%203.png)

The codes that I used in SQL.

~~~~sql
SELECT 
	    upper(trim(a.prnum)) prnum
     ,upper(trim(a.siteid)) siteid
	   ,upper(trim(b.itemnum)) itemnum
 FROM maximo.pr a
     ,maximo.prline b
WHERE 
      a.prnum = b.prnum 
  AND a.siteid = b.siteid
  AND a.prnum not like '%IN-%'
  AND a.status = 'APPR'
  AND trunc(a.statusdate) >= trunc(last_day(sysdate))-30
~~~~

~~~~sql
SELECT 
	    upper(trim(a.prnum)) prnum
     ,upper(trim(a.siteid)) siteid
	   ,upper(trim(b.itemnum)) itemnum
 FROM maximo.pr a
     ,maximo.prline b
WHERE 
      a.prnum = b.prnum 
  AND a.siteid = b.siteid
  AND a.prnum not like '%IN-%'
  AND a.status = 'APPR'
  AND trunc(a.statusdate) >= trunc(last_day(sysdate))-30
~~~~

Then you will make and order by, choose the left join and make a filter. In this case the Purchase Requisition that was integrated with the Oracle EBS was to a dummy and the PR's that was not integrate go to a TXT file that was used to inform the responsables of this PR's (most cases was because there was a missing field).
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%204.png)
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%205.png)
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%206.png)
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%207.png)


### Job
Above was how to make and transformation and now is how to make this a job for beeing running in a server.
Create the job is the most simple part of all.
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%208.png)

You will make an start option and schedule for running each hour, day...

![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%209.png)
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2010.png)

then you will exclude the txt file (sometimes the txt old was inputed with old querys, so i decided to exclude the file before the transformation starts)\
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2011.png)
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2012.png)
you just will put the path where the txt is.

then you will put the transformation.\
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2013.png)
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2014.png)

Then, I make a simple verification, after the transformation... if there is any TXT file, the job will create and email for our IT support and if there is no TXt file, the job will do nothing (dummy) and will close.\
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2015.png)

### Conclusion
This is how I made and simple verification of integration the I did not use any DBeaver or SQL Developer for look to integrations and the time of this Job in petaho was less and 2 minutes.

#### The Importsys x EBS Oracle Project.
Was with the same idea.
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2016.png)
![Alt text](https://github.com/msoaresrocha/ETL-Pentaho/blob/master/Images/Image%2017.png)
