---
title: 'XGI: A Python package for research on higher-order interactions'
tags:
  - python
  - higher-order
  - hypergraph
  - simplicial complex
authors:
  - name: Nicholas W. Landry
    orcid: 0000-0000-0000-0000
    affiliation: "1, 2" # (Multiple affiliations must be quoted)
    corresponding: true
  - name: Leo Torres
    affiliation: 2
  - name: Maxime Lucas
    affiliation: 4
  - name: Iacopo Iacopini
    affiliation: 5
  - name: Giovanni Petri
    affiliation: 4
  - name: Alice Patania
    affiliation: "1, 2"
  - name: Alice Schwarze
    affiliation: 6
  - name: Martina Contisciani
    affiliation: 7
affiliations:
 - name: Vermont Complex Systems Center, USA
   index: 1
 - name: Department of Mathematics, University of Vermont, USA
   index: 2
 - name: Max Planck Institute, Germany
   index: 3
 - name: CENTAI Institute, Italy
   index: 4
 - name: Department of Network and Data Science, Central European University, Austria
   index: 5
 - name: Department of Mathematics, Dartmouth College, USA
   index: 6
 - name: Max Planck Institute for Intelligent Systems, Germany

date: Sometime in the future
bibliography: paper.bib

---

# Summary

The Comple**X** **G**roup **I**nteractions (XGI) library provides data structures and algorithms for modeling and analyzing complex systems with group (higher-order) interactions.

Many datasets can be represented as graphs, where pairs of entities (or nodes) are related via links (or edges). Examples are road networks, energy grids, social networks, neural networks, etc. However, in many other datasets, more than two entities can be related at a time. For example, many scientists (entities) can collaborate on a scientific article together (links), and multiple email accounts (entities) can all participate on the same email thread (links). In this latter case, graphs no longer present a viable alternative to represent such datasets. It is for this kind of datasets, where the interactions are given among groups of more than two entities (also called higher-order interactions), that XGI was designed for.

XGI is implemented in pure Python and is designed to seamlessly interoperate with the rest of the Python scientific stack (numpy, scipy, pandas, matplotlib, etc). XGI is designed and developed by network scientists with the needs of network scientists in mind.

# Statement of need

# Mathematics

Single dollars ($) are required for inline mathematics e.g. $f(x) = e^{\pi/x}$

Double dollars make self-standing equations:

$$\Theta(x) = \left\{\begin{array}{l}
0\textrm{ if } x < 0\cr
1\textrm{ else}
\end{array}\right.$$

You can also use plain \LaTeX for equations
\begin{equation}\label{eq:fourier}
\hat f(\omega) = \int_{-\infty}^{\infty} f(x) e^{i\omega x} dx
\end{equation}
and refer to \autoref{eq:fourier} from text.

# Citations

Citations to entries in paper.bib should be in
[rMarkdown](http://rmarkdown.rstudio.com/authoring_bibliographies_and_citations.html)
format.

If you want to cite a software repository URL (e.g. something on GitHub without a preferred
citation) then you can do it with the example BibTeX entry below for @fidgit.

For a quick reference, the following citation commands can be used:
- `@author:2001`  ->  "Author et al. (2001)"
- `[@author:2001]` -> "(Author et al., 2001)"
- `[@author1:2001; @author2:2001]` -> "(Author1 et al., 2001; Author2 et al., 2002)"

# Figures

Figures can be included like this:
![Caption for example figure.\label{fig:example}](figure.png)
and referenced from text using \autoref{fig:example}.

Figure sizes can be customized by adding an optional second parameter:
![Caption for example figure.](figure.png){ width=20% }

# Acknowledgements

We acknowledge contributions from Tim LaRock, Marco Nurisso, and Sabina Adhikari.

The XGI package has been supported by NSF Grant 2121905, "HNDS-I: Using Hypergraphs to Study Spreading Processes in Complex Social Networks".

# References