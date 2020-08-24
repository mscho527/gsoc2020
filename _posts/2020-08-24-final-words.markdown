---
layout: post
title: "Final Reflections"
date: 2020-08-24 13:00:00 +0900
categories: cclib
---

*This post marks the end of my daily posts for GSoC 2020 and is intended to serve as the 'Work Product Submission' task for the program.*

During the last three months (or approximately sixty business days), I was able to experience a well-guided introduction to opensource software development in sciences while staying highly relevant to my academic objective as a chemical physics concentrator. It has been very fortunate for me to work with my mentors in [cclib](https://cclib.github.io/) -- Dr. Eric Berquist, Professor Geoff Hutchison, Dr. Karol Langner, and Shiv Upadhyay *(in alphabetical order)* -- especially because the project was very well-timed for me. After working on the benchmarks for different atomic charge partitioning methods last summer at the Weizmann Institute of Science in Rehovot, Israel, I've wanted to work on actually implementing such methods, preferably in some modern language. Project in cclib has exactly been what I have envisioned.

As suggested earlier in my proposal to the program, my contributions to `cclib` are largely in four topics: *Bickelhaupt Charges, Hirshfeld Charges & Bridge to* `horton`*, Bader's QTAIM Charges, and DDEC6 Charges*. Link to every PRs that I submitted can be found in the Github Projects [page](https://github.com/orgs/cclib/projects/2#card-40460776) that was used to keep track of the PRs. Each PR should be self-explanatory, but I will discuss each in more depth below. The diagram below summarizes my workflow for the last three months.

![GSoC Workflow]({{ site.url }}/assets/gsocdiagram.png)

## Logistics

Throughout the summer, my mentors and I had daily Zoom sessions for as short as five minutes to a maximum of half an hour. Those sessions helped me interact with my mentors more effectively, kept the progress roughly on schedule, and allowed me to understand my mentors' comments and guidance towards the world of opensource development easily. Due to the large time zone difference between us (I've been in Seoul -- GMT+9 and my mentors were in EDT and PDT), we had the daily meetings in the nighttime here, which is early morning in EDT and PDT. I'd like to express gratitude here to my mentors for holding the sessions so early to find a time that works and being so helpful throughout the summer.

## Background

*This part was adapted from my proposal to GSoC.*

The usual approach taken in computational chemistry is to first run a simulation, which can often take a long time, on a target molecule, and then to analyze the results to obtain an insight about the molecule. Usually, electron density assigned in the atoms that constitute the molecule is a key element in this process. This partitioning of the electrons itself does not suggest any observable properties of a molecule. However, many observable features of a molecule are derived directly from the electron densities, hence motivating chemists to relentlessly improve this noumena step by step.

Currently, cclib can calculate some common atomic charge potential methods, including C-squared Population Analysis, Mulliken Population Analysis, and Lowdin Population Analysis. Because Mulliken Population Analysis and Lowdin Population Analysis are similar in its theory (Lowdin uses a different basis set to reduce the basis set dependence observed in Mulliken Analysis), it would be advantageous to have other calculation methods implemented in cclib. Some examples of the methods that can be implemented include DDEC6, Hirshfeld method, Bader analysis, and so on.

## Subtopics

#### Bickelhaupt

