**HIVE**
<br>

First i showed output and codes of preprocessing. After preprocessing steps are completed query for each question is shown at the end of this file.

**If you want to refer to problems&answers directly please look at the end of document**

Intially copied dataset files into cluster machine using pscp command in cli.
<br><br>
Command: pscp localpath remoteip:remotepath
<br>
Following command is used to transfer tvshow_channel files and tvshow_Views files:
<br>
pscp C:\Users\123\Desktop\join2_genchanFiles darkweb36998465@e.cloudxlab.com:/home/darkweb36998465/tvchh<br> 
Pscp C:\Users\123\Desktop\join2_gennumFiles darkweb36998465@e.cloudxlab.com:/home/darkweb36998465/tvnum <br>
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/copied_files.PNG" height="80" width="620" hspace="5" vspace="5"/>

Then created database with name as bigdata_task.
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/create_db.PNG" height="90" width="620" hspace="5" vspace="5"/>

Table1 tvshow_channel is created. And also loaded tvshow and channel data into table tvshow_channel.
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/table1_created.PNG" height="120" width="720" hspace="5" vspace="5"/><br>
Showing number of records in table tvshow_channel.
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/no_records_table1.PNG" height="200" width="720" hspace="5" vspace="5"/><br>

Table2 tvshow_views is created. And then loaded tvshow and views data into table tvshow_views.
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/table2_creation.PNG" height="120" width="720" hspace="5" vspace="5"/><br>
Showing number of records in table tvshow_views.
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/no_of_records_table2.PNG" height="200" width="720" hspace="5" vspace="5"/><br>

Table tvshow_channel is updated from (tvshow,channel) to (channel,array(tvshow)) into new table tvshow_channel_updated.
 <img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/updated_table1.PNG" height="270" width="720" hspace="5" vspace="5"/><br>
Number of rows of updated tvshow_channel_updated are.<br>
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/no_rows_updated_tb1.PNG" height="40" width="400" hspace="5" vspace="5"/><br>
Displaying table tvshow_channel_updated(channel,array(tvshows)).
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/display_updated_table1.PNG" height="400" width="920" hspace="5" vspace="5"/><br>

Table tvshow_views is updated from multiple entries for a purticular tvshow to single entry with aggregation of viewers for respective tvshow into new table tvshow_views_updated.
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/table2_updated.PNG" height="270" width="720" hspace="5" vspace="5"/><br>
Following is number of rows of Tvshow_views_updated.<br>
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/table2_updated_rows.PNG" height="50" width="400" hspace="5" vspace="5"/><br>
Tvshow_views_updated is displayed.<br>
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/table2_updated_display.PNG" height="400" width="500" hspace="5" vspace="5"/><br>



**Problems**

**1.What is the total number of viewers for shows on ABC?**<br>
we can get the total number of viewers for all shows on ABC channel by adding all the number of viewers of all tvshows being played on ABC channel.<br>
I used following Query to achive this using two tables.
```
> select 'ABC',sum(views) from tvshow_views_updated as x
> join (select allshows from tvshow_channel_updated LATERAL VIEW explode(tvshow) visit as allshows where channel='ABC') as y
> on (y.allshows=x.tvshows);
```
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/ans1.PNG" height="385" width="870" hspace="5" vspace="5"/>And the answer is
```
ABC 1115974
```

**2.What is the number of viewers for the BAT channel?**<br>
This is quite similar to problem 1.<br>we can get the number of viewers for all shows on BAT channel by adding all the number of viewers of all tvshows being played on BAT channel.<br>
I used following Query to achive this using two tables.
```
> select 'BAT',sum(views) from tvshow_views_updated as x
> join (select allshows from tvshow_channel_updated LATERAL VIEW explode(tvshow) visit as allshows where channel='BAT') as y
> on (y.allshows=x.tvshows);
```
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/ans2.PNG" height="385" width="870" hspace="5" vspace="5"/>And the answer is
```
BAT 3031762
```

**3.What is the most viewed show on ABC channel?**<br>
we can get the most viewd show on ABC channel by sorting(descending) the number viewers of all tvshows that are being played in ABC channel and output the first channel and viewers.<br>
I used following Query to achive this using two tables.
```
> select tvshows,views from tvshow_views_updated as x
> join (select allshows from tvshow_channel_updated LATERAL VIEW explode(tvshow) visit as allshows where channel='ABC') as y
> on (y.allshows=x.tvshows) order by views desc limit 1;
```
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/ans3.PNG" height="385" width="870" hspace="5" vspace="5"/>And the answer is
```
Hourly_Talking 108163
```

**4.What are the aired shows on ZOO,NOX, ABC channels?**<br>
Here we will display the tvshows array from tvshow_channel_updated table.<br>
I used following Query to achive this using two tables.<br><br>**But here interesting part in result is ZOO channel is not available in dataset. So we get only 2 records as output with the following query**.
```
select channel,tvshow from tvshow_channel_updated where channel in('ZOO','NOX','ABC');
```
<img src="https://github.com/ManeeshDanthala/bigdata_task/blob/master/images/ans4.PNG" height="135" width="960" hspace="5" vspace="5"/>And the answer is
```
ABC ["Almost_Show","Hourly_Cooking","Hot_Show","Baked_Games","Dumb_Talking","PostModern_Games","Surreal_News","Loud_Show","Cold_News","Almost_Games","Hourly_Talking","Hot_Games","Baked_News","Dumb_Show","PostModern_News","Surreal_Sports","Loud_Games","Cold_Sports","Almost_News","Hourly_Show"]
NOX ["PostModern_Sports","Hourly_Talking","Baked_Cooking","Hot_Cooking","Loud_Sports","Almost_Show","Cold_News","Dumb_Cooking","Surreal_Talking","PostModern_Talking","Hourly_Show","Baked_Talking","Hot_Talking","Loud_Talking","Almost_Games","Cold_Sports","Dumb_Talking","Surreal_Cooking","PostModern_Cooking","Hourly_Games","Baked_Show","Hot_Show","Loud_Cooking","Almost_News","Cold_Talking","Dumb_Show","Hourly_News","Baked_Games","Hot_Games","Almost_Sports","Cold_Cooking","Dumb_Games","Surreal_Show","PostModern_Show","Hourly_Sports","Baked_News","Hot_News"]

```
