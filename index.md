---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---

The vised_marks extension for EEGLAB adds editing functions to the native eegplot data scrolling figure. Specifically, it allows adding/editing event markers, flagging channels/components, flagging time periods and displaying the properties of the marks structure. This extension provides an interface and "marks" the data structure, allowing for the flagging of channels/components and time periods. This extension also provides tools for epoching the data for artefact detection then concatenating the data back into a continuous form while storing the rejection information in the "marks" structure.

### Getting Started

The vised marks extension is a multipurpose tool that has many useful features for visualising and editing EEG data. Its functions can be summarised into three main categories:  

1. Manually edit the data by interacting with the scroll data and placing marks manually.  
2. Editing the data by running scripts that mark the data during times of specified criteria.  
3. Epoching and concatenating the data using the marks structure in order to apply effects specific to either data type.  

Vised marks is designed based off a **marks** structure that latches onto the time indices of the data. This allows it to remember particular status (usually 0 or 1) of a mark at each time point, as well as accurately remove, cut and merge data time.  Once the data is marked with your custom process, you can manipulate those points as you wish. Typically marked sections of the data are either removed or saved for the input of other functions.

### Set Up Figure Renderer

Depending on your operating system and the default setting you are using you may need to type the following code into your MATLAB command window before each session. It mostly appears to be required when running MATLAB versions earlier than 2014b. This will ensure that the plots will open correctly, and you can avoid this error.

![Java Error]({{ page.root }}/fig/javaerror.png "Java Error")


```matlab
>> set(0,'DefaultFigureRenderer','OpenGL');
```

> ## Prerequisites
> - All previous BIDS and BIDS EEG lessons.  
> - MATLAB  
> - EEGLAB  
{: .prereq}

{% include links.md %}