> [PR.869](https://github.com/cclib/cclib/pull/869) Feat: Bickelhaupt Charges

This PR/topic was a very simple extension from the Mulliken Population Analysis that already existed in `cclib` and was intended to help me get used to the tools and structure of the project.

#### Bridge to Horton

> [Issue 880](https://github.com/cclib/cclib/issues/880) Bridge between cclib and horton for Hirshfeld-related charges calculation

> [PR.881](https://github.com/cclib/cclib/pull/881) Add horton into Travis configuration

> [PR.882](https://github.com/cclib/cclib/pull/882) Add stencil code that detects horton version

> [PR.873](https://github.com/cclib/cclib/pull/873) Add horton 2 --> cclib bridge, along with tests

> [PR.883](https://github.com/cclib/cclib/pull/883) Add cclib --> horton 2 bridge, along with tests

> [PR.884](https://github.com/cclib/cclib/pull/884) Add horton 3 --> cclib bridge, along with tests

> [PR.879](https://github.com/cclib/cclib/pull/879) Add cclib --> horton 3 bridge, along with tests

> [PR.888](https://github.com/cclib/cclib/pull/888) Write documentation for the bridge and how the bridge can be utilized

Based on my mentors' suggestion to add a bridge to `horton`, which has the Hirshfeld method and few other Stockholder-type methods, rather than writing the Hirshfeld method from scratch.

While working on the bridge, I was advised to break down large PRs into smaller and intentional changes. This was something I hadn't thought about before because I have only used `git` for personal projects. Making smaller PRs and adding tests were awkward at first, but I was able to get used to it and see how helpful they are for code review.

#### Bader's QTAIM

> [Issue 886](https://github.com/cclib/cclib/issues/886) Add method to calculate Bader's QTAIM charges

> [PR.893](https://github.com/cclib/cclib/pull/893)  Add pyquante2 to Travis configuration

> [PR.894](https://github.com/cclib/cclib/pull/894) Update pyquante bridge for pyquante2 support

> [PR.896](https://github.com/cclib/cclib/pull/896)  Update volume method for updated cclib2pyquante bridge

> [PR.897](https://github.com/cclib/cclib/pull/897)  Add pyquante2 routines in volume method

> [PR.898](https://github.com/cclib/cclib/pull/898)  \<Bug Fix for existing code\> Fix formula that determine number of grid points in volume function

> [PR.899](https://github.com/cclib/cclib/pull/899)  Update test routine for volume method to include verify data from each point on a grid 

> [PR.900](https://github.com/cclib/cclib/pull/900)  Add QTAIM method

> [PR.907](https://github.com/cclib/cclib/pull/907)  Add function to read cube files and generate Volume object; Add the ability to use precalculated charge densities in Bader method

> [PR.902](https://github.com/cclib/cclib/pull/902)  Add a few more tests for Bader's QTAIM method

> [PR.903](https://github.com/cclib/cclib/pull/903)  Add docs for QTAIM method

Compared to the previous two topics, Bader's QTAIM was more involved and required me to modify the existing code as well as add some new code. The `Volume` object, which holds the charge densities on a grid, was extended further so that it reads cube files. Also, an existing bridge to `PyQuante` was extended by adding `pyquante2` support. This was necessary because the old `PyQuante` did not officially support `Python 3.x.x`. Since `cclib` targets both `Python 2.x.x` and `Python 3.x.x` for now (although Python 2 support will be dropped in a foreseeable future), `pyquante2` support was a necessity.

The implementation of Bader's QTAIM charges was based on the numerical method suggested by Henkelman group. The scheme allows a relatively robust partition of three-dimensional space to individual Bader spaces, each of which belongs to the atoms that constitute a molecule.

#### DDEC6

> [Issue 885](https://github.com/cclib/cclib/issues/885) Add method to calculate DDEC6 charges

> [PR.914](https://github.com/cclib/cclib/pull/914)  Add a function to read in proatom densities from chargemol's  `atomic_densities`

> [PR.917](https://github.com/cclib/cclib/pull/917)  Add optional weight argument for integrate method in volume 

> [PR.915](https://github.com/cclib/cclib/pull/915)  Set up the grid and calculate localized charge and stockholder-type charge for DDEC6 [Step 1 of algorithm]

> [PR.916](https://github.com/cclib/cclib/pull/916)  Calculate reference ion charge value [Step 2 of algorithm]

> [PR.919](https://github.com/cclib/cclib/pull/919)  Add a function that conditions reference density [Step 3 of algorithm]

> [PR.920](https://github.com/cclib/cclib/pull/920)  Calculate weights assigned to each grid point in DDEC6 [Step 4-7 of algorithm -- substeps 1~4]

> [PR.921](https://github.com/cclib/cclib/pull/921)  Add function that evaluates G_A(r_A) and H_A(r_A) for DDEC6 [Step 4-7 of algorithm -- substeps 5~7]

> [PR.922](https://github.com/cclib/cclib/pull/922)  Add more tests for DDEC6

> [PR.926](https://github.com/cclib/cclib/pull/926)  Add docs for DDEC6

> [PR.927](https://github.com/cclib/cclib/pull/927)  Separate out Hirshfeld method from DDEC6 as a standalone method

> [PR.928](https://github.com/cclib/cclib/pull/928)  Add docs for Hirshfeld

> [PR.930](https://github.com/cclib/cclib/pull/930)  Clean up DDEC6

DDEC6 method was the most complicated method among the ones I've implemented over this summer. It uses multiple numerical integrations and spherical averaging. My initial contributions, which followed the paper equation-by-equation was not exploiting enough optimized functions. Shiv and Dr. Berquist have helped me with cleaning up the function and made sure that the routine is optimized. Much usable state had been achieved thanks to my mentors' support and fixes on my initial attempt at implementing DDEC6.

For DDEC6 code, it is quite remarkable that this seems to be the first implementation of the method other than the team that devised this method. `chargemol`, which was written by Manz group, is written in Fortran so this code will provide an alternative way of calculating DDEC6 charges using a much modern language as well as improved interoperability with the packages that `cclib` has bridges or IO functionalities.

After completing the DDEC6 implementation, noting on the fact that step 1 of the algorithm calculates Hirshfeld charges in the process, I have separated out Hirshfeld charges into a standalone method in `cclib`. This would give users two possible choices when calculating Hirshfeld charges -- use `horton` via cclib2horton bridge or calculate the charges using `cclib` natively.

## Closing Words

The sixty days of participating in GSoC has been a wonderful and timely experience for me to extend my capacity as an aspiring computational chemist. I am glad that I was allowed to take another big step into the opensource community after participating in Google Code-In in my high school years. Especially during a time where many people are going through very insecure environment due to the global pandemic, I am grateful that I was able to continue to pursue my interest with necessary guidance. Knowing at this point that I was able complete all the issues I have proposed to GSoC adds some sense of accomplishment for me as well. I hope to continue the work I've been doing for `cclib` by trying to add more code/docs for the project throughout my academic career.

