---
title: "Edit Marks Visually"
teaching: 20
exercises: 0
questions:
- "How do we edit marks visually?"
objectives:
- "Learn how to create and purge marks visually."
keypoints:
- "FIXME"
---

## Getting Started

Editing data using this method is a manual way for you to inspect the scroll data and at the same time mark it up into different components. This method can be used before and after editing the data using scripts, depending on what aspects of the data you are looking at. Marks can help you visually interpret the data and will also save the marks selections on the data that you have made. This information can be used later to isolate or chop up the data into the different selections.

![Vised Scroll Plot]({{ site.root }}/fig/visedmanscrollplot.png "Vised Scroll Plot")

In order to start directly editing and placing marks, load the scroll data by navigating to **Edit->Visually Edit in Scroll Plot**.

![Edit Vised Drop Down Menu]({{ site.root }}/fig/editvised.png "Edit Vised Drop Down Menu")

The first pop up that shows up should look similar to the figure below. 

![Edit Vised]({{ site.root }}/fig/viseditscrollplot.png "Edit Vised")

If you haven't already, go check out the Configuration episode to find out what each of these interface fields are. If you have already created or loaded a configuration file then most of these fields will already be complete.

The most important field for this manual editing method is the key select command **[keyselectcommand]** as it is where you can define each of your editing key strokes. You will be creating a cell array of strings that should follow the following pattern, with one [key press/function] on each line:

`key_press,function_called('key_action1','key_value1'...'key_action2','key_value2'...)`

