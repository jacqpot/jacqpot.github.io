---
layout: post
title:      "understanding Dates"
date:       2020-10-20 01:42:08 +0000
permalink:  understanding_dates
---


while it doesnt sound like rocket science, manipulating a date or time can be quite challenging. Not unlike arriving on time after your first date in high school, Getting date and time to display just how you want it can be hard! lets take a look at the way our computers store view and store dates.

It turns out that our computers store the time  elapsed from January 1, 1970 in milliseconds and extrapolate that into our time zone and in a human readable version of that excedingly long number. 

```
var d = new Date();
console.log(d.getTime())
```
returns (1603156537711)
at the time of this writing.

That number will never be returned with those two lines of code again. which is kinda cool. 
now the tricky part is getting that number into our ideal format.

a dateTime data type in a rails api is stored somthing like this:
# 2020-10-20T19:30:00.000Z

so its our job to convert this into an easy read data type.

there are dozens of formats avalible, so lets try formating that one into somthing nice.

to make it easy ive done a console.log of the following code: 
```
let date = new Date(section.startTime);
  let formatedDate = new Intl.DateTimeFormat("en-US", options).format(date);
  console.log(section.startTime, date, formatedDate);
```
so lets take a look at the result of our console.log:

*section.startTime = 2020-10-20T19:30:00.000Z date = Tue Oct 20 2020 13:30:00 GMT-0600 (Mountain Daylight Time) formatedDate = 7:30:00 PM*

****for our reading pleasure I added in a lable to distinguish each of the items logged.

as we can see, just turning the stored datetime value into a new date turned the originial "19:30" into "13:30" what the heck is that all about? That is UTC time. I couldnt figure out for the life of me what on earth to do! my computer couldnt make my stored variable into a more readable format without changing time zones! oh boy. 

Enter into the scene *Intl.DateTimeFormat* this magic takes in our language and a few perameters entered into an options object to format our new date. for this specific example I just wanted the time, so in my options looked like this:

```
let options = {
    timeZone: "GMT",
    hour: "numeric",
    minute: "numeric",
    second: "numeric",
  };
	```
	
	Here I set the time zone and what data I wanted to display.
	
	Gosh thats alot of work! That was alot of work, but 7:30 pm is alot easier to read than 2020-10-20T19:30:00.000Z. 
	
Sometimes it takes one brave soul wrestling with simple logic and writing a blog about it to make it more accessable for the next yokle. so here I am, a brave and very tierd soul converting dates for your viewing pleasure! until next time. 
# keep calm and code on!
