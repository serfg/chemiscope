---
title: 'Chemiscope: interactive structure-property explorer for materials and molecules'
tags:
  - TypeScript
  - JavaScript
  - chemistry
  - material science
  - machine learning
authors:
  - name: Guillaume Fraux
    orcid: 0000-0003-4824-6512
    affiliation: 1
  - name: Michele Ceriotti
    orcid: 0000-0003-2571-2832
    affiliation: 1
affiliations:
 - name: Laboratory of Computational Science and Modeling, IMX, École Polytechnique Fédérale de Lausanne, 1015 Lausanne, Switzerland
   index: 1
date: 30 January 2020
bibliography: paper.bib
---

# Summary

The number of different materials or molecules that can created by combining
different chemical elements in various proportions and spatial arrangements is
enormous. We can use computational chemistry to generate databases containing
billions of potential structures [@Ruddigkeit2012], and predict some of the
associated properties [@Montavon2013; @Ramakrishnan2014]. Unfortunately, the
very large number of structures makes exploring such database — to understand
structure-property relations or find the *best* structure for a given
application — a daunting task. In the recent years, multiple molecular
*descriptors* [@Behler2007; @Bartok2013; @Willatt2019] have been developed to
compute structural similarities between materials or molecules, incorporating
physically-relevant information and symmetries. These descriptors can be used
for unsupervised machine learning applications, such as clustering or
classification of the different structures, and high-throughput screening of
database for specific properties [@Maier2007; @De2017; @Hautier2019].
Unfortunately, the dimensionality of most descriptors is very high, which makes
the resulting classifications, clustering or mapping very hard to visualize.
Additional dimensionality reduction algorithm [@Schlkopf1998; @Ceriotti2011;
@McInnes2018] can reduce the number of relevant dimensions to a handful,
creating 2D or 3D maps of the full database.

![The Qm7b database [@Montavon2013] visualized with chemiscope](screenshot.png)

Chemiscope is an graphical tool for the interactive exploration of materials and
molecular databases, correlating local and global structural descriptors with
the physical properties of the different systems. The interface is composed by
two panels. The left panel consists of a 2D or 3D scatter plot, in which each
point corresponds to a chemical entity. The axes, color, style of each point can
be set to represent a property or a structural descriptor, so that the map makes
it possible to visualize the structure-property relations. Structural
descriptors are not computed directly by chemiscope, but must be obtained from
one of the many codes implementing such descriptors [@librascal; @QUIP].
Since many of the most common descriptors can be very high dimensional, it can
also be convenient to apply a dimensionality reduction algorithm that maps them
to a lower-dimensional space. For example the sketchmap algorithm
[@Ceriotti2011] was used with the Smooth Overlap of Atomic Positions descriptor
[@Bartok2013] to generate the visualization above. The right panel consists of a
visualization of the three-dimensional structure of the chemical entities,
including possibly the periodic repeat unit, which can be non orthorhombic, and
of a slider that runs through the sequence of entities in the database.
Visualizing the chemical structure can help finding an intuitive rationalization
of the layout of the dataset and the structure-property relations.

Whereas similar tools [@Gong2013; @Gutlein2014; @Probst2017; @ISV] only allow
visualizing maps and structures in which each data point corresponds to a
molecule, or a crystal  structure, a distinctive feature of chemiscope is the
possibility of visualizing maps in which points correspond to atom-centred
environments. This is useful, for instance, to rationalize the relationship
between structure and atomic properties such as nuclear chemical shieldings
(Figure 2). This is also useful as a diagnostic tool for the many
machine-learning schemes that decompose properties into atom-centred
contributions [@Behler2007, @Bartok2010]


![Database of chemical shielding [@Paruzzo2018] in chemiscope showing the use of a 3D plot and atomic environments highlighting](./screenshot-3d.png)

Chemiscope took strong inspiration from a previous similar graphical software,
the interactive sketchmap visualizer [@ISV]. This previous software was used in
multiple research publication, related to the exploration of large-scale
databases, and the mapping of structure-property relationships [@De2016;
@De2017; @Musil2018].

# Implementation

Chemiscope is implemented using the web platform: HTML5, CSS and WebGL to
display graphical elements, and TypeScript (compiled to JavaScript) for the
interactivity. It uses [Plotly.js](https://plot.ly/javascript/) to render and
animate 2D and 3D plots; and the JavaScript version of [Jmol](http://jmol.org/)
to display atomic structures. The visualization is fast enough to be used with
datasets containing up to a million points, reacting to user input within a few
hundred milliseconds in the default 2D mode. More elaborate visualization are
slower, while still handling 100k points easily.

The web platform makes chemiscope usable from different operating systems
without the need to develop and maintain separate ports. It also makes it easy
to provide an online service that allows users without installation to visualize
their own dataset. One instance of this online service is currently available at
http://chemiscope.org. Chemiscope is implemented as library of re-usable
components linked together via callbacks. This makes it easy to modify the
default interface to generate mode elaborate visualizations: it would be simple,
for example, to re-use the molecular viewer to display multiple maps generated
with different parameters of a dimensionality reduction algorithm. Chemiscope
can also be distributed in a standalone mode, where the code and a predefined
dataset are merged together as a single HTML file. This standalone mode is
useful for archival purposes, for example in as supplementary information for a
published article; and for use in corporate environments with sensitive
datasets.

# Acknowledgements

The development of chemiscope have been funded by the [NCCR
MARVEL](http://nccr-marvel.ch/) and the [MAX](http://max-centre.eu/) European
centre of excellence.

# References