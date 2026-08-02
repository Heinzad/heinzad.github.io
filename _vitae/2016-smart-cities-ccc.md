---
title: "Data Federation"
excerpt: "City data hub and dashboard for council and citizens — Data Engineer, Smart Cities, CCC, 2016-2017."
author: "Adam Heinz"
date: 2026-03-30T20:16:00-00:00
ordinal: 360
---
*Engineering a data hub and live dashboard for an entire city.*  
  
Title: Data Engineer  
Department: Smart Cities  
Organisation: Christchurch City Council  
   
Technology:  

- Leaflet, MapBox, Power BI Service. 
- SQL Server Machine Learning Services (Python). 
- MS SQL Server, SSIS Integration Services, SSAS Analysis Services, PBIRS Report Server.  

Achievements:  

- City Data Hub & Dashboard: realtime information across the whole of council. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/ChChSmartCities_AdamHeinz.jpg" alt="Data Engineering" class="full">

---

**Can data empower citizen decision-making?**  
Take air quality for example:  
Existing air quality monitoring stations achieved high accuracy at high cost, to alert authorities of breaches of air quality regulations.  
If citizens could view air pollution in the central city, live, might they choose to come into the city during times of low air pollution?  
Might they change their transport choices from cars to bikes if they understood the impact of their actions?  
CCC Smart Cities funded low-cost sensors placed in the Re:Start mall that transmitted realtime information.  
If the sensor data could be callibrated to the monitoring station data, then citizens could make their own judgements from live data that was accurate enough for the purpose.  
I joined the Smart Cities team to help unlock data. 


The situation was that the Mayor of Christchurch wanted a way for City Councilors and the executive to understand everything that was happening in the city.  
My task was to deliver a proof-of-concept for an internal operations view of the city to complement the public view [SmartView](https://www.smartchristchurch.ccc.govt.nz/projects/smartview/).  
My actions were to research and develop a city data hub and dashboard based on the [Chicago OpenGrid](https://opengrid.chicago.gov/).  
The result was a federated data hub and dashboard (mobile & desktop) demonstrated live on national news at the second Smart Cities expo in 2017.  
Features included: —      

*Data Aggregation:*  

- **Big data:** Loading 50 million cycling records from a quarterly data extract from Strava.  
- **Sensor data:** Cycle Counters funded by CCC Smart Cities (in Hagley Park and at the Antigua Street bridge). 
- **Citizen data:** Loading cycleways data from OpenStreetMap curated by citizen data volunteers (*official data was only supplied to CCC at the end of a roading contract, which could happen 5 years after a cycleway was constructed*).

*Data Virtualisation:*  

- Live [NZTA traffic cameras](https://livetraffic.nz/south-island/christchurch/), with 
- Current roadworks from the [SCIRT Forward Works Planner](https://scirtlearninglegacy.org.nz/programme-management/forward-works-viewer/), and 
- Realtime [Traffic flows from Google Maps](https://developers.google.com/maps/documentation/javascript/trafficlayer). 

*Data Federation:*  

- Internal datasets from Christchurch City Council.  
- Public datasets from StatsNZ, LINZ.  

*Machine Learning:* 

- **Sentiment analysis:** Utilising social media (Facebook Graph API) to discern citizen sentiment outside of formal consultation processes.  

CCC Smart Cities was part of a consortium with the Wellington City Council. This followed an earlier effort by LINZ.  
Many of the lessons learned were operationalised as open datasets now made publicly available by CCC.  

---

Responsibilities:  

- Database design and development (tables, views, functions, and stored procedures)
- Data ingestion design and development
- Federated Data modelling
- SSIS design and development
- SSAS tabular model design and development for near realtime analytics
- SSRS reports development and design
- 3D spatial modelling and display
- Dashboard design and development for near real-time analytics
  

Outcomes:  

- Data hub & live dashboard presented at the first and second Smart Cities expos.  
- Achieved near-realtime performance by leveraging metadata-driven development, in-memory databases and parallel processing.
- Versions 1 and 2 presented in public at the first and second Smart Cities expos.  

--- 

Photo Credit: EkantTakePhotos on [Reddit](https://www.reddit.com/r/EarthPorn/comments/4lubly/sunrise_over_shag_rock_christchurch_nz_the_rock/), accessed from [Pinterest]( https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.pinterest.com/pin/crystal-clear-night-sky-stars-reflection-of-the-water-2600-1522--288230444883546210/).

© Adam Heinz 
2017
