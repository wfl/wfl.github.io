---
Project Benson Reflection
---

The goal of the project is to help the WomenTechWomenYes (WYWT) International to identify strategic subway stations where it can deploy their street teams to gather email signature from people who have strong interest in attending the annual gala in early summer and increasing participation of women in technology. 

The **methodology** to solve this problem includes the following steps:

1.	Ask the questions: 

    •	How do identify the strategic subway stations? Select those with high volume of traffic. 
    
    •	Who are the target audiences? We assume they are people who are working in the technology field, women who are employed in other fields, and students.
    
    •	When is the gala? We assume a specific time frame before the event would be the best time to approach the target audiences.

2. Find the data sets:
    •	NTC’s MTA turnstile data from the [MTA website]( http://web.mta.info/developers/turnstile.html)
    
    •	Demographic data from the [NYC Open website]( https://opendata.cityofnewyork.us/)
    
    •	Relevant maps to locate the stations and districts

3. Exploratory Data Analysis:
    •	Learn the raw data – What do the column labels represents? Is there any pattern or format? How many subway stations and their lines?
    
    •	Data Cleaning – Clear whitespace, reformat the data, drop unimportant information, remove outliers
    
    •	First analysis – Develop algorithm to extract relevant information from the cleaned MTA data and test different 
   
   ![The bar graph is an example list of stations after we set the threshold to more than 3 million of total volume.](https://github.com/wfl/wfl.github.io/tree/master/images/Example_List_of_stations_3milthreshold.png)
   
    •	Second analysis – Merge the cleaned demographic data and analyzed MTA data for second analysis to identify the final list of subway stations

4. Recommendation: 

   ![From our initial analysis, the subway stations where the street teams would gather the email signatures from most of the target audiences are GRAND CENTRAL – 42nd ST, 34th St. – Herald Square, 42nd St. – Port Authority, Times Square – 42nd St., Fulton St., 59th St. Columbus, and 47-50 Streets - Rockefeller Center Subway Station](https://github.com/wfl/wfl.github.io/tree/master/images/Final_List_of_Stations.png). These stations are located in one of these areas: Midtown Manhattan, Battery Park, and Lincoln Square. 


**What I have learned from this project:**

   •	Data: It takes a lot of time to find data sets that are useful for the analysis. Sometimes, the documentation is incomplete. So, it is best to allocate more time to search and study the data.

   •	Tools: If we have a collection of APIs, our analysis process could be more efficient. 

   •	Time: We must allocate a time frame for each step in the exploratory data analysis, so that an initial analysis could be completed with sufficient insights to inform us of where and how to improve our next iteration of analysis workflow.  



**Acknowledgement:**  

This a collaborative project. I thank my teammates are [Kristin](https://github.com/kmussar/metis_project_benson) and [Torin](https://github.com/Starplatinum87) for their contributions. They make it possible for me to complete my first exploratory data analysis at Metis. 




 




