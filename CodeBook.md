---
title: "run_analysis"
author: "Pat Murphy"
date: "Sunday, December 21, 2014"
output: html_document
---

Data to be analyzed was initially split into two frames, but needed to be analyzed as one entity. Read the data into R and combined them in lines 9-11. 
Meta data pertaining to the activities, subjects and features being tested was split up in the same way, so those tables were read into R and combined in the same way in lines 12-19.
In line 20 the columns of the matrix were named.
In line 21 a logical vector was created determining whether the data was measured on the mean or the standard deviation for each measurement or not. 
In line 22 that logical vector was used to extract the columns which were measured on either the mean or the standard deviation.
lines 23-31 uniquely ordered the subjects and activities (activities by number) and created two vectors. One was each subject repeated for however many activities, and the other activities repeated for however many subjects.
Line 32 created the matrix tidy_matrix with the proper dimensions which would then be filled by the for loop in lines 33-35 which called the function extractor written in lines 3-7.
Next the columns and rows were labeled matching up the proper subjects and activities with the data and the table was written and saved as a .txt file.
