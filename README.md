evestim - evidence based estimator
==================================

Based on ideas from https://www.joelonsoftware.com/2007/10/26/evidence-based-scheduling/  
but simplified and tailored for personal use:  
- to improve inner estimation ability by seeing the history,  
- to amortize external estimations told to manager.

The core idea is that you mostly underestimate,  
but you can become consistent in how you underestimate,  
and so real time can be predicted.

Edit "evestim.txt" file and just run it.  
It will update itself calling "evestim" script via shebang line.  
If your OS does not support shebang, then just run "evestim" script.

Lines starting with "#" or anything but digit or "-" are comments.

Each other line is:

    {velocity} {estimation} {real} {title with spaces}
    0.4 2 5 https://dev.example.com/issues/1230 - Some done task.
    0.6 1 1.7 https://dev.example.com/issues/1231 - Other done task.

Estimation and real are hours or whatever fits you better.

If velocity is "-", then script will calculate velocity = estimation / real:

    - 4 8 https://dev.example.com/issues/1232 - Just done task.
    -->
    0.5 4 8 https://dev.example.com/issues/1232 - Just done task.

But if real is "-" too, then script will calculate the range of velocity and real:

    - 4 - https://dev.example.com/issues/1233 - Open task.
    -->
    0.4-0.6 4 6.7-10.0 https://dev.example.com/issues/1233 - Open task.

Ranges are recalculated each time, as if they were just "-".

Cut too old lines and paste them to archive file  
to work with fresh (and hopefully improved) min/max velocity.

TODO: Move from trivial min/max to more precise probability percentiles.

Velocity is limited in config below to avoid division by zero, infinities, etc.

evestim version 0.1.1  
Copyright (C) 2017 by Denis Ryzhkov <denisr@denisr.com>  
MIT License, see http://opensource.org/licenses/MIT
