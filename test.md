---
layout: page
title: Modal model clips
permalink: /clips/
---

Reference recordings were made by plucking the strings behind the bridge on a
Jazzmaster unamplified. These references were analyzed to extract mode
parameters and the results were translated into resonant bandpass filters
implemented in faust. These filters are run in parallel as an effect in real
time on a teensy development board in a stomp-box enclosure.  

The following recordings were all made with a telecaster with the effect
applied between the guitar and amplifier. The preliminary variations here
are in the blend of the dry guitar sound with the filtered version and in
the tuning of frequency of modes.  


In this example the mode frequencies are left unchanged from the measured
reference. A scale is played with an increasing blend of the filtered signal:  

{% include no_tune.html %}


By tuning frequencies to notes of interest a less dissonant effect can be
achieved. In this example the fundamental of each string's model is tuned to
the closest octave of its fifth-fret harmonic. The fifth-fret harmonics contain
several notes useful to idiomatic guitar keys. All modes in a string's model are
tuned by the same amount to preserve the relationship between modes and
therefore some of the character of the modeled original. Moving up and down a
scale (in an intentionally chosen key) creates an effect of "cycling" through
mode frequencies as played notes and harmonics coincide with modes. As in the
first example the wet/dry mix is varied to illustrate the relationship between
the played note and the modal resonances.  

{% include third.html %}


In this final example each string's modes are tuned as though the guitar were in
an alternate tuning, in this case G G D D D# D#, taken from  Sonic Youth's
[tuning guide](http://www.sonicyouth.com/mustang/tab/tuning.html). The harmonics
and behind the bridge tones of this tuning were notably used by Thurston for
Bull in The Heather. By setting the effect to use the filtered signal only and
playing selected muted harmonics an impression reminiscent of the live version
of the Bull in the Heather intro can be made. A distortion effect is put after
the modal filters in this example to further emphasize certain resonances:  

{% include sy.html %}
