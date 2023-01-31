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
 - name: Vermont Complex Systems Center, University of Vermont, USA
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

notes: Authors have been ordered by date of arrival to the project and this order does not reflect their contributions to the project.

date: 03/12/2023
bibliography: references.bib

---

# Summary
The Comple**X** **G**roup **I**nteractions (XGI) library provides data structures and algorithms for modeling, analyzing, and visualizing complex systems with group (higher-order) interactions. At its core are hypergraphs and simplicial complexes. XGI provides multiple ways of building these objects—by hand, from data, or from generative models—and a range of standard and state-of-the-art algorithms and measures to analyze their structure. The library also offers flexible visualization functions with suitable layouts. Additionally, XGI provides basic functions to simulate dynamical processes on top of hypergraphs and simplicial complexes. XGI is implemented in pure Python, and is designed to seamlessly interoperate with the rest of the Python scientific stack (Numpy, Scipy, Pandas, Matplotlib, NetworkX). XGI is designed and developed by network scientists with the needs of network scientists in mind.

# Statement of need
The field of network science bridges across many different disciplines, bringing together theorists, computational scientists, social scientists, and many others, each with vastly different expertise. To facilitate collaboration and cross-disciplinary work, it is necessary to develop a tool kit that decreases the overhead cost of these collaborations. For pairwise networks, i.e. networks composed by units that interact in pairs, packages such as NetworkX [@hagberg_exploring_2008], graph-tool [@peixoto_graph-tool_2014], and igraph [@igraph] have provided a common framework for cross-disciplinary researchers to more easily collaborate and contribute to the field of network science. The subfield of higher-order network science has grown rapidly over the past few years, garnering wide attention for its ability to model interactions that cannot be reduced to a pairwise representation. Rich behavior can emerge from dynamics on higher-order interaction networks [@iacopini_simplicial_2019;@skardal_abrupt_2019;@neuhauser_multibody_2020;@hickok_bounded-confidence_2022], and often higher-order networks can more accurately model many empirical interaction patterns [@chodrow_configuration_2020]. These developments will have lasting impacts on fields such as computer science, infectious diseases, dynamical systems, behavioral science, and many others. Open-source algorithms and scripts are available as individual resources for published work, but these resources are not generalizable to the needs of the community at large. In addition, there are data repositories dedicated to higher-order datasets [@benson_data_2021], but with neither a standard format nor an easy method for loading these datasets. We have developed the Comple**X** **G**roup **I**nteractions (XGI) Python library to provide an open-source solution to support the higher-order network science community.

# Related Software
There are several existing packages to represent and analyze higher-order networks: `HyperNetX` [@doecode_22160] and `Reticula` [@badie-modiri_reticula_2022] in Python, `SimpleHypergraphs.jl` [@spagnuolo_analyzing_2020]  and `HyperGraphs.jl` [@diaz_hypergraphsjl_2022] in Julia, and `hyperG` in R. XGI is a valuable addition to the network science practitioner’s toolbox for several reasons. First, XGI emphasizes interoperability through its pure Python implementation, making installation as easy as a `pip install` for researchers on a variety of operating systems. Second, like several of the packages listed, XGI has a well-documented codebase with corresponding tutorials designed to make learning XGI an intuitive process. Third, unlike many existing packages, XGI contains a `stats` module enabling researchers to easily access nodal and edge quantities of interest as well as define custom quantities. Fourth, XGI offers data structures for not only hypergraphs but simplicial complexes as well, making this library useful to a wider variety of research areas. Lastly, XGI integrates higher-order datasets with its interface, providing not only a standard format in which to store hypergraphs with attributes, but also a data repository with corresponding functions to load these datasets.

# Overview of the API
We provide an overview of the functionality of the XGI library.

## Core architecture: hypergraphs and simplicial complexes
The two core classes of the library are those representing hypergraphs and simplicial complexes. The data structure employed by XGI for those two is a bipartite graph with entities represented by one node type and relationships among entities (i.e., hyperedges or simplices) represented by a second node type. Practically, this is implemented as two dictionaries: one mapping each node to the hyperedge (or simplex) IDs of which it is a member, and another mapping each hyperedge (or simplex) to its member nodes instead.

![Illustration of the underlying data structure. The hypergraph is internally represented as a bipartite network stored as two dictionaries, where keys are the node or edge IDs and the values are sets specifying which edges (or nodes) that node (edge) is connected to. Unique identifiers allow for multi-edges, as can be seen for edge IDs 1 and 2. \label{fig:diagram}](Figures/fig1_joss.pdf)

