.. _biomes:

World Generation
================

When I set out designing the world generation for Crea, I knew I wanted to enable players to be able to have unique experiences with every world. The answer was quite obvious – moddable biomes. If players could easily create their own biomes or play with other player’s biomes then the possibilities are endless. Here is an example of what a biome content looks like::

	biome.name = "Plains"
	#How often this biome should be used
	biome.frequency = 50

	#The minimum and maximum dimensions of the biome
	biome.minWidth = 80
	biome.maxWidth = 300
	biome.minHeight = 80
	biome.maxHeight = 300

	#Specifies that this biome only occurs on the surface
	biome.surface = True

	#The elevation of the biome
	biome.elevation = 100

	#The amount of elevation changes (hills) in the biome
	biome.hilly = 10.0

	#How much the lowest and highest elevation can differ
	biome.relief = 2.0

	#A list of biomes that cannot neighbor this biome
	biome.blacklist = ['Volcano']

	#List of actions to apply to the world
	#This part is still being worked on.
	biome.placeTiles('stone', frequency=20, pattern)
	biome.placeTiles('mud', frequency=10, pattern)
	biome.plant('grass', frequency=40)
	biome.plant('tree', frequency=5)
	
Some of these details will change but this gives a good idea of what a biome looks like. Most of the contents are explained. You can do simple things like increase the ‘hilly’ and ‘relief’ properties and have mountains. For the real power we have the actions section at the bottom.

After the basic terrain has been generated, each biome has its list of actions applied to it. In this case all Plains biomes will have stone randomly placed, then some mud, then lots of grass will be planted, and finally a few trees. I plan to include several basic actions that players can use to make creating biomes as simple as possible, but what if you want to do something completely different? Let's say you want to build a crazy dungeon. For advanced modders, I have added a special action that takes a callback function which provides you ample power to twist the world to your desires.

Now that we know how biomes are made, the next logical question is “How are they used?” Players will have the option to create a world with simple or advanced options. With simple the player chooses a name and the world size and is good to go. From the advanced options the player is also able to adjust the frequency of biomes. Want a world consisting purely of bunny warrens? Kelley, our artist, does!

Not only will players never have to see the same world twice, but they will be able to explore the worlds that they want.
