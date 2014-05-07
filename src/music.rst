.. _music:

Dynamic Music
=============

Overview
--------

The music in Crea will be dynamically generated. This means you will never hear the exact same track twice. The music is broken down into small sound clips that I am calling “samples”. These samples are played dynamically based off of some parameters such as how much danger you’re in, time of day, and how long the track has been playing.

The thing that turned me onto doing dynamic music was watching Renaud Bédard’s GDC talk Cubes All The Way Down. I had not heard too much about dynamic music in games, but I knew that it would be a perfect fit for a sandbox game. It is so easy to get sucked in for hours and before too long the music becomes very very very repetitive. What do we do when this happens? Mute! I know that’s what I do and it is a shame since video game music is so great. My hopes are that by making the music dynamic it doesn’t get repetitive enough to warrant mutiny…

As I have mentioned before, I made a tool for Charlie that I dubbed “Dynamuse”. Dynamuse is used to define the rules for when samples should play in a track. I will admit that Dynamuse is a little on the complicated side but with that comes much power. I recently got Dynamuse more or less completed and soon we will be hearing its true power.

Modding
-------

Because I’m all about modding and being open, I’m going to make Dynamuse available to everyone. That means that anyone who wants to can modify existing tracks or even create their own. Eventually I will make a tutorial on how to use Dynamuse, but for now here’s a brief overview of how it works.

A Dynamuse track is composed of any number of samples. A sample has a name and music file associated with it and it also contains a list of triggers. A trigger contains a few things but most importantly it contains a list of conditions. There are several types of conditions such as time of day, status of another sample (playing, paused, or stopped). If a trigger has all of its conditions met then it signals the sample to play. And that is about it. Simple, right? (not quite unless you’re a programmer)
