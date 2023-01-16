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
    affiliation: "1, 2"
    email: nicholas.landry@uvm.edu
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
    orcid: 0000-0002-9146-8068
    affiliation: 5
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

date: 03/12/2023
bibliography: references.bib

---

# Summary
The Comple**X** **G**roup **I**nteractions (XGI) library provides data structures and algorithms for modeling and analyzing complex systems with group (higher-order) interactions.

Many datasets can be represented as graphs, where pairs of entities (or nodes) are related via links (or edges). Examples are road networks, energy grids, social networks, neural networks, etc. However, in many other datasets, more than two entities can be related at a time. For example, many scientists (entities) can collaborate on a scientific article together (links), and multiple email accounts (entities) can all participate on the same email thread (links). In this latter case, graphs no longer present a viable alternative to represent such datasets. It is for this kind of datasets, where the interactions are given among groups of more than two entities (also called higher-order interactions), that XGI was designed for.

XGI is implemented in pure Python and is designed to seamlessly interoperate with the rest of the Python scientific stack (numpy, scipy, pandas, matplotlib, NetworkX). XGI is designed and developed by network scientists with the needs of network scientists in mind.

# Statement of need
The field of network science often bridges across many different disciplines, bringing together theorists, computational scientists, social scientists, and many others, each with vastly different expertise. In order to facilitate collaboration and cross-disciplinary work, it is necessary to develop a tool kit that decreases the overhead cost of these collaborations. For pairwise networks, packages such as NetworkX [@SciPyProceedings_11], graph-tool [@peixoto_graph-tool_2014], and igraph have provided a common framework for cross-disciplinary researchers to more easily collaborate and contribute to the field of network science. The subfield of higher-order network science has grown rapidly over the past few years, garnering wide attention for its ability to model interactions that cannot be reduced to a pairwise representation. Rich behavior can emerge from dynamics on higher-order interaction networks [@iacopini_simplicial_2019;@hickok_bounded-confidence_2022;@neuhauser_multibody_2020] and some higher-order networks can more accurately model many empirical interaction patterns [@chodrow_configuration_2020]. These developments will have lasting impacts on fields such as computer science, infectious diseases, dynamical systems, behavioral science, and many others.  Open-source algorithms and scripts are available as individual resources for published work but these resources are not generalizable to the needs of the community at large. In addition, there are data repositories dedicated to higher-order datasets [@benson_data_2021] but with neither a standard format nor an easy method for loading these datasets. We have developed the Comple**X** **G**roup **I**nteractions (XGI) Python library to provide an open-source solution to support the higher-order network science community.

# Related Software
There are several existing packages to represent and analyze higher-order networks: `HyperNetX` [@doecode_22160] and `Reticula` [@badie-modiri_reticula_2022] in Python, `SimpleHypergraphs.jl` [@spagnuolo_analyzing_2020]  and `HyperGraphs.jl` [@diaz_hypergraphsjl_2022] in Julia, and `hyperG` in R. XGI is a valuable addition to the toolbox of a network science practitioner for several different reasons. First, XGI emphasizes ease-of-use. This is evident in the pure Python implementation making installation easy for all operating systems, the well-documented codebase, and the `stats` module enabling researchers to easily access quantities of interest. In addition, XGI offers data structures for both hypergraphs and simplicial complexes, which makes this library useful to a wider variety of research areas. Lastly, XGI integrates higher-order datasets with its interface, not only providing a standard format in which to write hypergraphs with attributes, but also a data repository with corresponding functions to load these datasets.

# Overview of the API
We provide an overview of the functionality of the XGI python library.

## Core architecture
The data structure for hypergraphs and simplicial complexes employed by XGI is a bipartite graph with entities represented by one node class and relationships among entities (i.e. hyperedges or simplices) represented by another node class. [Insert Diagram] This is modeled as two dictionaries: one mapping the hyperedge (or simplex) IDs of which a node is a member and another specifying the nodes that are members of a given hyperedge (or simplex). This data structure is flexible and allows users to efficiently query relationships between nodes and hyperedges. Each hyperedge is assigned a unique ID which is user-provided or internally generated. This choice allows multiedges, but loopy edges (i.e. those that contain the same node more than once) are forbidden because the bipartite relationships are stored as sets. Lastly, two dictionaries store the attributes of the nodes and edges respectively.

## Hypergraphs, simplicial complexes, and mathematical representations
The hypergraph data structure provided by XGI offers methods for easily getting, for example, the number of nodes and hyperedges, the nodes that are members of a particular edge and conversely the edges to which a node belongs, subsets of hypergraphs, attributes of nodes and hyperedges, among other quantities of interest. It also allows for easy creation of a hypergraph as well as adding nodes and edges with and without attributes.

[SIMPLICIAL COMPLEXES HERE]

