---
layout: post
title: Research Update - Week 17
date: 2024-01-21 17:04 -0500

categories:
  - Research Updates
tags:
  - YSOLab
  - Cosmology Group
  - Gun Violence Archive
  - Galaxy Cluster Pod

---
    Much of what happened during the different meetings this week was more focused on defining plans for the next semester. As a result, I don't have much other than plans for next week to show.



## YSOLab

### Future Work

#### Comments on HOPS388 Research Note

    The HOPS388 Research Note is very nearly complete, and the plan is to have everyone read through and provide comments by next thursday, such that we can go through and apply any suggestions during the meeting on Friday.



#### Improve unTimely Saturation Designations

    One effect of integrating the saturation flag with the unTimely data is that something approaching 70% of the sources become fully saturated, with none of the unTimely data being valid for our use. There are still a number left, but we hope to investigate the exact definition of saturation, and maybe widen the acceptable range just a little, to allow more sources through the filter.

    For next week, this amounts to an investigation of the relationship between the magnitude in unTimely and in the Spitzer data, with an eye towards how that relationship changes as the magnitude decreases.



#### Swap to 2MASS-Style Names

    We have been discussing this change for a while, but we have finally decided to swap from naming our sources arbitrarily based on list order to naming them based on position. Specifically, we hope to emulate the same basic style as the 2MASS catalog.

    In addition, I will likely take this opportunity to refresh the NEOWISE data, which has likely had an additional data-release since I last downloaded it.



#### Organize Drive

    One consistent hassle to deal with has been our drive organization. We have a shared google drive folder that we use to house finished plots, tables, and other files related to the project. There has not been a particularly consistent structure to this document, which has resulted in multiple, incomplete versions of the data existing in different subfolders at different times, and a whole lot of confusion.

    To address this, I plan on cordoning off the older products inside a single labelled folder, and creating a central location with subfolders for each region, and further subfolders for each kind of product. Hopefully, with some encouragement, we can keep the drive relatively organized, and avoid any further confusion.



#### Integrate Serpens & Perseus Spitzer Data

    We recently recieved another set of Spitzer data, this time for both the Serpens and Perseus region. I plan on using my existing code to extract the measurements that are within the given IDL files and integrate them into the existing code base, which should allow us to being plotting the Serpens and Perseus sources.



#### Add Saturation to Tables

    One additional detail I plan to address was to quickly add the saturation flags for unTimely to the output FITS files I am generating, to make it possible to leverage this information further down the pipeline.



## Cosmology Research Group

### Future Work

    Our focus this semester is on wrapping up this project, with the hope that we can finish and submit a paper describing our results by the middle of the summer.



#### Put Together Final Plots

    In preparation for writing the paper, I will need to assemble the final plots that we will include. Currently, we plan to include the halo ionization plot, the bubble volume distribution plot, and several different halo slice plots.



#### Tune Each Model

    In service of putting together a final set of plots, I plan to work next week on tuning the escape fraction for each model of interest individually, such that each matches the renaissance model. 'Matching', here, is defined using the median sampled ionization fraction ratio, a method that I have described previously.



## Gun Violence Archive Analysis

### Future Work

    My hope is that, before I meet with Dr. Lilley, I can knuckle down and finally load in the BEA data, which I have been putting off for a while now. The primary reason is mostly that it is going to be fairly tedious.



## Cluster Research Pod

### Future Work

#### Plot SEDs

    For next week, I plan to plot a set of SEDs for a few chosen clusters, using both the existing HST catalog, as well as the preliminary photometry from the JWST image. The hope is that doing this will help us better understand if our photometry is functioning correctly.


