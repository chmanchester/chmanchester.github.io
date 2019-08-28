---
layout: page
title: Modal effect clips
permalink: /clips/
---

Reference recordings were made by plucking the strings behind the bridge on a
Jazzmaster unamplified. These references were analyzed to extract mode
parameters and the results were translated into resonant bandpass filters
implemented in Faust. These filters are then run in parallel as an effect on a
Teensy development board in a stomp-box enclosure.  

The following recordings were all made with a Telecaster with the effect
applied between the guitar and the amplifier. The preliminary parameters
explored here are the blend of the dry guitar sound with the filtered version
and the tuning of the frequency of modes.  

In this example, the mode frequencies are left unchanged from the measured
reference. A scale is played with an increasing blend of the processed signal:  

{% include no_tune.html %}


A less dissonant effect can be achieved by tuning frequencies to notes of
interest. In this example, the fundamental of each string's group of modes is
tuned to the closest octave of its fifth-fret harmonic. The fifth-fret harmonics
contain several notes useful to idiomatic guitar keys. All modes in a string's
model are tuned by the same amount to preserve the relationship between modes
and therefore some of the character of the modeled original. Moving up and down
a scale (in an intentionally chosen key) creates an effect of "cycling" through
groups of modes as played notes and harmonics coincide with mode frequencies. As
in the first example the wet/dry mix is varied to illustrate the relationship
between the played note and the modal resonances.  

{% include third.html %}


In this final example, each string's modes are tuned as though the guitar were
in an alternate tuning, in this case G G D D D# D#, taken from  Sonic Youth's
[tuning guide](http://www.sonicyouth.com/mustang/tab/tuning.html). The harmonics
and behind the bridge tones of this tuning were notably used by Thurston Moore
for "Bull in The Heather". An impression reminiscent of the
[live version](https://www.youtube.com/watch?v=ebVkCZ0Ei9Q) of the "Bull in the
Heather" intro can be made by setting the effect to use the processed signal
only and playing selected muted harmonics:  

{% include sy.html %}


Nels Cline says of the strings behind his Jazzmaster's bridge:

> It makes the palette so much broader. I remember hearing that sound on Sonic Youth records, especially around that time when there was a lot of good detuned rock going on in the No Wave scene. If the bridges are set right, then I have some specific notes I can play behind the bridge, and it has a bell-like resonance. I can also really distort it. I can just rip behind the bridge and create the sound of tearing or horrible shrieking. I don’t know why I like those kinds of sounds, but I do. Sometimes, just before a big chord, I like to swipe behind the strings and then hit the chord so it creates this splaying effect. It’s just part of my sound. I’m lost without it. It’s no fun to play other guitars for a whole night because I’m so used to being able to go to certain sounds like that.

— Nels Cline ([*Premier Guitar*](https://www.premierguitar.com/articles/Interview_Nels_Cline_?page=2), 2010)
