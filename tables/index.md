---
title: Tabular Election Data
---

# Tabular Election Data

This specification defines a number of simple tabular (CSV) data formats intended to support the publication of open data about elections, including:

* the choices available to voters, e.g. candidates, parties, or referendum choices
* the results of elections, e.g. the number of votes cast for individual choices
* high-level statistics about the voting process, e.g. number of valid and invalid votes cast

The intention is to define some simple formats that can be used to publish and aggregate data relating to a wide variety of different election types from a range of jurisdictions. The formats are based on a simple conceptual model of elections which provides a framework for further extension. This model focuses on describing the aspects common to all elections rather than the details of individual electoral processes.

Simple declarative schemas have been defined to support validation of data published in these formats. It is expected that these schemas will be customised and enriched for use in individual regions or types of election in order to validate additional elements, e.g. validating a list of voting regions against an official adminstrative geography.

## Requirements

There are several general requirements which have informed the design of these formats:

* Easy for election officials to generate both manually, e.g. using simple spreadsheet tools, and automatically, e.g. through database exports or APIs
* Easy for both citizens and developers to consume using a range of tools, including simple spreadsheet applications
* Customizable and/or extensible to allow for international and regional differences in types of election data
* Flexible enough to support reporting of data at different levels of granularity, e.g. at country, region and administrative district levels
* Make reference to standard terms, code lists and reference data to ensure that data is clearly documented
* Focused on supporting transparency of electoral processes, rather than driving process automation.

The formats are based on a simple conceptual model that describes elections as a "contest" between a range of choices. This is outlined in the following section.

## Conceptual Model

An _Election_ takes place on a specific date, or over a period of days.

A single ballot paper may ask voters to make choices for several different elections. This model treats the Election as the period of voting, which is then divided up into one or more _Contests_. 

A Contest asks eligible voters to express a preference between several distinct options. The rules under which a Contest takes place, known as an _electoral system_, define details such as how many times a voter may vote, how votes are counted, etc. Contests are of a particular 'electoral type', e.g. a local government election, parliamentary election, etc. 

However in every Contest voters are always making selections between several distinct options. These selections may represent a _Party_, individual _Candidates_ or, in the case of a referendum, between different political outcomes. Reporting of election results commonly involves identifying which of these options were selected according to the rules of the electoral system, usually with a break down of the number of votes.

Contests are conducted in specific 'Regions', e.g. an electoral district. Regions may be organised into a hierarchical administrative geography, allowing the reporting and aggregation of votes at multiple levels.

During a contest a voter may express their voting rights using a specific 'Voting Method', e.g. using a paper based ballot or electronic voting system. During the counting of votes, returning officers divide the votes cast into several different categories. The most two most common examples of a _Voting Category_ are valid and invalid votes. Invalid votes may be further divided into categories such as "spoilt" or "rejected".

Additional statistics may be derived from these vote counts, e.g. percentage of electorate choosing a candidate, or turnout for an election. As in some cases, particularly for turnout, there are various ways in which these statistics are calculated these are not covered by this model. The focus is instead on providing the raw data which can be the focus of additional analysis.

## Data Formats


### Contests

### Choices

### Candidates

### Voting Data

### Polling Stations

### Parties


* Extensions?
* 
