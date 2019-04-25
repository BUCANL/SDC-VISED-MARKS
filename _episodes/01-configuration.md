---
title: "Configuration"
teaching: 10
exercises: 0
questions:
- "How do we set up and configure the vised_marks plugin?"
objectives:
- "To configure vised marks which will enable editing the marks in a scroll plot."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

The vised configuration editor allows users to customise their vised plot behaviour. The user interface produces a configuration file that can be loaded as the default for future projects.

**Note:** Saving the vised configuration after personalising settings can save a lot of time!

![Manual Scroll Plot Options]({{ site.root }}/fig/manscrollplotoptionspic.png "Manual Scroll Plot Options")

This page outlines accessing the configuration interface and explaining each of the elements you can modify. In EEGLAB go to **File->Vised Configuration**.

![Find Vised Config]({{ site.root }}/fig/findvisedconfig.png "Find Vised Config")

If you already have a configuration file you can load it by clicking Load Vised Config and browsing to the file location.

![Vised config]({{ site.root }}/fig/visedconfig.png "Vised Config")

## Visual Editing Options

The first group of configuration options deal with assigning hotkeys to different plotting functions. These will allow you to quickly add visual marks to the the data scroll plot when using the Editing Marks Visually method.

#### Quick Event Create

String event type to immediately add (without a pop up window) when the alternate press (**[Ctrl + left-click]** or **[right-click]**) is executed on the eegplot data axis. This option overwrites any other specification for altselectcommand at run time (Default = '' = no quick event).

#### Quick Event Remove

- Enable single click event removal (no pop up GUI).
    - **"ext_press"** = remove event when **[Shift + left-click]** is executed on events in the eegplot figure axis.
    - **"alt_press"** = remove event when **[Ctrl + left-click]** or **[right-click]** is executed on events in the eegplot figure axis.
- When set to **"alt_press"** this option will overwrite any other specification for altselectcommand at run time.
- When set to **"ext_select"** this option will overwrite any other specification for extselectcommand (including quick_evtmk) at run time.

#### Quick Channel Flagging

- Enable single click channel flag toggle (no pop up GUI).
    - **"ext_press"** = toggle channel flag when **[Shift + left-click]** is executed on event in eegplot figure axis.
    - **"alt_press"** = toggle channel flag when **[Ctrl + left-click]** or  **[right-click]** is executed on event in eegplot figure axis

- Select Command **[selectcommand]**
    - **[cell array]** list of 3 commands (strings) to run when **[left-click]** is pressed, when it is moving, and when it is up.

- Extended Select Command **[extselectcommand]**
    - **[cell array]** list of 3 commands (strings) to run when **[Ctrl + Shift]** is pressed, when it is moving, and when it is up.

- Alternate Select Command **[altselectcommand]**
    - **[cell array]** list of 3 commands (strings) to run when **[Ctrl + left-click]** or **[right-click]** is pressed, when it is moving, and when it is up.

All of the select commands above use cell arrays to capture the field data. Make sure you use the following structure, with each function ending with a semicolon and a new line at the end:

`function_called('key_action1','key_value1'...'key_action2','key_value2'...);`

![Cell Array Functions Only]({{ page.root }}/fig/cellarrayjustfunc.png "Cell Array - Functions Only")

- Key Select Command **[keyselectcommand]**
    - **[cell array]** each row is string containing a key character followed by "," then a command to execute when the key character is pressed while the pointer is over the data axis.
    - This select command uses a cell array to capture the field data. Make sure you use the following structure, with each [key press,function] combination on a separate line:
     
`key_press,function_called('key_action1','key_value1'...'key_action2','key_value2'...)`

