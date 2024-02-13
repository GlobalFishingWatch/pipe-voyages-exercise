# Voyages exercise

This repository contains an exercise for data engineering positions at GFW.
This exercise is based on an actual task we tackled a couple of months ago. The
goal of this exercise is for it take 1 hour. You are not expected to produce a
complete solution to this problem. You are expected to ask questions, and
optionally describe how you would implement a solution.

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

The goal of the exercise is to review and understand the query, and then
propose and design how you would change this process to be both faster and
cheaper.

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
