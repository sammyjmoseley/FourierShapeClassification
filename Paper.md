--- 
title: Fourier Analysis Algorithm to Identify Shapes
author: Sammy Moseley
date: 12/12/2016
header-includes: 
    - \usepackage{natbib}

bibliography: Bibliography.bib
--- 

# Introduction

Computers have been becoming increasingly integral to everyday life over the past half century. The advent of smart-phones has made computer technology more accessible and mobile. They have transformed the way we interact with society: making it easier to communicate with individuals, and making it easier to share and view content on social networks. Over the next couple decades, we can expect new technologies such as driver-less cars, augmented/virtual reality, and unmanned aerial vehicles (UAVs) to cause a similarly significant fundamental change to the way we go about our daily lives. 

CUAir is an interdisciplinary team at Cornell University that designs, builds, and engineers an UAV. The team competes annually at AUVSI SUAS, an international competition in Maryland. The team has a number of objectives to meet including automatic takeoff, landing, egg drop, and target recognition (TR).  At competition targets are spaced in unknown locations on a field. Targets are colored shapes with an alphanumeric-character inside the shape. The team's TR objective is two pronged, to identify manually (MTR) and automatically (ATR) the targets. Identification characteristics include color of shape and alphanumeric, shape name, alphanumeric character, GPS location, and orientation. The MTR application is a distributed system that allows people to manually identify targets. The ATR system has to automatically identify and classify the targets. Each ATR task presents specific challenges, and those presented by classifying the shape are particularly well suited for a Fourier analysis algorithm.

A Fourier analysis algorithm involves taking the unidentified data, transforming it into a waveform, and using the Fourier analysis vector to classify the shapes with a machine learning algorithm.

Our primary application for our system is search and rescue. Other applications include military, agriculture, and industry.

Two foundational fields of computer science that are integral to those new technologies are computer vision and machine learning. 
Computer vision and machine learning were both born in the 1960s with major foundational developments taking place through 1970s [@wiki_cv; @forbes_ml]. MORE ABOUT CV AND ML