First is the keyboard key that you going to assign a certain action to, followed by a comma and the function that you be calling with this key press. Typically, this is `ve_eegplot()` or `ve_edit()`. These functions are explained on the [Functions Wiki Page](https://github.com/BUCANL/Vised-Marks/wiki/Function-Reference) on Github, as well as in the built in Matlab help command.

![Key Select Command Popup]({{ site.root}}/fig/keyselectcommandpopup.png "Key Select Command Popup")

The `ve_eegplot()` and `ve_edit()` functions both contain different common actions you may want as hotkeys in them, some of which are outlined below. You can call as many key_actions as you would like in the same function. You can also assign a key to call two different functions by simply creating a new line and reusing the same designated key stroke.

### ve_eegplot

|key_action | key_value | Description |
|-----------|-----------|-------------|
| 'topoplot'|[figure handle]|	Draws a topographic plot of the time selected |
| 'drawp'	|[movement direction]|	Refreshes and redraws the figure |


> ## Where movement direction is:
>
> 0 - to redraw  
> 1 - to move left by one window  
> 2 - to move left by one second  
> 3 - to move right by one second  
> 4 - to move right by one window  
>
> {: .source}
{: .checklist}

### ve_edit

|key_action | key_value | Description |
|-----------|-----------|-------------|
|'quick_event_make' or 'qem'|	['name_of_event'] | Insert event at latency of mouse pointer
|'quick_channel_flag' or 'qcf'|	['name_of_chan_info_mark_label'] | Flag the channel at mouse pointer with the named mark. |
|'select_mark' or 'sm'|	['on' or 'off'] |	Select (winrej highlight) the period of time flagged by time_info mark under the mouse pointer. |
|'add_winrej_marks' or 'awm'|	['name_of_time_info_mark'] | Insert a mark on the data selected by the winrej highlight under the mouse pointer. |
|'remove_winrej_mark' or 'rwm'|	['name_of_time_info_mark'] | Remove the named time_info mark on for the period of winrej highlight under the mouse pointer. If the 'name_of_time_info_mark' is set as 'pop_select' a pop up selection appears on each use. |
|'add_page_mark' or 'apm'|	['name_of_time_info_mark'] | Applies a mark to the whole page window. |
|'remove_page_mark' or 'rpm'| ['name_of_time_info_mark'] |	Removes a mark from the whole page window. |
|'page_forward' or 'pf'| ['on' or 'off'] |	Moves 1 page forward, based on the time displayed. |
|'data_move' or 'dm'| ['on' or 'off'] |	Data move from data2 to data array for selected period. |


Once you have input all of the keystrokes that you want to be editing with, you should make sure that the other configuration settings are also set as desired. Across the top of the pop up page there are four additional selections that were not in the configuration interface:

![Top Part of Visually Edit]({{ site.root}}/fig/toppartofvisuallyedit.png "Top Part of Visually Edit")

You can change these fields by typing or by clicking the `| ... |` button and using the selection tool.

1. **Channels to display in the window:** In this field, you can select what channels/components you would like to display in the pop up scroll data window. Alternatively you can also use the number of channels to display option in the main config fields and use the side scroll bar to navigate the window.
2. **Events to display:** This will filter which events will be displayed on the scroll data.
3. **Mark types to include in rejection:** This section contains the types of marks that you will be rejecting when you click the Update EEG Structure button. By default this field contains manual marks.
4. At the top right there is a drop down selection between EEG and ICA data types. The marking system works the same way in ICA components mode, except instead of displaying the channel data it will display the component data.

## Editing Marks Visually

When you click the `| Ok |` button at the bottom of the interface, a scroll plot of your data that is set up the way you specified in the configuration should pop up. If you get an error in the matlab command window, make sure you have set your default renderer correctly:

`>> set(0,'DefaultFigureRenderer','OpenGL');`

Marks can either be in the x-axis (time) or in the y-axis (channels/components) depending on what functions you are using.

- If you are selecting a channel (**[Shift + left-click]**), you will see the whole channel get selected. In the case of a removal, this will remove the whole channel from the data.
- If you are selecting using the winrej highlighting tool (**[left-click -> drag -> release left-click]** in MATLAB 2014b or later, OR **[left-click -> release left-click -> drag -> left-click -> release left-click]** in earlier versions) in the time axis, you will be selecting a particular time but also be selecting all of the channels/components at once. In the case of doing a removal you will be deleting all of the channel data during that time frame. The missing time will then be indicated by a boundary event. You cannot remove a time section from only a single channel, as the time for all the channels need to match.  

**Note:** The vertical location of the color indicator on the screen does not matter, as it is applied to all of the channels during the selected time frame. If you have two colors during the same time frame it means the data is flagged for both reasons. If you scroll up and down through the channels you will see that the color marks do not move while the channels scroll up and down unattached.

## Adding a Mark

To start adding marks, use the winrej highlighting tool and hover your mouse over the selected data. Then, use one of the hotkeys you have assigned to mark the data with the desired mark. In the example below, a awm mark named custom_mark was added by using the key select command string:

`m,ve_edit('awm','custom_mark')`

Using an addition mark for the first time will cause the following popup to appear:

![New Mark Info]({{ site.root }}/fig/newmarkinfo.png "New Mark Info")

It should already be populated with most of the information you provided. Make sure you select a color for the mark by typing in the **[R G B]** or by clicking the `|   |` button and selecting from a color pallet. Once you have finished with this pop up, you should see the mark appear at the top of the data.

![Custom Mark Added]({{ site.root }}/fig/markworked.png "Custom Mark Added")

In this example, the navy blue custom_mark (see the name on the right side of the window) that we created above, was added to the area where the winrej had highlighted from approximately 84 to 87 seconds.

## Removing a Mark

The process to remove a mark is very similar to adding. Start by using the winrej tool to select the area where you would like to remove a mark. Make sure to keep your mouse hovered over that selection when pressing your designated remove key. In the example below part of the navy blue mark called custom_mark will be removed with the rwm command specified to remove custom_mark by using the string:

`M,ve_edit('rwm','custom_mark')`  
  
OR  
  
`M,ve_edit('rwm','pop_select')`  

No popups will appear in the first case because custom_mark was explicitly specified to be removed. In the second case, a popup menu will appear, and you will be able to select any of the existing marks, making it a more versatile but less efficient hotkey command.

![Custom Mark removed]({{ site.root }}/fig/removeworked.png "Custom Mark Removed")

## Defaults

If you leave the function `ve_edit()` blank, then your keystroke will give you the following popup menu to create an action:

![Edit Event Popup]({{ site.root }}/fig/newremovepopup.png "Edit Event Popup")

From this interface you can either create an **event** or **toggle a channel command**.

- Event
    - Make sure the Edit events button is selected.
    - Under Event type, type the event name.
    - Click `| Ok |`.
    - You should see a new event line created where your mouse was positioned.
  
- Channel
    - Make sure the Toggle channel marks status button is selected.
    - Select the mark designation (manual type is the default, but you can also make this the same as other channel/component marks).
    - Select the channel/component you wish to apply this to.
    - Click `| Ok |`.
    - You should see that the channel/component you selected is now highlighted with the designated mark color.

## Completing the Session

Once you have marked the data as you would like, you will need to click the `| Update EEG Structure |` button at the bottom right of the window. This will take the marks that you have added/removed/modified and update them in the EEG.marks structure, which will essentially save your changes so that the next time the data is loaded you will see your updated marks.

**NOTE:** Clicking `| Update EEG Structure |` does NOT delete/purge the selected channels/components or time points. It merely marks them in the EEG.marks structure, which could later be used to purge the marked data. The file also needs to be explicitly saved after this, because only the EEG structure in MATLAB has been updated, but the file itself has not been saved.

{% include links.md %}