This data structure (seen in \autoref{fig:diagram}) is flexible and allows users to efficiently query relationships between nodes and hyperedges. Each hyperedge is assigned a unique ID, which is either user-provided or internally generated. This choice allows multi-edges, but loopy hyperedges (i.e., those that contain the same node more than once) are forbidden because the bipartite relationships are stored as sets. Multi-edges are not allowed, however, for simplicial complexes. In fact, by definition, simplicial complexes need to respect the inclusion condition which, in XGI, is enforced when adding and removing simplices. Lastly, two dictionaries store the attributes of the nodes and edges respectively.

## Creating and manipulating

XGI provides several ways to create hypergraphs and simplicial complexes, such as: 

* Manually adding and removing hyperedges
* Converting from data in various formats
* Generative models
* From data files and stored objects  

Below, we detail each of these options.

### Generative models
Generative models are important for the generation of synthetic datasets, which are useful as null models to compare empirical datasets, create datasets with similar characteristics as another dataset, or control structural properties such as assortative mixing or degree heterogeneity. XGI currently implements several different generative models:

* The Chung-Lu model for hypergraphs [@aksoy_measuring_2017]
* The Degree-Corrected Stochastic Block Model (DCSBM) model for hypergraphs [@larremore_efficiently_2014]
* The Erdős–Rényi model for hypergraphs [@dewar_subgraphs_2016]
* The configuration model for uniform hypergraphs [@landry_effect_2020]
* The flag complex (or clique complex) of a graph [@kahle_topology_2009]
* The random simplicial complex [@iacopini_simplicial_2019]

### File I/O
Higher-order network datasets are often stored in a variety of different formats [@benson_data_2021;@clauset_colorado_2016;@peixoto_netzschleuder_2021], which can be a significant overhead cost for researchers trying to analyze empirical datasets. XGI alleviates this cost in two ways: first, by implementing methods for importing and writing hypergraphs from several common formats and second, by implementing a standard for hypergraph data in JSON format. The XGI library offers 4 main types of file input and output:

* A list of hyperedges, where each line of the file is a hyperedge
* A list of bipartite edges, where each line is a (node, edge) entry
* An incidence matrix, where each column corresponds to a particular hyperedge, and the non-zero entries correspond to the nodes that participate in that hyperedge.
* A JSON file according to the xgi-data standard.

### Converting between formats

Once a higher-order network is created, it may be represented in many different ways [@battiston_networks_2020], e.g. an edge list or an incidence matrix,  and different applications require different hypergraph representations for efficient computation. For example, when modeling contagion, a node's infection status may change depending on the statuses of its neighbors, indicating that a representation allowing efficient access to the node's edge neighbors is desirable. Likewise, when one is interested in computing properties of a hypergraph that is averaged over the hyperedges such as assortativity or modularity, it may be most efficient to represent a hypergraph by the list of its hyperedges. XGI provides methods for users to convert between a hypergraph and multiple formats, including: 
(i) an edge list, adjacency list, or bipartite edge list; 
(ii) an incidence matrix, adjacency matrix, or Laplacian matrix; and 
(iii) a bipartite graph. 
The two core classes can also be instantiated from input data in any of these formats.

### Manipulating the structure
The hypergraphs and simplicial complexes may be modified by adding or removing nodes and hyperedges (simplices). XGI also provides functions for more complex manipulations such as swapping node and edge memberships.

## Analyzing
For hypergraphs and simplicial complexes, XGI offers methods for easily getting common basic outputs, including the number of nodes or hyperedges, the nodes that are members of a particular edge, and -- conversely -- the edges to which a node belongs to, subsets of hypergraphs, attributes of nodes or hyperedges. Below, we detail the stats subpackage, as well as more complex measures and dynamic simulations available in XGI.

### Stats
The core network classes (i.e. `Hypergraph` and `SimplicialComplex`) provide an interface with which to build the nodes and links of a network, whereas the `stats` package provides a way to compute summary statistics and other quantities of interest from these networks. The main class defined by the `stats` package is `NodeStat`, which is an abstract representation of a mapping from a node to a quantity. For example, the degree of a node (i.e. the number of edges it belongs to) is a quantity that assigns an integer to each node in the network, thus it is a node-to-quantity mapping. The degree in XGI is available via the `nodes` attribute of a network:

```
>>> H = xgi.Hypergraph([[0], [0, 1], [1, 2, 3]])
>>> H.nodes.degree
NodeStat('degree')
```

`NodeStat` objects are lazily evaluated, so a specific output type must be requested:

```
>>> H.nodes.degree.asdict()
{0: 2, 1: 2, 2: 1, 3: 1}
>>> H.nodes.degree.aslist()
[2, 2, 1, 1]
```

