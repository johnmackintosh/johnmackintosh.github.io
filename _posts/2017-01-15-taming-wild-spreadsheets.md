---
layout: post
published: true
title: R to the Rescue
subtitle: A strategy for gathering scattered spreadsheet data with R
tags:
  - rstats
  - Excel
date: '2017-01-15'
---
##  A simple request

Recently I was tasked with extracting data from multiple spreadsheets so that it could be included in a report. 
Seemed like a fairly innocuous request, but it turned out to be a bit more challenging.

I had over 140 files to process.  That's not usually a big deal  - I normally use SQL Server Integration Services to loop through network folders, connect to hundreds of spreadsheets and extract the source data. 

**But** this relies on the data being in a tabular format (like a dataframe or database table).  

A quick glance at the first few sheets confirmed I could not use this approach - the data was not in tabular format. Instead it was laid out in a format suited to viewing the data on screen - with the required data scattered in different ranges throughout each sheet ( over 100 rows and many columns). It wasn't going to be feasible to point SSIS at different locations within each sheet. (It can be done, but it's pretty complex and I didn't have time to experiment).


The other challenge was that over time, changes to design meant that data moved location e.g. dates that were originally in cell C2 moved to D7, then moved again as requirements evolved. There were 14 different templates in all, each with subtle changes. Each template was going to need a custom solution to extract the data.



