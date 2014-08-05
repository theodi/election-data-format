# Election Data Format

This project defines some open data formats design to support the publication of open election data to drive transparency and engagement on democratic processes.

The goal is to define formats that are suitable for use on an international basis, to support publication of election data from a wide variety of different sources and for different electoral systems. 

## Quick Links

* [Publishing Open Election Data](https://docs.google.com/document/d/1gyPpfod0eGzzutyZuak_xe5ZcY8hcWzH3onTScBoTPU/edit?usp=sharing) research paper
* [Election Data Tables](https://github.com/theodi/election-data-format/blob/gh-pages/tables/index.md) specification

## Background Research

The design of the data formats has been guided by the results of a short research project exploring current practices relating to the publication, collection and sharing of election data.

In addition to a reviewing of current practices, the research project explored some of the properties of different electoral systems in order to identify a simple conceptual model that could underpin the design of various data interchange formats.

The results of this project have been written up in a publicly accessible paper: __[Publishing Open Election Data](https://docs.google.com/document/d/1gyPpfod0eGzzutyZuak_xe5ZcY8hcWzH3onTScBoTPU/edit?usp=sharing)__.

## High-Level Requirements

As described in the [research paper](https://docs.google.com/document/d/1gyPpfod0eGzzutyZuak_xe5ZcY8hcWzH3onTScBoTPU/edit?usp=sharing) the decision was made to focus on supporting the publication of "post-election" metadata. There is significant benefit to be had in aiding the reporting, analysis, and auditing of votes and the communication of the results of the election. Well-defined data formats will help improve how data is currently reported, whilst also providing the groundwork for further work in other areas.

In order to gain the most benefits, election data should be:

* available from an authoritative, primary source
* available on a timely basis, e.g. published immediately results are announced
* released under an open licence
* published in standard formats to facilitate reuse
* published according to a standard model that will support both aggregation of data and customization to allow for regional differences in election processes

Two approaches will be defined: a tabular format and a graph (Linked Data) based format. Both of these will share the conceptual model outlined in the research paper. The graph based model will be derivable from the tabular formats.

## Election Data Tables

A tabular format for election data should be:

* Easy for election officials to generate both manually, e.g. using simple spreadsheet tools, and automatically, e.g. through database exports or APIs
* Easy for both citizens and developers to consume using a range of tools, including simple spreadsheet applications
* Customizable and/or extensible to allow for international and regional differences in types of election data
* Flexible enough to support reporting of data at different levels of granularity, e.g. at country, region and administrative district levels
* Make reference to standard terms, code lists and reference data to ensure that data is clearly documented
* Focused on supporting transparency of electoral processes, rather than driving process automation.

Read the draft __[Election Data Tables](https://github.com/theodi/election-data-format/blob/gh-pages/tables/index.md)__ specification.

View the example data files in the [tables](https://github.com/theodi/election-data-format/tree/gh-pages/tables) directory.

## Election Data Graph Model

Recognising that election data consists of statistical data (vote counts, etc) and reference data (geographies, controlled vocabularies) the graph model will be based on the RDF Data Cube.

TODO: publish draft vocabularies.

## License

Any code in this project is open source under the MIT license. See the LICENSE.md file for full details.

All content, including documentation and schemas are available to use a Creative Commons Attribution license. More details are available at https://creativecommons.org/licenses/by/4.0/