A hypergraph may be represented in many different ways [@battiston_networks_2020] and different applications require different hypergraph representations for efficient computation. For example, when modeling contagion, a node's infection status may change depending on the statuses of its neighbors, indicating that a representation allowing efficient access to the node's edge neighbors is desirable. Likewise, when one is interested in computing properties of a hypergraph that is averaged over the hyperedges such as assortativity or modularity, it may be most efficient to represent a hypergraph by a list of its hyperedges. XGI provides methods for users to convert between a hypergraph and, among other things, an edge list, adjacency list, or bipartite edge list; an incidence matrix, adjacency matrix, or Laplacian matrix; or a bipartite graph.

## Stats
In XGI, the core network classes (i.e. `Hypergraph` and `SimplicialComplex`) provide an interface with which to build the nodes and links of a network, whereas the `stats` package provides a way to compute summary statistics or other quantities of interest from these networks. The main class defined by the `stats` package is `NodeStat`, which is an abstract representation of a mapping from a node to a quantity. For example, the degree of a node (i.e. the number of edges it belongs to) is a quantity that assigns an integer to each node in the network, thus it is a node-to-quantity mapping. The degree in XGI is available via the `nodes` attribute of a network:

```python
>>> H = xgi.Hypergraph([[0], [0, 1], [1, 2, 3]])
>>> H.nodes.degree
NodeStat('degree')
```

`NodeStat` objects are lazily evaluated, so a specific output type must be requested:

```python
>>> H.nodes.degree.asdict()
{0: 2, 1: 2, 2: 1, 3: 1}
>>> H.nodes.degree.aslist()
[2, 2, 1, 1]
```

The main benefit of the `stats` package is that any other notion that can be conceived of as a node-to-quantity mapping has the same interface. This includes notions such as other centrality measures, categorical node attributes, and even user-defined functions. Furthermore, all of these are given the exact same interface. For example, obtaining the average degree or average clustering coefficient over the entire network (or the average of any other node-to-quantity mapping) is done with a single method call:

```python
>>> H.nodes.degree.mean()
1.5
>>> H.nodes.clutering.mean()
0.25
```

Multiple statistics can be handled at the same time. This example computes two statitics and outputs them in a pandas DataFrame, ready for subsequent processing:

```python
>>> H.nodes.multi(["degree", "clustering"]).aspandas()
   degree  clustering
0       2         0.0
1       2         1.0
2       1         0.0
3       1         0.0
```

An anologous object for edge-to-quantity mappings is provided via `EdgeStat`.

## File I/O
Higher-order network datasets are often stored in a variety of different formats [@benson_data_2021;@clauset_colorado_2016;@peixoto_netzschleuder_2021], which can be a significant overhead cost for researchers trying to analyze empirical datasets. XGI alleviates this cost in two ways: first, by implementing methods for importing and writing hypergraphs from several common formats and second, by implementing a standard for hypergraph data in JSON format. The XGI library offers 4 main types of file input and output:

* A list of hyperedges, where each line of the file is a hyperedge
* A list of bipartite edges, where each line is a (node, edge) entry
* An incidence matrix, where each column corresponds to a particular hyperedge, and the non-zero entries correspond to the nodes that participate in that hyperedge.
* A JSON file according to the xgi-data standard. [ADD A JSON SCHEMA?]

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
[Graphic here]

## Dynamics
Perhaps this can be lumped with algorithms?

# XGI-DATA
With larger datasets becoming more widely available, it is important to close the gap between dataset creators and consumers [@gebru_datasheets_2021] because using data without an underlying knowledge of its creation and limitations is an incomplete picture. Although there are many excellent collections of hypergraph datasets [@benson_data_2021;@peixoto_netzschleuder_2021;@clauset_colorado_2016], the format of each dataset and the information about how and why it was collected varies widely. Creating a *datasheet* for each dataset detailing characteristics, limitations, and surrounding factors that informed the collection of that data is a responsible practice in the curation of datasets [@gebru_datasheets_2021]. There is significant work to be done so that researchers can easily access a summary of the dataset's statistics, a description of the collection process, limitations of the dataset, and other relevant information. In addition, a standard format for storing hypergraph datasets with an accompanying standard will reduce the overhead time and increase accuracy for researchers working on cross-disciplinary datasets.

The XGI-DATA repository [@Landry_xgi-data] is a collection of openly available hypergraph datasets in JSON format with documentation more extensively describing the datasets. There is also a rudimentary inspection script for checking that datasets are in the proper format.

# Projects using XGI
* hypercontagion
* https://github.com/maximelucas/higherorder_sync_promoted


# Funding
The XGI package has been supported by NSF Grant 2121905, "HNDS-I: Using Hypergraphs to Study Spreading Processes in Complex Social Networks".

# Acknowledgements
We acknowledge contributions from Martina Contisciani, Tim LaRock, Marco Nurisso, Alexis Arnaudon, and Sabina Adhikari.

# Future Work
The XGI library remains under active development.

# References
