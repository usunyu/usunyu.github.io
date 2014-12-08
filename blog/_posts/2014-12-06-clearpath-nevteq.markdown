---
layout: post
title:  "ClearPath Navteq"
date:   2014-12-06 17:57:05
categories: jekyll update
---
This is the pattern I created from the data of sensors I matched, I filled the missing data from near related sensor or history data. 

![Pattern](/images/clearpath_pattern.jpg)

* Blue:		speed < 10
* Red:		speed >= 10 && speed < 25
* Yellow:	speed >= 25 && speed < 50
* Green:	speed >= 50

#### **Manual:**
1. CA GNDEMO Process (In PatternGeneration Project):
* input/CAInputFileGeneration
* (Optional) output/CAOutputKMLGeneration to generate kml
* pattern/CAAdjListPattern

2. Clean Data Process (In PatternGeneration Project):
* input/InputFileGeneration

			writeAverageCube
			readAverageCube(i, 0);
			changeInterval();
			writeAverage15Cube(i, 0);
			renameAverageFile(i, 0);
* process/DataClean
* output/OutputDatabaseGeneration

3. AdjList Create Process (In ArterialPatternGeneration_shireesh Project):
* GetAverageSpeedForArterials1to5New
* GetAverageSpeedForHighways
* CreateListForArterials1to5New
* CreateListForHighways1to5New
*  CreateAdjList1to5New

#### **GitHub:**
* [Repositories](https://github.com/usunyu/ClearPath/tree/master/Nevteq)
