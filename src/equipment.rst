.. _equipment:

Equipment
=========

Overview
--------

All equipment in Crea is randomly generated to some degree. Each piece of equipment is made up of attributes. These attributes can be either positive or negative effects such as stat increase/decrease, increased health regeneration, or vampiric hit. The human race has a large assortment of equipment slots: Head, Body, Hands, Legs, Feet, Ring, and Neck. All but the last two will change your character’s appearance.

The majority of equipment found in Crea will have some constant attributes as well as some random ones. Such as all Copper Greaves will always have a DEF +5 attribute and will randomly be assigned 0-2 of the following attributes: ATK +(1 to 2), MDEF +(2 to 4), and AGI +1. (This is just an example).

Modding
-------

All aspects of equipment are completely moddable. Not only can you create new pieces of equipment, it is very possible to create entirely new attributes and even add new equipment slots.

Creating a new piece of equipment is reasonably simple. In addition to creating a normal item entity (Render, Item, Craft components), add an equipment component to an item. An Equipment component has a list of image substitutions and a function (onCreate) called when a new piece of that equipment is created. Inside of the onCreate function is where attributes are added to the equipment component::

	equipment = Equipment('chest')
	#These are image substitutions similar to what is discussed in Character Customization
	equipment.addSub('customizations/shirt_front.png', 'mods/base/item/armor/chest/silver_breastplate/torso.png', Vectorf(2, 0))
	equipment.addSub('customizations/sleeve_lower_0.png', 'mods/base/item/armor/chest/silver_breastplate/arm_lower_0.png')
	equipment.addSub('customizations/sleeve_upper_0.png', 'mods/base/item/armor/chest/silver_breastplate/arm_upper_0.png')
	#... and more substitutions
	 
	def createSilverChestplate(component):
		component.addAttribute(ChangeStatAttribute('HP', 5))
		component.addAttribute(ChangeStatAttribute('Attack', 3))
	 
	equipment.onCreate = createSilverChestplate
	add(equipment)
	
Creating a new type of attribute involves a new python class that implements an interface. Here is an example with a brief description is provided with each method::

	class ChangeStatAttribute(object):
		name = "ChangeStat"
		def __init__(self, stat='', value=0):
			self.stat = stat
			self.value = value
			self.changed = 0
	 
		def alignment(self):
			#Called to determine if this is a positive or negative attribute
			return self.value >= 0
	 
		def quality(self):
			#Reports the quality of this attribute.
			#The quality of an equipment piece is the sum of its attributes.
			return self.value * statWeights[self.stat]
	 
		def onEquip(self, args):
			#Called when this attribute is being equipped
			player = args['player']
			stat = player.stats.get(self.stat)
			if stat.hasMax():
				self.changed = stat.adjustMax(self.value)
			else:
				self.changed = stat.adjust(self.value)
	 
		def onUnequip(self, args):
			#Called when this attribute is being unequipped
			player = args['player']
			stat = player.stats.get(self.stat)
			if stat.hasMax():
				stat.adjustMax(-self.changed)
			else:
				stat.adjust(-self.changed)
			self.changed = 0
	 
		def save(self, stream):
			stream.saveString(self.stat)
			stream.saveInt32(self.value)
			stream.saveInt32(self.changed)
	 
		def load(self, stream):
			self.stat = stream.loadString()
			self.value = stream.loadInt32()
			self.changed = stream.loadInt32()
			
When creating a new piece of equipment you must specify which equipment slot the new item is compatible with. In order to create a new equipment slot it only requires adding the slot to the character body script (see Character Customization). After this is done any new equipment can use the new equipment slot::

	#Define which gear slots this body supports
	#These show up in the character equipment UI in game
	body.gear = Gear()
	body.gear.slots.append(GearSlot("head", "mods/base/body/hm/gear/head.png"))
	body.gear.slots.append(GearSlot("chest", "mods/base/body/hm/gear/chest.png"))
	body.gear.slots.append(GearSlot("hand", "mods/base/body/hm/gear/hand.png"))
	body.gear.slots.append(GearSlot("leg", "mods/base/body/hm/gear/leg.png"))
	body.gear.slots.append(GearSlot("feet", "mods/base/body/hm/gear/feet.png"))