![](http://i.giphy.com/SEp6Zq6ZkzUNW.gif)  

_the most perfect gif on the planet_ 

##  What else could I try?

I need to create a  consistent, tabular summary in each workbook - referencing the relevant cells in each sheet.  This would give me 8 rows ( 1 header row,  plus 1 each day for Mon-Sun) with as many columns as required (1 for each metric), that I could then import via SSIS. 

But - how to write the formulae in this summary sheet? 
I could easily do this manually, but then was I going to copy and paste the formulae to the remaining workbooks? The formulae would need amending each time to remove the external references. One false move and there'd be an infernal loop of broken references that Excel couldn't navigate. No - this was not going to be the answer.  

How about VBA? I've used that in the past (before SSIS) to loop through hundreds of XLS files and collating to a master workbook - but the looping could be quite slow and could be difficult to trouble shoot. It had also been a while since I wrote any VBA and I didn't have much time to get myself back into a VBA mindset.

Thinking ahead, once I'd created these summary sheets, and collated them, I was going to need to analyse and visualise the data. I needed a tool that could write to Excel, collate the data, perform analysis and visualise many metrics at once. I also needed the solution to be reproducible, in case of errors. And I needed to produce multiple visualisations without having to manually create each plot. 


## Excel SOS %>% R

I already had R in mind as a potential solution, so had spent some time  earlier looking into packages that could  both read and write to Excel. I chose [XLConnect](https://cran.r-project.org/web/packages/XLConnect/index.html). It has a simple syntax, which, given the time pressure I was under, was very welcome.  

I won't show the exact code I used, but can give a general idea of my approach:  


-	Set up a loop through the current directory (I won't show that here)  

-	Create the "SUMMARY" sheet:   	

```
removeSheet(wb,"SUMMARY") 
createSheet(wb, name = "SUMMARY") 
# delete the summary sheet (in case of having to re-run the code due to errors) and recreate
```	  			

-	Create a named range of appropriate size:   

```
createName(wb, name = "Summary", formula = "SUMMARY!$A$1:$M$8", overwrite = TRUE) 	
```

-	Define friendly column names (e.g. no spaces)  

```
Headers <- data.frame("Header1","Header2")# for as many cols as you need
writeWorksheet(wb, Headers, sheet = "SUMMARY", startRow = 1, startCol = 1, header = FALSE)
# Write the headers to the range beginning A1. 
# Use header =  FALSE to stop default column headers being written, we want OUR newly defined column headers
```  

-   Define a consistent date format:  


```
cs <- createCellStyle(wb)
setDataFormat(cs, format = "yyyy-mm-dd")
```

-   Now populate the date column, starting in cell A2 below the "Date" header:  

```
# insert dates into A2 on SUMMARY sheet
setCellFormula(wb,"SUMMARY",2,1,formula = "Sheet1!$C$5") # reference the date from Sheet1, cell C5 
setCellStyle(wb, sheet = "SUMMARY", row = 2, col = 1, cellstyle = cs)# now format as date on Summary sheet
# repeat for the remaining days
```		

-   Add other columns as required:  		

```
setCellFormula(wb,S,2,4,formula = "Sheet1!$J$20")
setCellFormula(wb,S,3,4,formula = "Sheet2!$J$20")
setCellFormula(wb,S,4,4,formula = "Sheet3!$J$20")
setCellFormula(wb,S,5,4,formula = "Sheet4!$J$20")
setCellFormula(wb,S,6,4,formula = "Sheet5!$J$20")
setCellFormula(wb,S,7,4,formula = "Sheet6!$J$20")
setCellFormula(wb,S,8,4,formula = "Sheet7!$J$20")

# In real life the sheets had different names 
# e.g. if your was called "Expenses" then the reference might be :
setCellFormula(wb,S,8,4,formula = "Expenses!$B$10")
```

-   Add the workbook name and path for troubleshooting:  	

```
writeWorksheet(wb, n, sheet = "SUMMARY", startRow = 2, startCol = 20, header = FALSE)

saveWorkbook(wb)
```		

###		Problems I encountered   



-  The Summary sheet not showing properly in the file browser preview window. Not sure if this was a Windows or Excel (2010) issue, but browsing a few of the spreadsheets in preview mode to test the code showed the "SUMMARY" sheet had been created, but I couldn't see anything in the sheet itself - it looked completely blank. 

It wasn't until I eventually opened the workbook and saw the summary sheet with the populated references that I knew my approach had worked. The rest of the workbook was completely visible via the preview window, so not sure why the contents of this new sheet weren't?  This certainly threw me for a bit - I was double checking my code and couldn't see why it wasn't working.    

-  Out of memory errors. I couldn't process more than few files at a time before my laptop began to hang - and then cryptic warnings appeared about heap space errors.    

Here are the links you'll need if it happens to you:  

[out-of-memory-error-java-when-using-r-and-xlconnect-package](http://stackoverflow.com/questions/7963393/out-of-memory-error-java-when-using-r-and-xlconnect-package)  
The second answer helped me.

[Increase heap size for rJava](http://www.bramschoenmakers.nl/en/node/726)
This is referenced from the SO link and gives more info on what to do. 


Ultimately I got round this by creating several more sub-directories, and ensuring I never had more than 4 or 5 files in each. This may or may not work for you, depending on the files you are manipulating, and computer specs. (My laptop has 8MB RAM and handles virtually everything else I throw at it). 

###  Pulling it all together

Once the code worked for the first template, it was a matter of working through the other dozen or so and amending the code to reference the correct cells. Lots of Find/Replace ( a lot easier in RStudio, thankfully).

I was wondering afterwards if I should have attempted to create a set of functions, but I'd have had to amend the cell references anyway each time anyway, so I'm not sure it would have benefitted.

The final challenge was collating all the summary sheets, from all the files, into 1 main file. 


This is not a task I've had to do before with R so at this point, I deferred to the geniuses at Stack Overflow:
[loop-for-read-and-merge-multiple-excel-sheets-in-r](http://stackoverflow.com/questions/22394234/loop-for-read-and-merge-multiple-excel-sheets-in-r)

This helped, but turned out to be quite cumbersome as the number of files racked up. 
I also began getting out of memory errors again, so I decided on a different approach, abandoning XLConnect in favour of [readXL](https://cran.r-project.org/web/packages/readxl/index.html) (for reading the files) and [data.table](https://cran.r-project.org/web/packages/data.table/index.html) ( for collating everything together).

[This post](https://itsalocke.com/r-quick-tip-collapse-a-lists-of-data-frames-with-data-table/) by Steff Locke gave me the push towards data table, the first example worked fine for me as thankfully all the sheets were now consistent. 

Basically, all the individual summary sheets got read in as a dataframe, and each  was added to a list in turn:		

```
pathname <- getwd()
files<- list.files(pattern = "*.xlsx") #or use dir()

mylist <- list()

for (i in seq_along(files)){
filename<-files[i] 
df<-read_excel(path=filename, sheet = "SUMMARY", 
               col_names = TRUE, col_types = NULL, 
               na = "", skip = 0)
mylist[[i]]<-df
rm(df)
}  
```		

This was much faster and there were no memory  issues.  The list was then output to a dataframe per template:  

```
mydf1 <-data.table::rbindlist(mylist)

```  

Finally all these  intermediate dataframes got merged into a new list, then combined to one master dataframe/ data.table:   

```
## merge all the dfs
l.df <- lapply(ls(), function(x) if (class(get(x)) == "data.table") get(x))
#http://stackoverflow.com/questions/14954399/put-multiple-data-frames-into-list-smart-way
# I set class to data.table as that is first in list and data.frame was being ignored
maindf<-data.table::rbindlist(l.df)  

```  

This probably still looks quite clunky but it worked fine.
I had one master dataframe which I then set to work on with dplyr. 

I could have moved all the files to one directory, and looped through them all at once. But I left them in case I had to repeat the process - I did not want the out of memory errors to reoccur. It was straighforward to import the data via SSIS at this stage, but the advantage of doing so in R meant I could move straight on to exploring the data. 


Is this the best approach? Well it worked... and I was not aware of anything else that would do the job at the time.

A couple of months later I saw this presentation by Jenny Bryan : 
[Jailbreakr-Get-out-of-Excel-free](https://channel9.msdn.com/Events/useR-international-R-User-conference/useR2016/jailbreakr-Get-out-of-Excel-free).  

This refers to jailbreakr - the github repo is here :  [https://github.com/rsheets/jailbreakr](https://github.com/rsheets/jailbreakr) and it seeks to address the same problem. 

The purpose is :

**Data Liberator. To extract tabular data people put in nontabular structures in a program designed to hold tables.**

It's still in the early stages though so I couldn't have used it, even if i'd been aware of it - but it's certainly one to keep an eye on.  Especially because of the awesome gifs (including the brilliant one above).

At least until such time as jailbreakr is available I have a proven strategy for dealing with spreadsheet chaos. So next time this situation occurs I can be more serene :

![](http://i.giphy.com/99e1L4KIXQh6U.gif)

Thank you to all the brilliant R developers out there who make life easier for the rest of us.
