---
layout: post
title:  "Project: Ruby CLI Gem Overview"
date:   2016-06-08 04:21:09 +0000
---

*This post was transferred from an older blog dated 3/16/2016*

**Background:**

I created the Currency Xchange Cli Gem as one of the final projects for the Learn-Verified Curriculum for Flatiron School. I must say that working on the project was quite interesting as I've learned a bunch of stuff about Ruby Gems and how they are created and published. 

The requirement for the project was to create a gem with CLI that a user can interact with. There's literally thousands of great ideas for the topic, and I did had a tough time in deciding which one to go with.

**Idea:**

In the end, I decided to work on something that might be useful for some people. There's probably a lot like this gem out there, but I figured that it's still a way for me to demonstrate what I've learned so far in the Learn-Verified Curriculum. 

I chose to go with a money converter, hence the "Currency Xchange CLI Gem". I looked up at the first page of Google to see which sites are prominent when it comes to Currency conversion. One of the sites on top of the page was [x-rates.com](http://www.x-rates.com). I looked at the site and I saw that their structure was quite straightforward. The information that I needed was easy to scrape.  


![](http://1.bp.blogspot.com/-DzECht5TxHU/Vtvg0EID-pI/AAAAAAAAAbQ/utf0K6MA0kk/s1600/xrateshome.png)


When you scroll at the bottom, you'll find the section URLs for each type of currency:


![](http://4.bp.blogspot.com/-5qTQ1Tf_Tm4/VtvhPDaek7I/AAAAAAAAAbY/VAn2sYH_OJ8/s1600/xratesindex.png)


Bingo. That's my entry point for my Nokogiri gem to get in. When you click on the individual links, you'll see a lot of information regarding their conversion rates when compared to other currencies. Basically, this is the first step in designing my scraper program or class. 

When you click on one of the currencies, you'll see that the URL on top will default to:


![](http://2.bp.blogspot.com/-g5FsyiXSFOM/VtviCNYg0mI/AAAAAAAAAbo/18UR2ChtJT8/s1600/xratesurl.png)


As you can see, the amount is set to 1, which means that all the information on the page uses $1 as the basis for conversion. Below the page, you'll find an alphabetical directory on how much $1 is in other currencies:


![](http://4.bp.blogspot.com/-wS3QSI-3ELo/VtviafnBZYI/AAAAAAAAAbw/Njmh_iHJiP4/s1600/xratesdir.png)


This made my job a lot easier, as I already have a directory of all the conversion rates for all countries, for a specified amount. Now, it's all about changing the URL so that the amount will affect and change the whole page. The scraper process is now complete. 

**The CLI:**

When running the CLI, you'll be presented with options on what to do next:


![](http://3.bp.blogspot.com/-G5jK1Aa7nNc/VtvjU3bmLVI/AAAAAAAAAcA/bpieLWb2L2k/s1600/Cli1.png)

From this page, you have two options:

1. Enter search if you want to browse through the directory of currencies, by using the first letter of the country or currency.

2. Enter the name of the currency you want to convert. Take note that you have to enter the exact currency as in "US Dollar" as it appears in the search results or the sample above. 


![](http://2.bp.blogspot.com/-3qBIxoRT0V0/VtvkCEGsVgI/AAAAAAAAAcM/IEGjf3Mq_1I/s1600/Cli2.png)


After entering the correct currency that you want to convert from, you must enter the amount you want to convert, and then to which currency do you want to convert to:


![](http://2.bp.blogspot.com/-hcTjXyFfBu8/Vtvl1RhfS5I/AAAAAAAAAck/FOOqq5LC8LY/s1600/CLi3.png)


As you can see from the screenshot, conversion can be done instantly on the amount that you entered. After displaying the results, you'll get to see another menu for options to change the local currency, the amount, or the foreign currency that you want to convert to. I think this CLI does a good job in making sure that the program won't break and there's flexibility on the side of the user. 

The rates that will be used for conversion are quite accurate as we only scrape the information from the site when the user successfully "enters" all the inputs required. 



