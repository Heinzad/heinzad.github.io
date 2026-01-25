---
layout: post
tags: Agile
title: "User Stories and Acceptance Criteria"
author: "Adam Heinz"
date: 2022-12-02 01:00:00
---
AIDE MEMOIRE 

How to Write User Stories and Acceptance Criteria for Agile 
=========================================================== 

Test-driven development is critical to knowing whether or not your progam actually works in the way it was expected to. 

Well-written user stories and acceptance criteria can help bridge the gap between business analysts and developers. 

Love it or hate it, SAFe - Scaled Agile Framework for enterprises - provides explanatory material that is quite useful for building a shared reference point for a team. 


# User Stories 

SAFe gives a useful overview of a [story](https://www.scaledagileframework.com/story/), with an emphasis on behaviour. 

This approach is described in further detail by Dean Leffingwell and Pete Behrens in [A User Story Primer](https://www.scaledagileframework.com/wp-content/uploads/2021/07/User-Story-Primer.pdf). 

They emphasise that a story should be broken down until you reach a point where it is testable and achievable. 

A user story follows the format: 
* As a ... 
* I want to ... 
* So that ... 

## card: 
A user story card format  

``` 

    As a {user role}, I want {to do}, so that {I can achieve} 

```

## conversation: 

A card should read as if a customer had walked up to your desk and said in their own words what it is that they need to be able to do that they cannot do with existing functionality. 

The card is not the description of the requirements, but the beginning of a conversation. 

## confirmation: 

Acceptance criteria follow the format: 
* Given 
* When 
* Then 

Although acceptance criteria are critical to test-driven development, they are not elaborated in the same way by SAFe. Here is a handy layout for business analysts and developers:  

``` 

    Given {parameter}, When {function}, Then {return} 

``` 

This behaviour-driven approach helps user stories and acceptance criteria contribute to test-driven development from the earliest possible stage. 


QED 

© Adam Heinz 

2 December 2022 
