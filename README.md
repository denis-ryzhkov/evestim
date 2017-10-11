evestim - evidence based estimator
==================================

Based on ideas from [Evidence Based Scheduling](https://www.joelonsoftware.com/2007/10/26/evidence-based-scheduling/)  
but simplified and tailored for personal use:  
- to improve inner estimation ability by seeing the history,  
- to amortize external estimates told to manager.

The core idea is that you mostly underestimate,  
but you can become consistent in how you underestimate,  
and so real time can be predicted.

Term "velocity" used in EBS is a relative value "velocity = estimate / real".  
However Agile defines "velocity" as an absolute value - a sum of the estimates of features, delivered in one sprint.  
Also "velocity" in a general sense means "speed". "low speed" can mean "low productivity", while even highly productive person can underestimate a lot.  
To avoid all this confusion, "velocity" is replaced here with "optimism = real / estimate".  
The bigger is optimism - the smaller is estimate and the bigger is real time.  

Edit "evestim.txt" file and just run it.  
It will update itself calling "evestim" script via shebang line.  
If your OS does not support shebang, then just run "evestim" script.

Lines starting with "#" or anything but digit or "-" are comments.

Each other line is:

    {optimism} {estimate} {real} {title with spaces}
    2.0 2 4 https://dev.example.com/issues/1230 - Some done task.
    1.5 4 6 https://dev.example.com/issues/1231 - Another done task.

Note how simple it is to multiply optimism by estimate to get real time  
vs a more complex option to divide estimate by fractional velocity.

When you estimate a new task, use "-" in fields unknown yet:

    - 4 - https://dev.example.com/issues/1232 - A new task.

Script will replace each "-" with predicted range, based on 100 recent lines of history.  
History is limited because your estimation ability improves over time,  
so we can get more narrow ranges ignoring old bad estimates.  
At the same time we should not ignore bad estimates if they are fresh enough.

    - 4 - https://dev.example.com/issues/1232 - A new task.
    -->
    1.5-2.0 4 6.0-8.0 https://dev.example.com/issues/1232 - A new task.

In this example estimate of 4 hours was amortized to 6-8 hours.  
The range could be much wider if you have done inconsistent estimates recently.  
It is tempting to narrow the range automatically via the "confidence interval",  
but the [black swan theory](https://en.wikipedia.org/wiki/Black_swan_theory)
advises to avoid ignoring information that looks improbable, as it may bite you.  
That's why script adds a chart showing distribution of probability in min/max range.  
You may find that particular distribution could be or should not be trimmed.

- [ ] TODO: Allow to select a range at chart that fits enough probability.
Use this range for predictions instead of 100% min/max.
Requires either GUI or configuration included into the file format.

In case you have zero history, optimism would be limited to wide enough 0.1-10.0 range.  
Script will recalculate each range once it has more data.

Once you finished the task, replace predicted range with real time spent  
and the script will calculate precise optimism for this task:

    1.5-2.0 4 7 https://dev.example.com/issues/1232 - Just done task.
    -->
    1.8 4 7 https://dev.example.com/issues/1232 - Just done task.

It will become one of 100 recent lines of history used for the next predictions.

evestim version 0.2.1  
Copyright (C) 2017 by Denis Ryzhkov <denisr@denisr.com>  
MIT License, see http://opensource.org/licenses/MIT