First is the keyboard key that you are going to assign a certain action to, followed by a comma and the function that will be called with this key press. Typically this will be `ve_eegplot()` or `ve_edit()`. The inputs to these functions are explained in more detail in the [Edit Data Visually](https://bucanl.github.io/SDC-VISED-MARKS/02-visualedit/index.html) episode.

![Key String Command]({{ page.root }}/fig/keystringcommand.png "Key String Command")

## EEG Plot Options

The second group of configuration options deal with creating a default setting for the scroll data pop up. This allows you to control how large the plot is, the window time length, the number of channels to display, and more.

- Keep figure at front **[mouse_data_front]**
    - **['on' 'off']** When mouse moves over the data axis bring/keep the eegplot figure window at the front (default = 'on')

- Sampling Rate **[srate]**
    - Sampling rate in Hz (default = 256Hz). Use this in the calculation of times labels on the x axis of the eegplot scroll window.

- Y-axis spacing **[spacing]**
    - Display range per channel (default = 0:max(whole_data)-min(whole_data)). this is the y-axis distance in uV between the zero value of each waveform in the eegplot scroll window.

- Time Limits **[limits]**
    - **[start end]** Time limits for data epochs in ms (for labelling purposes only).

- Window Time Length **[winlength]**
    - **[value]** Seconds (or epochs) of data to display in window (default = 5).

- n Channels to Display **[dispchans]**
    - **[integer]** Number of channels to display in the activity window (default: from data). If < total number of channels, a vertical slider on the left side of the figure allows vertical data scrolling.

- Title
    - Figure title (default = none).

- X-Axis Grid Lines **[xgrid]**
    - **['on' 'off']** Toggle display of the x-axis grid (default = 'off')

- Y-Axis Grid Lines **[ygrid]**
    - **['on' 'off']** Toggle display of the y-axis grid (default = 'off')

- Plot Event Duration **[ploteventdur]**
    - **['on' 'off']** Toggle display of event duration (default = 'off')

- Overlay Data **[data2]**
    - **[float array]** identical size to the original data and plotted on top of it.
    - This is particularly useful when comparing an artefact corrected dataset with its predecessor. You can see both side by side and determine what parts you should keep from each.

- Button Command **[command]**
    - **['string']** Matlab command to evaluate when the 'REJECT' button is clicked. The 'REJECT' button is visible only if this parameter is not empty. As explained in the "Output" section below, the variable TMPREJ' contains the rejected windows (see the functions `eegplot2event()` and `eegplot2trial()`).

- Button Label **[butlabel]**
    - Reject button label. (default = 'REJECT')

- Channel Color **[color]**
    - **['on' 'off' {cell array}]** Plot channels with different colors. If an RGB cell array {'r' 'g' 'b'}, channels will be plotted using the cell-array color elements in cyclic order (default = 'off').

- Axis Marking Color **[wincolor]**
    - **[color]** Color to use to mark data stretches or epochs (default = [0.7 1 0.9] is the default marking color).

- Subtract Signal Mean **[submean]**
    - **['on' 'off']** Remove channel means in each window (default = 'on').

- Figure Window **[position]**
    - **[lowleft_x lowleft_y width height]** Position of the figure in pixels.

- Figure Window **[tag]**
    - **[string]** Matlab object tag to identify this `eegplot()` window (allows keeping track of several simultaneous `eegplot()` windows).

- Figure **[children]** Handle
    - **[integer]** Figure handle of a dependent `eegplot()` window. Scrolling horizontally in the master window will produce the same scroll in the dependent window. Allows comparison of two concurrent datasets, or of channel and component data from the same dataset.

- Amplitude **[scale]**
    - **['on' 'off']** Display the amplitude scale (default = 'on').

## Marks Property Options

This last section of the configuration deals with adjusting how your marks will appear visually on the scroll data. This includes transparency and spacing.

- Marks Y-axis Location **[marks_y_loc]**
    - Location along the y axis (percent from bottom to top) to display the marks structure flags (default = .8). May also be an array of values to plot.

- Inter-Mark Interval **[inter_mark_int]**
    - Distance along the y axis (percent from bottom to top) to separate each marks type (default = .04).

- Inter_tag Interval **[inter_tag_int]**
    - Distance along the x-axis (percent from left to right) to separate each channel tag pointing at flagged channel labels (default = .002).

- Color Interval **[marks_col_int]**
    - Marks surface plots depict values between 0 to 1. The **[marks_col_int]** sets the interval of color change in the plot (default = .1).

- Marks Surface Plot Transparency **[marks_col_alpha]**
    - Alpha is a value between 0 and 1 where 0 = transparent and 1 = opaque (default = .7).


{% include links.md %}

