---
sidebar: false
---
# Virtual screening of the SARS-CoV-2 main protease (de.NBI-cloud, STFC)

Powered by: [![usegalaxy.eu](https://img.shields.io/static/v1?label=usegalaxy&message=eu&color=green)](https://usegalaxy.eu)


[Tim Dudgeon](https://github.com/tdudgeon),
[Simon Bray](https://github.com/simonbray),
[Gianmauro Cuccuru](https://github.com/gmauro),
[Björn Grüning](https://github.com/bgruening)

This repo serves as a companion to our recent docking simulations on the SARS-CoV-2 main protease.

It contains descriptions of workflows and exact versions of all software used. The goals of this study were to:

 1. Underscore the importance of access to raw data
 2. Demonstrate that existing community efforts in curation and deployment of computational chemistry software can reliably support rapid reproducible research during global crises

------------

The [Diamond Light Source's XChem team](https://www.diamond.ac.uk/Instruments/Mx/Fragment-Screening.html) recently completed [a successful fragment screen on the SARS-CoV-2 main protease (MPro)][1], which provided 55 fragment hits . In an effort to identify candidate molecules for binding, [InformaticsMatters](http://informaticsmatters.com), the XChem group and the [European Galaxy team](https://galaxyproject.eu) have joined forces to construct and execute a Galaxy workflow for performing and evaluating molecular docking on a massive scale.

<p align="center">
  <a href="https://usegalaxy.eu/u/sbray/v/mpro-x0072"><img src="./img/mpro-x0072.png" width= "40%" alt="Mpro-x0072 complex, visualized with the NGL viewer integrated into Galaxy." /></a>
</p>

An initial list of ~42,000 candidate molecules was assembled by using the [Fragalysis fragment network] [2] to elaborate from the initial fragment hits. This was done using [Informatics Matters’ Fragnet Search APIs](https://fragnet.informaticsmatters.com/), querying a database of ~64M molecules available from [Enamine REAL](https://enamine.net/), [ChemSpace](https://chem-space.com/) and [MolPort](http://www.molport.com). These were used as inputs for the docking and scoring workflow. The workflow consists of the following steps, each of which was carried out using tools installed on the European Galaxy server:
1. [Charge enumeration](1-DockingPrep) of those 42,000 candidate molecules to generate ~159,000 docking candidates.
2. [Generation of 3D conformations](1-DockingPrep) based on SMILES strings of the candidate molecules.
3. [Preparation of active site for docking](2-ActiveSitePrep) using rDock.
3. [Docking](3-Docking) of molecules into each of the MPro binding sites using rDock, generating 25 docking poses for each molecule.
4. [Evaluation of the docking poses](4-Scoring) using a [deep learning approach] [3] developed at the University of Oxford, employing augmentation of training data with incorrectly docked ligands to prompt the model to learn from protein-ligand interactions. The algorithm was deployed on the European Galaxy server inside a Docker container, thanks to work by InformaticsMatters and the European Galaxy team.
5. [Scoring](4-Scoring) of the top scoring pose from each molecule against the original fragment screening hit ligands using the [SuCOS MAX shape and feature overlay algorithm] [4], again deployed on the European Galaxy server by InformaticsMatters and the European Galaxy team.

This workflow was repeated for each of the 17 fragment screening crystal structures that were available at the time (more are expected).
 
Of these steps, the third (docking) is the most compute-intensive. Here, the project benefited from the enormous distributed compute capacity which underlies the European Galaxy project. Over 5000 CPUs were made available, provided by [Diamond’s STFC-IRIS](https://www.diamond.ac.uk) cluster at Harwell, UK and the [de.NBI cloud](https://www.denbi.de) in Freiburg, Germany. With each docking job requiring 1 CPU, thousands of poses could thus be docked in parallel, allowing millions of poses to be docked over a single weekend. The fourth step (pose scoring), while less computationally expensive, was accelerated thanks to GPUs provided by de.NBI and STFC. In total, the entire workflow described here took around 120,000 hours of CPU time (13 years) to complete.

All data is publicly available via https://usegalaxy.eu, together with the workflows used for data generation, and we are working to provide more detailed documentation that will allow other users to perform similar studies, including on other systems. Histories for each fragment structures are provided [here](Histories).

Having identified promising candidate ligands, we are now looking for funding to purchase compounds as a basis for further experimental study.


[1]: https://www.diamond.ac.uk/covid-19/for-scientists/Main-protease-structure-and-XChem.html "Diamond Light Source, press release."

[2]: https://diamondlightsource.atlassian.net/wiki/spaces/FRAG/pages/8323192/The+Astex+Fragment+network  "Skyner, The (Astex) Fragment network, Diamond Public Confluence."

[3]: https://www.biorxiv.org/content/10.1101/2020.03.06.979625v1 "Scantlebury et al., Dataset Augmentation Allows Deep Learning-Based Virtual Screening To Better Generalise To Unseen Target Classes, And Highlight Important Binding Interactions."

[4]: https://dx.doi.org/10.26434/chemrxiv.8100203.v1 "Leung et al., SuCOS is Better than RMSD for Evaluating Fragment Elaboration and Docking Poses"

In addition we will be looking at newly released data here &#8594; [Updates: Analysis of additional data](Histories)

The experiments have been performed using the [Galaxy](http://galaxyproject.org) platform and open source tools from [BioConda](https://bioconda.github.io/) and [conda-forge](https://conda-forge.org/). Tools were run using cloud resources provided by [de.NBI](https://www.denbi.de/) and [STFC](https://stfc.ukri.org/).


 <p align="center">
  <a href="https://galaxyproject.org">   <img src="./img/galaxy_logo.png" width= "22%" alt="Galaxy Project" /></a> &nbsp;
  <a href="https://galaxyproject.eu">    <img src="https://raw.githubusercontent.com/usegalaxy-eu/branding/master/galaxy-eu/galaxy-eu.256.png" width= "20%" alt="European Galaxy Project" /></a> &nbsp;
  <a href="https://bioconda.org">        <img src="./img/bioconda_logo.png" width="20%" alt="bioconda" /></a> &nbsp;
  <a href="https://www.informaticsmatters.com/"><img src="./img/informatcsmatters_logo.svg" width="20%" alt="informaticsmatters" /></a> &nbsp;
  <a href="https://www.diamond.ac.uk/Instruments/Mx/Fragment-Screening.html"> <img src="./img/xchem_logo.png" width="18%" alt="xchem" /></a> &nbsp;
  <a href="https://www.denbi.de">        <img src="./img/denbi-logo-color.svg" width="20%" alt="de.NBI" /></a> &nbsp;
  <a href="https://www.ukri.org">        <img src="./img/UKRI_STF_Council-Logo_Horiz-RGB.png" width="23%" alt="STFC" /></a> &nbsp;
  <a href="https://elixir-europe.org">   <img src="./img/elixir_logo.png" width="15%" alt="ELIXIR" /></a> &nbsp;
  <a href="https://training.galaxyproject.org"> <img src="./img/gtn_logo.png" width="20%" alt="Galaxy Training Network" /></a> &nbsp;
  <a href="https://www.eosc-life.eu">          <img src="./img/eosclife.png" width="10%" alt="EOSC-Life" /></a> &nbsp;
  </p>
