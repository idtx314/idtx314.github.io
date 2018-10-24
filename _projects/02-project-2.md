---
layout: project
title: Starbax Program for Baxter
date: March 23, 2018
image: baxter.jpg
permalink: "project-2.html"
---

## Overview

|Components                     ||Concepts|
|:------------------------------||:----------------------|
|Rethink Robotics Baxter robot  || ROS                   |
|Keurig Coffeemaker             || Python                |
|Assorted cups and k-cups       || Trajectory Planning   |
|                               || Group coordination    |



<!--
Todo:
    Add image:
    Get this project working on my station and collect images
    Get video of my section of the project working. I know can use and rely on it.  
    Add details section?
-->


## Project Overview
This program was proposed and designed as a group project with 3 other students. Our design goal was have a Baxter robot operate a keurig coffeemaker and produce a cup of coffee, using visual processing to identify the position and orientation of items in the workspace. This would allow the system to work without rigidly defining the position of equipment and supplies.

To facilitate our 2-week timeframe, we split the project into segments and each worked on a subset of the needed programming. I handled the code for manipulating the keurig lid and performing the start button press, and took lead on coordinating the final integration when I saw signs that it would be an issue.

![Chassis](../public/images/baxter_open.gif){: height="415px" width="300px"}

The git repository for this project is available [here](https://github.com/Laurenhut/ME495-final-project).

## Implementation Summary
The Baxter robot is a common research and academic platform produced by Rethink robotics that uses the highly modular Robot Operating System (ROS). To interact with Baxter we ran our own ROS code on a laptop and connected through SSH.
In order to facilitate integration of my code into the initially loosely defined structure of our overall progect I elected to create services for each of the actions I needed Baxter to perform. These services could then be called by my groupmates' code at the appropriate points in the program without any major modification being necessary to either set of code.
In order to achieve what I needed to do with Baxter's end effector, I selected an appropriate grip width for the keurig gripper and verified that the fingers were both thin enough to press the button and far enough apart that the second finger would not encounter an obstacle during the press service.

![Chassis](../public/images/baxter_press.gif)

I elected to use the inverse kinematic solver built into Baxter to perform all required movements for my task set. IK is notoriously unreliable for moving any distance in a controlled fashion, but by using set start positions for all three actions I was able to keep the commanded motions short enough that this unpredictability was not an issue.

![Chassis](../public/images/baxter_close.gif){: height="297px" width="300px"}

In order for the services to work regardless of where the keurig was in the workspace, all movement commands were translated from desired positions in the keurig's transformation frame relative to Baxter, with the keurig's pose being passed in as part of the service call. However, I found that in practice Baxter's work space was too small to accomodate some orientations. Since this was a physical limitation resulting from Baxter's dynamics I resolved the issue by creating a requirement that wherever it was on the table, the Keurig had to face Baxter.