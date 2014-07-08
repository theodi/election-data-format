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

During a contest a voter may express their voting rights using a specific _Voting Method_, e.g. using a paper based ballot or electronic voting system. During the counting of votes, returning officers divide the votes cast into several different categories. The most two most common examples of a _Voting Category_ are valid and invalid votes. Invalid votes may be further divided into categories such as "spoilt" or "rejected".

Additional statistics may be derived from these vote counts, e.g. percentage of electorate choosing a candidate, or turnout for an election. As in some cases, particularly for turnout, there are various ways in which these statistics are calculated these are not covered by this model. The focus is instead on providing the raw data which can be the focus of additional analysis.

## Data Formats

The following sections define simple schemas for a number of tabular data formats. Each format is intended to support the publication of a specific type of election data.

The following general requirements apply to all formats:

* Files should be published as UTF-8 CSV files according to the [Model for Tabular Data and Metadata on the Web](http://w3c.github.io/csvw/syntax/)
* Identifiers for Regions, Parties, etc should all be URIs. Those URIs should draw from official sources. Ideally URIs should resolve into additional metadata about the resource
* References to URIs should be expressed as absolute rather than relative URIs 
* Where several files are published together, it is recommended that these are organised into a [Data Package](http://dataprotocols.org/data-packages/)
* Columns should use the name specified below. 
* Dates should be expressed according to XML Schema dates, e.g. `yyyy-mm-dd`

TODO: naming conventions?
TODO: notes about multi-lingual data?
TODO: anything further about packaging?

### Contests

A contest data file provides basic metadata about an electoral contest. This includes the date(s) during which the contest was held, the type of election and the electoral system being used.

|Column|Name|Required?|Type|Description|
|------|----|---------|----|-----------|
|1|Contest ID|Yes|URI|A URI providing a unique identifier for the individual electoral contest|
|2|Name|Yes|String|A name for the election, e.g. "UK Parliamentary Election 2015"|
|3|Election Type|Yes|String|The type of election. This should ideally draw from a controlled vocabulary|
|4|Start Date|Yes|Date|The date on which the electoral contest begun|
|5|End Date|Yes|Date|The date on which the electoral contest ended. Some contests run for a single day, in which case the start and end dates will be the same|
|6|Electoral System|No|String|Identifies the electoral system being used in the election. Ideally this should draw from a controlled vocabulary|

There are two fields in this format which are expected to draw on controlled vocabularies. The first defines the type of election, the second the electoral system being used to organise the contest. In this first draft of this specification these vocabularies have been left undefined to allow for further research and experimentation. However, using the United Kingdom as an example, valid values for Election Type might include:

* General Election
* European Parliament Election
* Mayoral Election
* Police Commissioner Election
* Local By-Election
* Local Government Election
* ...etc

Ideally the controlled vocabularies used to capture these variations would be defined by the national or regional electoral body publishing data for an election.

### Choices

The choices data file is used to publish a basic description of the choices presented to voters and a count of the valid votes associated with each choice. In some electoral systems voters are choosing between different individual people who are candidates in an election. These candidates may be independent or may have an affiliation with a political party. These affiliations are important to capture. 

However for some electoral systems, particular those using a proportional system that uses [an electoral list](https://en.wikipedia.org/wiki/Electoral_list), voters are choosing between parties rather than individual candidates. The actual candidates who take office are not directly voted for by the electoral and may be nominated by the winning party. An example is the European Parliamentary Elections. Voters in the EU choose a political party and then individual MEPs are allocated seats based on the number of votes won by each party. In this type of election no candidate details would be included in the choices file and an additional "seats" data file would be published (see below).

|Column|Name|Required?|Type|Description|
|------|----|---------|----|-----------|
|1|Contest ID|Yes|URI|A URI providing a unique identifier for the individual electoral contest|
|2|Region ID|Yes|URI|A URI identifying the region in which the contest is taking place|
|3|Region Name|String|Yes|The name of the region in which the contest is taking place|
|4|Party ID|No|URI|A URI identifiying a political party. If candidate details are provided then this indicates the candidate's affiliation. If no candidate details are provided then this should be understood as identifying the party for whom the electorate are voting|
|5|Party Name|No|String|The name of the identified political party|
|6|Candidate ID|No|URI|A URI identifying a candidate in the election|
|7|Candidate Name|No|String|The name of the identified candidate|
|8|Votes|No|Integer|The number of valid votes cast for the specific candidate or party|

### Seats

The seats data file is used to provide a list of the candidates who have been allocated granted political office following an election. The format of the file overlaps with the choices data file. However for elections based on votes choosing from [an electoral list](https://en.wikipedia.org/wiki/Electoral_list) the contents will be different to that of the choices data file. The choices file will provide the results for individual parties, and the seats data file will list the individual people voted into office.

|Column|Name|Required?|Type|Description|
|------|----|---------|----|-----------|
|1|Contest ID|Yes|URI|A URI providing a unique identifier for the individual electoral contest|
|2|Region ID|Yes|URI|A URI identifying the region in which the contest is taking place|
|3|Region Name|Yes|String|The name of the region in which the contest is taking place|
|4|Party ID|No|URI|A URI identifiying a political party with whom a candidate is affiliated|
|5|Party Name|No|String|The name of the identified political party|
|6|Candidate ID|No|URI|A URI identifying a candidate in the election|
|7|Candidate Name|No|String|The name of the identified candidate|
|8|Elected|No|Boolean|Indicates whether a candidate was elected|
|9|Rank|No|Integer|Some electoral systems rank or order candidates, this column provides a means to express that ordering.|

### Voting Data

The Voting Data file provides high-level statistics about the voting activity for an election, this provides a means for publishing data on returns for an election according to both the voting method used and the category of vote, e.g. valid or invalid.

|Column|Name|Required|Description|
|------|----|---------|----|-----------|
|1|Region ID|Yes|URI|A URI identifying the region in which the votes are being reported|
|2|Region Name|Yes|String|The name of the identified region|
|3|Electorate|Yes|Integer|The size of the electorate in the specified region. This column will have the same value for all rows.
|4|Vote Category|Yes|String|The category of vote being counted. This uses a controlled vocabulary defined below
|5|Voting Method|Yes|String|The voting method describes the method used by the voter to record their vote. This uses a controlled vocabulary defined below
|6|Votes|Yes|Integer|The number of votes with the specified method and category

The vote category column should use one of the following values. This vocabulary draws on [the definitions defined by the ACE Project](http://aceproject.org/ace-en/topics/vc/vce/vce02/vce02b)

|Category|Parent Category|Description|
|--------|---------------|----|-----------|
|Issued|-|All ballots issued, regardless of whether they were returned.
|Valid|Issued|All valid votes
|Spoilt|Issued|A spoiled ballot is generally one that a voter has inadvertently spoiled by marking it incorrectly; it is handed back to the voting station officers in exchange for a new blank ballot that is then marked by the voter and placed in the ballot box
|Invalid|Issued|An invalid ballot is one that has made its way into the ballot box, but has been rejected at the count because it was improperly marked, or is not marked at all when a mark is required. In some jurisdictions, blank ballots (ballots with no marks) are counted separately (and may be considered as protest votes), in others, they are considered to be rejected ballots
|Blank|Invalid|A ballot paper returned without any marks. In some cases these are counted separately as protest votes
|Rejected|Invalid|Ballot papers rejected at the count because they were deliberately or accidentally spoilt

The voting method column should use one of the following values

|Method|Definition|
|------|----------|
|In-Person|A vote recorded in person at a polling location|
|Proxy|A vote recorded via a proxy -- a nominated individual empowered to record a vote for another person -- at a polling station|
|Online|A vote recorded using an online e-voting system|
|Postal|A vote submitted by post|


## TODO

* Polling Stations
* Parties
