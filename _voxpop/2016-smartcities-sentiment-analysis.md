---
title: "Smart Cities"
excerpt: "Culture and Machine Learning"
author: "Adam Heinz"
date: 2026-08-31T20:16:03-00:00
ordinal: 362
---
*Citizen Sentiment Analysis*  

Can technology help ensure that all citizens’ opinions are considered, when the majority of citizens never participate in formal consultations? That question was explored by Smart Cities Christchurch in early 2017 using sentiment analysis.  

Genuine consultation is concerned with hearing the voices of all concerned citizens. But formal consultation processes are dull, the press releases are vapid, and it is very difficult to write a convincing submission without possessing a university education.  

As part of the consultation process for the Christchurch City Council 2017-2018 Annual Plan, some topical questions were posted on the council’s Facebook page to see if this could increase the range of citizen's voices that are heard.  

Over 600 responses were extracted from the Facebook graph and run through a sentiment analysis with a pretrained machine learning model on SQL Server with Python. The pretrained model, however, presented significant challenges.  

On the face of it, sentiment analysis seems simple. Text is given to the model, which indicates the sentiment as being one of “positive”, “negative”, “neutral” or “mixed”. The complexity, however, lies in what the model was trained on in the first place.  

The key challenge may best be understood with a different but indicative example.  

Many New Zealanders have been subjected to “engagement” surveys at work, only to find that the most passionate and committed teams get scored as “disengaged”. One reason is that it is quite common for New Zealanders to give a top grade of 4/5 on the grounds that “nobody is perfect”. Yet the engagement survey is purchased from American software companies, where giving anything less than 5/5 is considered disengaged. The bias is simply cultural.  

Cultural bias quickly became apparent in applying sentiment analysis to the Facebook responses. 

One aspect was purely linguistic. The British may be famous for using the “double negative” to express a positive sentiment, but New Zealanders may use a “triple negative” to express dissatisfaction with council in an online forum.  

Another aspect was that every single online response was immediately rated as having a “negative” sentiment by the pretrained model, even though the general discussion seemed positive overall to this New Zealander. Seeing the outcome of the analysis, it was easy to imagine that the model had been trained on blond, blue-eyed Californians with a naturally sunny disposition, who had never played rugby in their life. The machine learning model had to be retrained with manual scoring over a period of two weeks before the sentiment analysis was commensurate with New Zealand ways of speaking.  

The results were displayed publicly at the second Christchurch Smart Cities Expo in March 2017.  


Further Reading 
--------------- 

Microsoft. Learn SQL Server. [Install pretrained machine learning models on SQL Server]( https://learn.microsoft.com/en-us/sql/machine-learning/install/sql-pretrained-models-install?view=sql-server-ver15 ), accessed August 2026.


© Adam Heinz 
2016
