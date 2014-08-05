# Tabular Election Data
_Leigh Dodds, Open Data Institute, 5th August 2014._

This specification defines a number of simple tabular (CSV) data formats intended to support the publication of open data about elections, including:

* the choices available to voters, e.g. candidates, parties, or referendum choices
* the results of elections, e.g. the number of votes cast for individual choices
* the outcomes of an election, e.g. the number of seats won, candidates elected into office, etc
* high-level statistics about the voting process, e.g. number of valid and invalid votes cast

The intention is to define some simple data formats that can be used to publish and aggregate data relating to a wide variety of different election types from a range of jurisdictions. The formats are based on a simple conceptual model of elections which provides a framework for further extension. This model focuses on describing the aspects common to all elections rather than the details of individual electoral processes.

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

An Election or _Contest_ takes place on a specific date, or over a period of days.

A single ballot paper may ask voters to make _Choices_ for several different elections. 

A Contest asks eligible voters to express a preference between several distinct options. The rules under which a Contest takes place, known as an _electoral system_, define details such as how many times a voter may vote, how votes are counted, etc. Contests are of a particular 'electoral type', e.g. a local government election, parliamentary election, etc. Capturing these details of an electoral system and the method of counting votes is outside the scope of this model.

However in every Contest voters are always making selections between several distinct options. These selections may represent a _Party_, individual _Candidates_ or, in the case of a referendum, between different political outcomes. At its simplest level, reporting of election results usually involves identifying which of these options were elected according to the rules of the electoral system, usually with a break down of the number of votes or the number of seats allocated.

Contests are conducted in specific 'Regions', e.g. an electoral district. Regions may be organised into a hierarchical administrative geography, allowing the reporting and aggregation of votes at multiple levels.

During a contest a voter may express their voting rights using a specific _Voting Method_, e.g. using a paper based ballot or electronic voting system. During the counting of votes, returning officers divide the votes cast into several different categories. The most two most common examples of a _Voting Category_ are valid and invalid votes. Invalid votes may be further divided into categories such as "spoilt" or "rejected".

Additional statistics may be derived from these vote counts, e.g. percentage of electorate choosing a candidate, or turnout for an election. As in some cases, particularly for turnout, there are various ways in which these statistics are calculated these are not covered by this model. The focus is instead on providing the raw data which can be the focus of additional analysis.

## Data Formats

The following sections define simple schemas for a number of tabular data formats. Each format is intended to support the publication of a specific type of election data.

The following general requirements apply to all formats:

* Files MUST be published as UTF-8 CSV files according to the [Model for Tabular Data and Metadata on the Web](http://w3c.github.io/csvw/syntax/)
* Identifiers for Regions, Parties, Candidates, etc MUST all be URLs. Where possible those URLs SHOULD be drawn from official sources. Ideally URLs SHOULD resolve into additional metadata about the resource, but the metadata made available and the formats provided are outside the scope of this specification.
* All URLs MUST be expressed as absolute rather than relative URIs 
* Where several files are published together, the files SHOUDL be organised into a [Data Package](http://dataprotocols.org/data-packages/)
* Column names MUST use the names given in the documentation and schemas defined below
* Column values may be empty to indicate null results but columns MUST NOT be omitted
* Dates MUST be either a valid XML Schema [date](http://www.w3.org/TR/xmlschema-2/#date) or [dateTime](http://www.w3.org/TR/xmlschema-2/#dateTime) value, e.g. `yyyy-mm-dd`
* Files may use any naming convention, however it is recommended that a standard convention is used, e.g. "`election-region-filetype.csv`"
* Files may contain data on a single election contest or may contain data about multiple contests. This provides data publishers with the flexibility to publish and append to a single set of files to share election data, or to publish individual data packages for each contest.

The following sections define 4 different tabular formats:

* __Contests__ -- metadata about electoral contests
* __Choices__ -- list of choices and basic vote counts
* __Results__ -- the outcomes of an electoral process
* __Voting Data__ -- basic statistics on the electoral process

For simple scenarios, e.g. [first-past-the-post](https://en.wikipedia.org/wiki/First_Past_the_Post_electoral_system) systems, a publisher MAY publish two data files: a Contest file providing contest metadata, and a Choices file which provides a list of candidates and vote counts. 

For anything other than these simple electoral contests, a publisher SHOULD also publish a Results data file to provide a summary of the election outcomes. 

It is RECOMMENDED that publishers always provide a Voting Data file in order to provide additional transparency on the electoral process. 

### Contests

A Contest data file provides basic metadata about an electoral contest. A file may contain data for a single contest, or a list of contests.

|Column|Name|Required?|Type|Description|
|------|----|---------|----|-----------|
|1|Contest ID|Yes|URI|A URI providing a unique identifier for the individual electoral contest|
|2|Name|Yes|String|A name for the election, e.g. "UK Parliamentary Election 2015"|
|3|Election Type|Yes|String|The type of election. This should ideally draw from a controlled vocabulary|
|4|Start Date|Yes|Date|The date on which the electoral contest begun|
|5|End Date|Yes|Date|The date on which the electoral contest ended. Some contests run for a single day, in which case the start and end dates will be the same|
|6|Electoral System|No|String|Identifies the electoral system being used in the election. Ideally this should draw from a controlled vocabulary|

There are two fields (`Election Type`, `Electoral System`) in this format which are expected to draw on controlled vocabularies. The first defines the type of election, the second the electoral system being used to organise the contest. In this first draft of this specification these vocabularies have been left undefined to allow for further research and experimentation. However, using the United Kingdom as an example, valid values for Election Type might include:

* General Election
* European Parliament Election
* Mayoral Election
* Police Commissioner Election
* Local By-Election
* Local Government Election
* ...etc

Ideally the controlled vocabularies used to capture these variations would be defined by the national or regional electoral body publishing data for an election. At a minimum, data publisher SHOULD document the list of values that they expect to use in this field along with a brief definition.

### Choices

An Electorial Choices data file is used to publish a basic description of the choices presented to voters and a count of the valid votes associated with each choice. This supports simple data publishing use cases where the only data available in vote count and the elected candidate(s) are those with the most votes. However in more complex systems the count of votes is used as the basis for calculating the winning party or candidate and is not a direct indication of success.

In some electoral systems voters are choosing between individual people who are standing for candidates in an election. These candidates may be independent or may have an affiliation with a political party. These affiliations are important to capture.

However for some electoral systems, particularly those using a proportional system that uses [an electoral list](https://en.wikipedia.org/wiki/Electoral_list), voters are choosing between parties rather than individual candidates. The actual candidates who take office are not directly voted for by the electoral and may be nominated by the winning party. An example is the European Parliamentary Elections. Voters in the EU choose a political party and then individual MEPs are allocated seats based on the number of votes won by each party. In this type of election the list of people being elected into office would be reported separately in an additional Results data file (see below).

|Column|Name|Required?|Type|Description|
|------|----|---------|----|-----------|
|1|Contest ID|Yes|URI|A URI providing a unique identifier for the individual electoral contest|
|2|Region ID|Yes|URI|A URI identifying the region in which the contest is taking place|
|3|Region Name|String|Yes|The name of the region in which the contest is taking place|
|4|Party ID|No|URI|A URI identifiying a political party. If candidate details are provided then this indicates the candidate's affiliation. If no candidate details are provided then this should be understood as identifying the party for whom the electorate are voting|
|5|Party Name|No|String|The name of the identified political party|
|6|Candidate ID|No|URI|A URI identifying a candidate in the election|
|7|Candidate Name|No|String|The name of the identified candidate. If not candidate is specified then the choice is at a Party level, not for individuals|
|8|Votes|No|Integer|The number of valid votes cast for the specific candidate or party|

### Results

The Results data file is used to provide a summary of the outcome of an election. The most basic use of this file is to indicate which party or candidate has been elected into office. In some cases, particularly where data is reported at an aggregate level, the results file may include just a list of parties and the number of legislative seats won by that party. Some electoral systems rank successful candidates, e.g. first and second choices in an electoral list, this information can also be captured in the results file.

The format of the file overlaps with the Choices data file, the basic set of columns is the same. In many electoral systems the content of the file will also overlap. For example the same list of candidates may be present in both files. But the decision to use two separate files draws from the observation that for some electoral systems, e.g. those using [an electoral list](https://en.wikipedia.org/wiki/Electoral_list) the choices presented to voters and the individuals who take office are different: voters may be voting for paries who then allocate their candidates into seats. In this case the content of the Results data file may be considerably different.

|Column|Name|Required?|Type|Description|
|------|----|---------|----|-----------|
|1|Contest ID|Yes|URI|A URI providing a unique identifier for the individual electoral contest|
|2|Region ID|Yes|URI|A URI identifying the region in which the contest is taking place|
|3|Region Name|Yes|String|The name of the region in which the contest is taking place|
|4|Party ID|No|URI|A URI identifiying a political party with whom a candidate is affiliated|
|5|Party Name|No|String|The name of the identified political party|
|6|Candidate ID|No|URI|A URI identifying a candidate in the election|
|7|Candidate Name|No|String|The name of the identified candidate|
|8|Elected|No|Boolean|Indicates whether a party or candidate was elected. This should always be true if the Seats column is non-zero|
|9|Seats|No|Integer|This column is used to support the publication of aggregate statistics, e.g. number of seats allocated to a party in a specific region. In cases where the Result file contains more fine-grained data, e.g. the list of individual winning or allocated candidates for a region, then the column value will always be 1. A empty or zero value indicates that the party or candidate was not allocated any seats|
|10|Rank|No|Integer|Some electoral systems rank or order candidates, this column provides a means to express that ordering.|

### Voting Data

The Voting Data file supports the publication of high-level statistics on the ballot activity for a specific election. This allows electoral bodies to provide details on the number of ballots cast in an election on a regional level, as well as broken down by the voting method used and the category of vote, e.g. valid or invalid.

|Column|Name|Required|Description|
|------|----|---------|----|-----------|
|1|Contest ID|Yes|URI|A URI providing a unique identifier for the individual electoral contest|
|2|Region ID|Yes|URI|A URI identifying the region in which the votes are being reported|
|3|Region Name|Yes|String|The name of the identified region|
|4|Electorate|Yes|Integer|The size of the electorate in the specified region. This column is expected to have the same value for all rows that relate to the same contest and region.
|5|Vote Category|Yes|String|The category of vote being counted. This uses a controlled vocabulary defined below
|6|Voting Method|Yes|String|The voting method describes the method used by the voter to record their vote. This uses a controlled vocabulary defined below
|7|Votes|Yes|Integer|The number of votes with the specified method and category

The `Vote Category` column SHOULD use one of the following values. This vocabulary draws on [the definitions defined by the ACE Project](http://aceproject.org/ace-en/topics/vc/vce/vce02/vce02b)

|Category|Parent Category|Description|
|--------|---------------|----|-----------|
|Issued|-|All ballots issued, regardless of whether they were returned.
|Valid|Issued|All valid votes
|Spoilt|Issued|A spoiled ballot is generally one that a voter has inadvertently spoiled by marking it incorrectly; it is handed back to the voting station officers in exchange for a new blank ballot that is then marked by the voter and placed in the ballot box
|Invalid|Issued|An invalid ballot is one that has made its way into the ballot box, but has been rejected at the count because it was improperly marked, or is not marked at all when a mark is required. In some jurisdictions, blank ballots (ballots with no marks) are counted separately (and may be considered as protest votes), in others, they are considered to be rejected ballots
|Blank|Invalid|A ballot paper returned without any marks. In some cases these are counted separately as protest votes
|Rejected|Invalid|Ballot papers rejected at the count because they were deliberately or accidentally spoilt

If a data publisher wishes to expose additional statistics then they SHOULD provide documentation on the categories being reported, including a clear definition of how ballots are allocated to that category.

The `Voting Method` column SHOULD use one of the following values:

|Method|Definition|
|------|----------|
|In-Person|A vote recorded in person at a polling location|
|Proxy|A vote recorded via a proxy -- a nominated individual empowered to record a vote for another person -- at a polling station|
|Online|A vote recorded using an online e-voting system|
|Postal|A vote submitted by post|

## Future Work

Future work on this specification could include definitions of data formats for the following:

* Polling Stations -- these are geographic locations and are likely to be well supported by existing standards. However it may be useful to define a simple tabular format to supplement the details provided here
* Controlled Vocabularies -- defining additional controlled vocabularies or a means to publish simple tabular vocabularies which map to SKOS.
* Parties -- metadata on political parties covering identifiers, logos, and potentially coalitions/alliances
* Candidates -- basic metadata about political candidates, again this is already well covered by other formats, but a companion format may be useful.
