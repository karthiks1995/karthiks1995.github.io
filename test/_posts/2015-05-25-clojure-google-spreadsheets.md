---
layout: post
title:  "Starting with Google Spreadsheet API and OAuth2"
date:   2015-05-25 23:25:05
author: Karthik
categories: clojure google-api google-spreadsheet oauth2 leiningen maven
permalink: "/getting-started-with-google-spreadheet-api-and-oauth2"
image: true
---

You might be working with huge databases on which you want to perform some heavy weighted queries. Repeating these queries will take a heavy toll  on the CPU. You would want to store the queried database so as to avoid repetation of these queries. Using spreadsheets for storing the data would be one nice choice as it is easier for people not familiar with db to do a visual comparison.

In this process of storing the data into a spreadsheet, adding a row to a spreadsheet is the basic step one has to do and this is not a cake walk for someone who's not an expert because of the following reasons.


* Basically you are supposed to use the Authentication API and Spreadsheet API of Google. Here the Authentication API is mavenised whereas the Spreadsheet API is not. 
* Once the authorization is done through one account, it wont ask you to authorise again. So it is not possible for you to edit a spreadsheet from another account.

This simple example of adding a row demonstrates all the process involved and can be extended to perform the required task.To familiarise yourself with Clojure or Leiningen, you can use the links for the 
<a href="http://www.moxleystratton.com/blog/2008/05/01/clojure-tutorial-for-the-non-lisp-programmer/">clojure</a> and <a href="https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md">leiningen</a> tutorials.

## Dependencies

In order to include the mavenised version of Spreadsheet API, you can include the following snippet in your project.clj if you are using clojure or pom.xml for java.
{% gist karthiks1995/f9ab9dc4099df62691ac %}
{% gist karthiks1995/d884c60ad9b2d611099c %}

## Authentication
Below is the code snippet for authorization. Execution of this code requests authorisation in the default browser. Successful authorisation creates .store directory in /home/user/ . Successful authorisation grants access to all the files accessible by the authorised account.


{% gist karthiks1995/3e33bfc568c139d3823e %}

{% gist karthiks1995/b77374c8e6f1fd850754 %}

The basic structure of a spreadsheet is like this:

* A user has access to many `Spreadsheets`
* Each spreadsheet has many sheets under it which are called `Worksheets`
* Each Worksheet has many `Entries` which are nothin but rows

Our objective is to get access to the last entry so as to add contents to it




## Getting the Worksheet and adding rows


Now add the below provided snippet which contains the functions `get-ws` and `add-row` which can be invoked from any file.
{% gist karthiks1995/a35e47f4feed3e12f20e %}
{% gist karthiks1995/4cb923c3220c0defe29e %}





`get-ws` takes two arguments. One is the URL of the spreadsheet and the other is the class of the sheet you are accessing. In this case its WorksheetFeed.This gets the list of all the entries.

`add-row` also takes two arguments. One is the URL for the list of the spreadsheet and the other is the data you want to enter.


Note that the URL accepted by the the above mentioned functions is not the same as the URL of the spreadsheet in the browser. You have to modify it as shown below.

This is the sample browser URL for the spreadsheet.  
*https://docs.google.com/spreadsheets/d/1SpsUoJjXfrRlHkbuxY54Y6wAw-EBpQptF6gFAgTGFwU/edit#gid=0*

The get-ws URL should be modified as:
*https://spreadsheets.google.com/feeds/worksheets/1SpsUoJjXfrRlHkbuxY54Y6wAw-EBpQptF6gFAgTGFwU/private/full*

The add-row URL should be modified as:
*https://spreadsheets.google.com/feeds/list/1SpsUoJjXfrRlHkbuxY54Y6wAw-EBpQptF6gFAgTGFwU/od6/private/full*

## Implementation

Now let us use the above defined functions to add a row to the spreadsheet.
Assume you have a spreadsheet whose first row has "Name" "Roll" "Rank", which corresponds to names of first three columns and let the first row be filles as shown in the figure.
{% if page.image %}

<img class="img-responsive img-post" src=" {{site.baseurl}}/img/Selection_001.png "/>


{% endif %}

Now if you want to add another row to the spreadsheet use the above defined functions as shown in code snippet below.

{% gist karthiks1995/27507482af2c20acba0e %}
{% gist karthiks1995/2a14e7d15e8eb16a87bd %}


Spreadsheet after adding the row.

<img class="img-responsive img-post" src=" {{site.baseurl}}/img/Selection_002.png "/>
<figcaption>A screenshot from Google Sheets</figcaption>
Note that the second argument for add-row function should be of the form {"ColumnName1" "Data1","ColumnName2" "Data2",.....so on} where ColumnName1,ColumnName2.... are the elements of the first row in the spreadsheet.



## Changing the authorised account

You might have noticed that once you have authorised it wont ask for authentication again. Once you authorise, a file called analytics is created in /home/user/.store/ . As long as this file is there , it wont ask authorisation. Therefore if you want to change the auhorised account, you have to delete the file and run the auth code.

{% gist karthiks1995/ab5c84c3c917cb4db617 %}




## Why was this blog written?
* For the past 2-3 years, google had not released the mavenised version of Spreadsheet API, and the link for the one suggested here was not easily available.
* In order to edit spreadsheets authorised by different accounts, we have to make sure that the `home/user/.store/analytics` file is deleted or the authentication file for the new account shold be created in a separate folder. This is not explicitly stated anywhere in the OAuth2 documentation of google.
* All the tutorials or blogs about Spreadsheet modify it by iterating over the spreadsheets which increases the time taken to complete the query. In this approach, the URL of the required worksheet and the Entry is calculated and appended (setValueLocal in add-row) which reduces the complexity drastically.






