---
title: 'XGI: A Python package for research on higher-order interactions'
tags:
  - python
  - higher-order
  - hypergraph
  - simplicial complex
authors:
  - name: Nicholas W. Landry
    orcid: 0000-0003-1270-4980
    affiliation: "1, 2" # (Multiple affiliations must be quoted)
    corresponding: true
  - name: Leo Torres
    orcid: 0000-0002-2675-2775
    affiliation: 2
  - name: Maxime Lucas
    orcid: 0000-0001-8087-2981
    affiliation: 4
  - name: Iacopo Iacopini
    orcid: 0000-0001-8794-6410
    affiliation: 5
  - name: Giovanni Petri
    orcid: 0000-0003-1847-5031
    affiliation: 4
  - name: Alice Patania
    orcid: 0000-0002-3047-4376
    affiliation: "1, 2"
  - name: Alice Schwarze
    affiliation: 6
  - name: Martina Contisciani
    orcid: 0000-0002-6103-5499
    affiliation: 7
affiliations:
 - name: Vermont Complex Systems Center, USA
   index: 1
 - name: Department of Mathematics and Statistics, University of Vermont, USA
   index: 2
 - name: Max Planck Institute for Mathematics in the Sciences, Germany
   index: 3
 - name: CENTAI Institute, Italy
   index: 4
 - name: Department of Network and Data Science, Central European University, Austria
   index: 5
 - name: Department of Mathematics, Dartmouth College, USA
   index: 6
 - name: Max Planck Institute for Intelligent Systems, Germany

date: 03/12/2023
bibliography: references.bib

---

# Summary

The Comple**X** **G**roup **I**nteractions (XGI) library provides data structures and algorithms for modeling and analyzing complex systems with group (higher-order) interactions.

Many datasets can be represented as graphs, where pairs of entities (or nodes) are related via links (or edges). Examples are road networks, energy grids, social networks, neural networks, etc. However, in many other datasets, more than two entities can be related at a time. For example, many scientists (entities) can collaborate on a scientific article together (links), and multiple email accounts (entities) can all participate on the same email thread (links). In this latter case, graphs no longer present a viable alternative to represent such datasets. It is for this kind of datasets, where the interactions are given among groups of more than two entities (also called higher-order interactions), that XGI was designed for.

XGI is implemented in pure Python and is designed to seamlessly interoperate with the rest of the Python scientific stack (numpy, scipy, pandas, matplotlib, etc). XGI is designed and developed by network scientists with the needs of network scientists in mind.

# Statement of need

The accessibility and inclusivity of a scientific community rests, in part, on its commitment to share resources. For the pairwise network community, packages such as NetworkX [@SciPyProceedings_11], graph-tool [@peixoto_graph-tool_2014], and igraph have provided a common language and tools allowing cross-disciplinary researchers to more easily collaborate and contribute to the field of network science.

The sub-field of higher-order network science has grown rapidly over the past few years, garnering wide attention for its ability to offer additional insights for machine learning, the structure of complex systems, the dynamics of complex systems, and many other applications. This topic is relevant to many different fields such as computer science, infectious diseases, dynamical systems, behavioral science, and many others. Open-source code is available for hypergraph dynamics [@iacopini_simplicial_2019;@st-onge_social_2021;@landry_effect_2020], inference [@benson_simplicial_2018;@chodrow_generative_2021;@young_bayesian_2021], generative models [@chodrow_configuration_2020;@landry_effect_2020;@antelmi_analyzing_2020], algorithms [@doecode_22160;@benson_three_2019], and data [@benson_data_2021] written in languages such as Python, Julia, C++, and Matlab, but these resources are relatively piecemeal. We have developed the Comple**X** **G**roup **I**nteractions (XGI) Python library to provide an open-source resource for this community [@Landry_XGI].

# Overview of the API

We provide an overview of the functionality of the XGI python library.

## Data structures

In this section, we describe the data structures that we use.

### Hypergraphs

The data structure for hypergraphs employed by XGI leverages the bipartite representation of a hypergraph which allows users to efficiently query the hyperedges of which a specified node is a part and the nodes that are members of a specified hyperedge. Each hyperedge is assigned an ID which allows for multi-hyperedges. The hypergraph data structure provided by XGI offers methods for easily getting the following:

* The number of nodes
* The number of hyperedges
* The degree sequence
* The hyperedge size sequence
* The singleton edges
* Subhypergraphs
* Attributes of nodes and hyperedges (Such as weights, names, etc.)

### Simplicial Complexes

### Mathematical representations

A hypergraph may be represented in a variety of ways including an incidence matrix, a bipartite network, a list of hyperedges, and many others [@battiston_networks_2020]. Different applications require different hypergraph representations for efficient computation. For example, when modeling contagion, a node's infection status may change dependent on the statuses of its neighbors, indicating that a representation allowing efficient access to the node's edge neighbors is desirable. Likewise, when one is interested in computing properties of a hypergraph that is averaged over the hyperedges such as assortativity or modularity, it may be most efficient to represent a hypergraph by a list of its hyperedges.

XGI provides methods for users to convert between a hypergraph and the following formats:

* A list of hyperedges
* An adjacency list
* An incidence matrix
* A bipartite network

## File I/O

There are many sources of hypergraph data [@benson_data_2021;@clauset_colorado_2016;@peixoto_netzschleuder_2021], often in very different formats which is a significant overhead cost for researchers. XGI alleviates this cost in two ways: first, by implementing methods for importing and writing hypergraphs from several different formats and second, by implementing a standard for hypergraph data in JSON format.

The XGI library offers 4 main types of file input and output:

* A list of hyperedges, where each line of the file is a hyperedge
* A list of bipartite edges, where each line is a (node, edge) entry
* An incidence matrix, where each column corresponds to a particular hyperedge, and the non-zero entries correspond to the nodes that participate in that hyperedge.
* A JSON file.

## Algorithms

There are two types of algorithms that XGI supports: generative random models and methods for analyzing the connectedness of a hypergraph. Generative models are important for the generation of synthetic datasets, which are useful as null models to compare empirical datasets, create datasets with similar characteristics as another dataset, or control structural properties such as assortative mixing or degree heterogeneity. XGI currently implements several different generative models:

* The Chung-Lu model for hypergraphs
* The Degree-Corrected Stochastic Block Model (DCSBM) model for hypergraphs
* The Erd\"os-R\'enyi model for hypergraphs
* The configuration model for uniform hypergraphs


Measuring connectedness in hypergraphs is important as a data exploration tool because many theoretical frameworks assume that a hypergraph is connected and this may not be the case. XGI currently provides the following functionality for users:

* Determining whether or not a hypergraph is connected
* Finding the connected components of a hypergraph
* Finding the component of which a specified node is a part

## Visualization

## Dynamics

# XGI-DATA

With larger datasets becoming more widely available, it is important to close the gap between dataset creators and consumers [@gebru_datasheets_2021] because using data without an underlying knowledge of its creation and limitations is an incomplete picture. Although there are many excellent collections of hypergraph datasets [@benson_data_2021;@peixoto_netzschleuder_2021;@clauset_colorado_2016], the format of each dataset and the information about how and why it was collected varies widely. Creating a *datasheet* for each dataset detailing characteristics, limitations, and surrounding factors that informed the collection of that data is a responsible practice in the curation of datasets [@gebru_datasheets_2021]. There is significant work to be done so that researchers can easily access a summary of the dataset's statistics, a description of the collection process, limitations of the dataset, and other relevant information. In addition, a standard format for storing hypergraph datasets with an accompanying standard will reduce the overhead time and increase accuracy for researchers working on cross-disciplinary datasets.

The XGI-DATA repository [@Landry_xgi-data] is a collection of openly available hypergraph datasets in JSON format with documentation more extensively describing the datasets. There is also a rudimentary inspection script for checking that datasets are in the proper format.

# Projects dependent on XGI

* hypercontagion

<!-- # Figures -->

<!-- Figures can be included like this:
![Caption for example figure.\label{fig:example}](figure.png)
and referenced from text using \autoref{fig:example}. -->

<!-- Figure sizes can be customized by adding an optional second parameter:
![Caption for example figure.](figure.png){ width=20% } -->

# Funding

The XGI package has been supported by NSF Grant 2121905, "HNDS-I: Using Hypergraphs to Study Spreading Processes in Complex Social Networks".

# Acknowledgements

We acknowledge contributions from Tim LaRock, Marco Nurisso, and Sabina Adhikari.



# References