The main benefit of the `stats` package is that any other notion that can be conceived of as a node-to-quantity mapping has the same interface. This includes notions such as other centrality measures, categorical node attributes, and even user-defined functions. Furthermore, all of these are given the exact same interface. For example, obtaining the average degree or average clustering coefficient over the entire network (or the average of any other node-to-quantity mapping) is done with a single method call:

```
>>> H.nodes.degree.mean()
1.5
>>> H.nodes.clustering.mean()
0.25
```

Multiple statistics can be handled at the same time. This example computes two statistics and outputs them in a pandas DataFrame, ready for subsequent processing:

```
>>> H.nodes.multi(["degree", "clustering"]).aspandas()
   degree  clustering
0       2         0.0
1       2         1.0
2       1         0.0
3       1         0.0
```

An analogous object for edge-to-quantity mappings is provided via `EdgeStat`.

### Algorithms
As the field of network science grows, standard algorithms are being developed to describe the structure of higher-order networks. These algorithms include metrics such as the assortativity, centralities, or clustering coefficient; the connectedness or connected components; the community labels of nodes and different clustering objectives.
While XGI has not yet exhaustively implemented every metric of hypergraph structure so far, it has incorporated important measures of assortativity, centrality, connectedness, and clustering, and it will continue to incorporate more of these metrics in the future.

### Dynamics
Much research is interested not only in the structure of (higher-order) networks, but also in the dynamical processes that can take place on top of them. Currently, XGI provides functions to simulate two types of synchronization models on hypergraphs: one where oscillators are placed only on the nodes of the hypergraphs [@adhikari_synchronization_2022;@lucas_multiorder_2020], and one where oscillators can also be placed on simplices [@millan_explosive_2020;@arnaudon_connecting_2022]. In the future, the library could be extended to other landmark dynamical processes on higher-order networks such as spreading, diffusion, and socio-physics models.

## Visualizing
The `draw()` function in XGI relies heavily on NetworkX and Matplotlib and allows the user to visualize both hypergraphs and simplicial complexes. \autoref{fig:viz} illustrates an example of a hypergraph visualization. XGI currently offers four different algorithms for calculating the nodal positions used when drawing. The function is tremendously flexible: edge color and node size, face color, and border width and color can be user specified, a constant value or colored based on nodal or edge statistics. This flexibility is illustrated in \autoref{fig:viz} where nodes are colored and sized by the degree and centrality respectively.

![A visualization of the email-enron dataset [@landry_xgi-data_2023;@benson_data_2021] with hyperedges of sizes 2 and 3 (all isolated nodes removed). The nodes are colored by their degree and their size proportional to the Clique motif Eigenvector Centrality [@benson_three_2019]. \label{fig:viz}](Figures/fig_2_joss.pdf)

When drawing simplicial complexes, the draw function only displays the pairwise links contained in each maximal simplex (while omitting simplices of intermediate orders) to reduce the clutter in the visualization. Another tool to reduce the clutter when visualizing hypergraphs and simplicial complexes is the specification of the maximum edge size to plot.

# XGI-DATA
With larger datasets becoming more widely available, it is important to close the gap between dataset creators and consumers [@gebru_datasheets_2021] because using data without an underlying knowledge of its creation and limitations is an incomplete picture. Although there are many excellent collections of hypergraph datasets [@benson_data_2021;@peixoto_netzschleuder_2021;@clauset_colorado_2016], the format of each dataset and the information about how and why it was collected varies widely. Creating a *datasheet* for each dataset detailing characteristics, limitations, and surrounding factors that informed the collection of that data is a responsible practice in the curation of datasets [@gebru_datasheets_2021]. There is significant work to be done so that researchers can easily access a summary of the dataset's statistics, a description of the collection process, limitations of the dataset, and other relevant information. In addition, a standard format for storing hypergraph datasets with an accompanying standard will reduce the overhead time and increase accuracy for researchers working on cross-disciplinary datasets.

The XGI-DATA repository [@landry_xgi-data_2023] is a collection of openly available hypergraph datasets in JSON format with documentation that describes each one extensively. There is also a rudimentary inspection script for checking that datasets are in the proper format.

# Projects using XGI
One of the goals of XGI was to provide a common language and framework on top of which many projects could be built. Even in its nascence, XGI has proved to be an invaluable resource for research projects [@zhang_higher-order_2022] on higher-order networks as well as other software projects [@landry_hypercontagion_2022]. We fully expect that as this library matures, it will become a more essential part of the higher-order network science community.

# Funding
The XGI package has been supported by NSF Grant 2121905, "HNDS-I: Using Hypergraphs to Study Spreading Processes in Complex Social Networks".

# Acknowledgements
We acknowledge contributions from Martina Contisciani, Tim LaRock, Marco Nurisso, Alexis Arnaudon, and Sabina Adhikari.

# References

