# Back-end Data Engineer Position - Interview Task

This repository contains an exercise for data engineering positions at GFW.
This exercise is based on an actual task we tackled a couple of months ago. The
goal of this exercise is for it take **1 hour**. 

This task is designed to (1) evaluate your analytical skills, (2) your communications skills 
and (3) your SQL understanding and technical knowledge applied to data problems. 

# Interview Task

Please answer both questions in a document, remove your name so it can be considered anonymously and email responses to HR by the deadline shared in the email.

This should take no longer than one hour to complete in total - we are not expecting full briefings, introductory paragraphs and bullets are very welcome. 


## Context

One of the branches of our data pipelines computes port visits. These are
events detected when a vessel get close to an anchorage and stops in its
vicinity. Once we've calculated port visits, a very important data product we
produce is to compute voyages. A voyage is defined as an entity that comprises
everything that happens between 2 consecutive port visits for a single vessel.
For more information on how anchorages and port visits are calculated visit [this page](https://globalfishingwatch.org/datasets-and-code-anchorages/).

In this repository you will find a query that computes these events in the form
of a [jinja](https://jinja.palletsprojects.com/en/3.1.x/) template. This query
was produced by the research team, and while it produces accurate results, it
does have some operational problems. The main issue is that it is a very
expensive and slow query, taking more than 6 hours to run every single day.



## The Excercise

The goal of the exercise is to review and understand the query, and then
propose and design how you would change this process to be both faster and
cheaper.

1. Send a list of questions you think need to be answered in order to design the best solution.
2. Even though you may not have the complete context, write a max 400 word description of what solution you would implement and why. 
3. (optional) Submit a high-level diagram of your solution or reference material pointing/supporting the solution you mention in the point above. 


## Constraints

* Both the source data (port visits) and the output (voyages) need to go to
  [BigQuery](https://cloud.google.com/bigquery?hl=en).

* You are not required to keep this as a query. You can propose alternative
  technologies and frameworks to produce these results.

## Schemas and additional documentation

The source table, `port_visits`, is a [monthly date partitioned table](https://cloud.google.com/bigquery/docs/partitioned-tables), with the following schema

visit_id STRING  REQUIRED  - Unique ID for this visit  
vessel_id STRING  REQUIRED  - `vessel_id` of the track this visit was found on  
ssvid STRING  REQUIRED  - `ssvid` of the vessel involved in the visit.N.B. Some `ssvid` may be associated with multiple tracks  
start_timestamp TIMESTAMP  REQUIRED  - timestamp at which vessel crossed into the anchorage  
start_lat FLOAT  REQUIRED  - latitude of vessel at `start_timestamp`  
start_lon FLOAT  REQUIRED  - longitude of vessel at `start_timestamp`  
start_anchorage_id STRING  REQUIRED  - `anchorage_id` of anchorage where vessel entered port  
end_timestamp TIMESTAMP  REQUIRED  - timestamp at which vessel crossed out the anchorage.  
end_lat FLOAT  REQUIRED  - latitude of vessel at `end_timestamp`  
end_lon FLOAT  REQUIRED  - longitude of vessel at `end_timestamp`  
duration_hrs FLOAT  REQUIRED  - duration of visit in hours  
end_anchorage_id STRING  REQUIRED  - longitude of vessel at `end_timestamp`  
events RECORD  REPEATED  - sequence of port events that occurred during visit 
