.. _world:

Interactive World
=================

Overview
--------

As expected in a sandbox game, worlds in Crea can be fully reconstructed. Terrain exists on two tile layers referred to as ground layer and wall layer. The ground is collidable and sits in front of the wall layer which is not collidable. Aside from being able to rebuild the terrain, it is also possible to place and remove entities such as tables and treasure chests.
Unlike tiles which are grid based, entities are free to be placed wherever there is provided support. Item placement has been made as simple and intuitive as possible. When a position is suitable for placing the active item a visual hint is provided, that is a transparent rendering of the item and where it will be placed at. If an item does not quite fit where the mouse is hovered but there is an available nearby space then it will show a hint there.

These items can then be interacted with, such as opening a door or flipping a switch. Interacting with an item is not restricted to right clicking on it though. There are other interactions such as harvesting it with an axe, stepping on a button, using an item or attempting to remove the item’s support. With all of these interactions, the world moves from a static playground to a very much interactive and alive one.

Modding
-------

The key to having an interactive world is the modding support. New tiles can be created, there is a placement component that can be added to any item, and an interactive component added to any entity.

Creating a new tile requires the use of the TileComponent. Here is an example with some embedded comments::

	name = 'Dirt'
	render = Render('mods/base/tile/dirt.png')
	add(render)
	 
	#ItemComponent making it possible to collect this tile
	item = Item()
	item.stack = 999
	item.delay = 50
	add(item)
	 
	#TileComponent
	tile = Tile()
	#Number of tile variants
	tile.variants = 3
	#Tile priority used for tile overlapping
	tile.priority = 10
	#Amount of life the tile has.
	tile.durability = 10
	#Defines the groups the tile belongs to.
	#This is used for tool compatability among other things.
	tile.groups = ['soil']
	#The layer this tile is placed on - either 'ground' or 'wall'.
	tile.layer = "ground"
	add(tile)
	 
	#This sets up the frames for the tile to make it easy to draw
	render.data = standardTile(tile.variants, tile.priority)
	
As mentioned above, there is a placement component which makes very easy to make items placeable. A item PlacementComponent is composed off a list of possible axes that the item can be placed on::

	placement = Placement()
	floorAxis = PlacementAxis(AxisType.AXIS_FLOOR, Vector(5, 37))
	floorAxis.addSupport(PlacementSupport(SupportType.SUPPORT_TOP, Vector(5, 37)))
	placement.addAxis(floorAxis)
	add(placement)

The item can be placed on any axis but only one at a time. The types are floor, ceiling, left, right, backwall and wall. The 'backwall' is support from the wall layer and 'wall' is the combination of left and right. In addition to the axis type, each axis has a range, an area, and a list of supports.

The range of an axis defines where and how much support is needed to place the item down. This is the second parameter for PlacementAxis. Here we have Vector(5, 37), which means to start at pixel 5 on the bottom of the image (since it is floor axis) and go for 37 pixels. This will default to starting at 0 and using the entire axis length (width or height of image). The reason for this feature is to enable items to have a skinny base and only require support for the base and not the entire width of the item, such as a candelabra.

Area for an axis is the game’s physical representation of the item relative to upper left corner of the image. This area is used to reserve space for items in the world. This area defaults to the size of the image. The last part of an axis is the list of support it can provide. It is defined very similarly to an axis with a support type and a range of where and how much support to provide. The type can be on any one of the four sides; top, bottom, left or right.

Moving onto InteractableComponent, which is much simpler. An InteractableComponent contains a list of interactions and a callable object. Here is an example of the door InteractableComponent::

	def interactDoor(args):
		#Toggle between being open and closed
		#We can check which animation is currently being played to know the state
		door = args["entity"]
		if door.animation.isPlaying("closed"):
			door.animation.play("open")
			door.physics.setActive(False)
		else:
			door.animation.play("closed")
			door.physics.setActive(True)
 
	interactable = Interactable()
	interactable.add("interact", interactDoor)
	add(interactable)
	
The second to last line adds an interaction, the "interact" which is called when right clicking on an item. As mentioned in the overview there are several interactions built into the engine. Because the interaction is simply a string, it is entirely possible to create new interactions.
