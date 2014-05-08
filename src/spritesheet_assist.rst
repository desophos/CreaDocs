.. _spritesheet_assist:

Frame Generation from Spritesheets
==================================

Introduction to getSpriteFrames()
---------------------------------
Each entity with an :class:`AnimationComponent`
has a .ce file containing, among other information, variables
corresponding to each of its animations, each initialized
with a function called getSpriteFrames() from
the siege.component.Monster module.

This function takes a list of Frames and two PixelVectors as arguments.
These arguments themselves take specific numbers as arguments,
corresponding to pixels on the axes of the .png spritesheets
for the given entity.

TexturePacker exporting
-----------------------

So that modders (you) don't have to count pixels in the .png,
TexturePacker has a utility to export a .xml file with pixel data
of all the sprites in a spritesheet.

TODO: give more information about *how* to do TexturePacker exporting

spritesheet_assist.py
---------------------

However, if this .xml file were all you had, you would have to manually
create the getSpriteFrames() functions in the .ce file. Fortunately,
there's a script to do it for you: spritesheet_assist.py!

This script generates a .txt file with all the animation variables
defined with the names of the corresponding spritesheets,
each with its own getSpriteFrames() function with the proper pixel data.
All you have to do is copy that .txt file (it has the same name as the
entity) into your .ce file for the entity, add your own animation cycle
information, add custom behavior, and your new entity is complete!

More information
----------------

For more information about animation cycles, custom behavior, and other
aspects of a .ce file, see :doc:`ce_files`.
