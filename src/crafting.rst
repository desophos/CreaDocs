.. _crafting:

Crafting
========

Overview
--------

In Crea the majority of items are obtained through crafting them. Crafting is a simple process but in order to craft an item you first must obtain that item’s recipe. There are several means to acquiring recipes: monster loot; treasure chests; quests; and, most importantly, the Researcher NPC.

The Researcher is specifically for discovering new recipes. First you provide the researcher with materials. Upon returning to him after some time, you gain any recipes his research turned up. Upon request the research can provide hints as to what items he may need to discover more recipes.

After obtaining an item recipe, you must obtain the materials needed to craft the item. Additionally, many items will require you to be near "crafting surfaces" such as an anvil or furnace. To make crafter lives easier, materials in nearby containers will be accessible during crafting. This means you can stash all of your ingots in a treasure chest next to your anvil. Then you do not have to move them from the chest to your inventory before crafting.

Modding
-------

Creating new items to craft is quite simple. All it requires is to add a craft component to the item. Through this you can specify the item’s category, subcategory, surface requirement (if any), list of materials and quantity required, and the quantity produced::

	#Example for Copper Ingot
	craft = Craft('Recipes', 'Smelting', 'anvil')
	craft.add('mods/base/tile/copper_ore.ce', 3)
	craft.quantity = 1

The surface requirement can be left empty to designate that the item can be crafted anywhere. If it is specified, the surface requirement does not refer to a crafting surface item but to a crafting surface service. There is a “Surface” component which enables entities to provide any number of crafting services.

Most items requiring an anvil can be crafted with any anvil; however, some special items may need an upgraded anvil. This upgraded anvil would provide the services needed for both the special items and any other items requiring an anvil. Any item can provide any surface service, so it is entirely possible to create uber surface items. It is of course possible to create entirely new crafting surface services that new items can require::

	#Iron Anvil
	surface = Surface(["anvil"])
	#Uber Anvil
	surface = Surface(["anvil", “uber anvil”])
