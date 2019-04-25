---
title: "Edit Marks by Script"
teaching: 20
exercises: 0
questions:
- "How do we edit marks using scripts?"
objectives:
- "Learn how to create and purge marks using scripts."
keypoints:
- "Flagging is crucial to preparing data for further analysis such as ICA because you can remove the bad channel and time data for the ICA, but then return the flags once ICA is complete, thus retaining the original data."
---

Marks can be generated from computing a flag structure based on any criteria you would like. Creating functions that test the data for particular features is easily done by generating a script.

The example below calls the function `chan_neighbour_r()`, which creates an array based on the statistical difference between channels. If these channels are correlated too similarly then they will be assigned a mark value of 1, while the good channels remain with a mark value of 0. This information is then stored in one of the `EEG.marks.chan_info().flags` arrays. All of the bad channels can be isolated using the **chan_info** marks structure and then dealt with accordingly.

> ## Example: Bridged channels
>
> Perform the calculation on the data and sort if by the resulting criteria.
>
> {: .source}
>
> > ## See code
> >   
> > `mr = mean(fisherz(EEG.m_neigbr_r_ch), 2);`  
> > `sr = std(fisherz(EEG.m_neigbr_r_ch), [], 2);`  
> > `msr = mr ./ sr;`  
> >
> > {: .output}
> {: .solution}
>
> If a channel fits the criteria specified, its index is added to an array.
>
> {: .source}
>
> > ## See code
> >
> > `flag_b_chan_inds = find(msr > ve_trimmean(msr, [bridge_trim]) + ve_trimstd(msr, [bridge_trim]) * [bridge_z]);`  
> >
> > {: .output}
> {: .solution}
>
> This next section of code creates a new channel mark label called **lnkflags** and fills it with the data in the array gathered above. It then copies the mark locations into the flags structure labelled manual. At the end, manual will be the only label that is updated with all of the information from the other criteria structures.
>
> {: .source}
>
> > ## See code
> > 
> > `%% Create a new array to store the results:`  
> > `lnkflags = zeros(EEG.nbchan, 1);`  
> > `lnkflags(chan_inds(flag_b_chan_inds)) = 1;`  
> > `%% Create the new marks label, set its color, and list which array contains the marks it is using.`  
> > `EEG.marks = marks_add_label(EEG.marks, 'chan_info', {'bridge', [.7, 1, .7], [.2, 1, .2], -1, lnkflags});`
> > `%% Add the marks label into the **manual** labelled structure.`  
> > `EEG = pop_marks_merge_labels(EEG, 'chan_info', {'manual', 'low_r', 'bridge'}, 'target_label', 'manual');`  
> > `%% Remove flagged channels (removes the **manual** channel mark label which deletes the channels stored`   
> > `%% in the **lnkflags** structure and any others that were merged into manual).`  
> > `EEG = pop_marks_select_data(EEG, 'channel marks', [], 'labels', {'manual'}, 'remove', 'on');`
> >
> > {: .output}
> {: .solution}
>
> More information on the marks functions can be found on the Functions and Marks Structure Design wiki pages. In the scalpart.htb script there are many mark labels all merged into manual. Areas marked with manual are then hidden before the data is passed on to the ICA analysis. If you want to remove a single mark label you can simplify the code to the following, directly rejecting the marks, without merging to **manual** first.
>
> {: .source}
>
> > ## See code
> >
> > `EEG = pop_marks_select_data(EEG, 'channel marks', [], 'labels', {'lnkflags'}, 'remove', 'on');`
> >
> > {: .output}
> {: .solution}
{: .challenge} 

{% include links.md %}

