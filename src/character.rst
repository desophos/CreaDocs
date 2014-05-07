.. _character:

Character Customization
=======================

Overview
--------

The character creation provides a great amount of customization. The first and most important choice the player has is choosing the type of body to use. The body type defines the majority of the aspects for the character - appearance, customizations, equipment, stats, and so on. A body type can be “human male”, “human female” or even some completely different race such as a Robot. Body customizations are a part of the rendered body that can be changed such as hair, shirt, pants, shoes, and accessories.

Aside from customizations, a body type also defines which equipment slots the character will have, the base stats for the character, and the proficiencies. Each of these will be expanded upon in their own respective feature posts. With these character aspects being associated to the body type means that entire unique characters can be created. Such as a Robot race that has its own unique pieces of equipment, customizations, stats, and proficiencies.

Modding
-------

Every aspect of character customization is moddable: body types, customizations, equipment slots, stats, and proficiencies. I will cover all of this by going over the human male body script, which is quite lengthy. Some explanation is embedded in the code - Look for lines starting with '#'::

	name = "Human Male"
	body = Body('hm')
	 
	body.render = ModularRender('mods/base/body/hm/human.scml')
	 
	#Character's are rendered with a ModularRender.
	#This means that the character is broken up into small sprites
	#such as head, arms, hands, torso, legs, clothing, hair and so on.
	#Each of these sprites can be replaced with another sprite.
	#These mappings below associate a name with a sprite or group of sprites.
	#This makes it possible to do things like recolor all body sprites or hide them.
	 
	body.render.addMapping('body', 'arm/arm_lower_0.png')
	body.render.addMapping('body', 'arm/arm_upper_0.png')
	body.render.addMapping('body', 'arm/hand_0.png')
	body.render.addMapping('body', 'leg/leg_lower_0.png')
	body.render.addMapping('body', 'leg/leg_upper_0.png')
	body.render.addMapping('body', 'leg/foot.png')
	body.render.addMapping('body', 'head/head.png')
	 
	#Lots more mapping removed for brevity...
	 
	#Sets the animation render ordering.
	body.render.setOrder(['arm_back', '', 'arm_front'])
	 
	#Set up the body animations
	body.animation = Animation("idle")
	body.animation.bind('arm_back', 'run_arm_back', 'run')
	body.animation.bind('arm_back', 'walk_arm_back', 'walk')
	body.animation.bind('arm_back', 'idle_arm_back', 'idle')
	body.animation.bind('arm_back', 'jump_arm_back', 'jump')
	body.animation.bind('arm_back', 'fall_arm_back', 'fall')
	body.animation.bind('arm_back', 'use_arm_back', 'use')
	body.animation.bind('arm_front', 'run_arm_front', 'run')
	body.animation.bind('arm_front', 'walk_arm_front', 'walk')
	body.animation.bind('arm_front', 'idle_arm_front', 'idle')
	body.animation.bind('arm_front', 'jump_arm_front', 'jump')
	body.animation.bind('arm_front', 'fall_arm_front', 'fall')
	body.animation.bind('arm_front', 'use_arm_front', 'use')
	body.animation.setLooping('idle_arm_front', False)
	body.animation.setLooping('jump_arm_front', False)
	body.animation.setLooping('fall_arm_front', False)
	body.animation.setLooping('use_arm_front', False)
	body.animation.setLooping('use', False)
	body.animation.setLooping("death", False)
	 
	#Here we add the customizations this body supports
	#These show up in the Character Creation UI when this body is selected
	hair = Customization('Hair', True)
	hair.isOptional = True
	body.addCustom(hair)
	 
	accessory1 = Customization('Accessory 1', True)
	accessory1.isOptional = True
	body.addCustom(accessory1)
	 
	accessory2 = Customization('Accessory 2', True)
	accessory2.isOptional = True
	body.addCustom(accessory2)
	 
	body.addCustom(Customization('Shirt', True))
	body.addCustom(Customization('Pants', True))
	body.addCustom(Customization('Shoes', True))
	 
	#Import this file and make substitions to body
	#body.addSub('torso/torso.png', 'mods/base/body/hf/torso/torso.png')
	 
	body.hide('customizations/gloves.png')
	body.hide('customizations/helmet.png')
	 
	#Define which gear slots this body supports
	#These show up in the character equipment UI in game
	body.gear = Gear()
	body.gear.slots.append(GearSlot("head", "mods/base/body/hm/gear/head.png"))
	body.gear.slots.append(GearSlot("chest", "mods/base/body/hm/gear/chest.png"))
	body.gear.slots.append(GearSlot("hand", "mods/base/body/hm/gear/hand.png"))
	body.gear.slots.append(GearSlot("leg", "mods/base/body/hm/gear/leg.png"))
	body.gear.slots.append(GearSlot("feet", "mods/base/body/hm/gear/feet.png"))
	 
	#Define which stats characters with this body get
	body.stats = Stats()
	body.stats.stats.append(Stat('HP', 'HP', 300, 9999, True, True))
	body.stats.stats.append(Stat('AP', 'AP', 100, 9999, True, True))
	body.stats.stats.append(Stat('ATK', 'Attack', 100, 999, False, True))
	body.stats.stats.append(Stat('DEF', 'Defense', 100, 999, False, True))
	body.stats.stats.append(Stat('MATK', 'M Attack', 100, 999, False, True))
	body.stats.stats.append(Stat('MDEF', 'M Defense', 100, 999, False, True))
	body.stats.stats.append(Stat('AGI', 'Agility', 100, 999, False, True))
	body.stats.stats.append(Stat('DEX', 'Dexterity', 100, 999, False, True))
	 
	#Define which proficiencies characters with this body get
	execfile('mods/base/proficiency/craft/craft.py')
	execfile('mods/base/proficiency/explore/explore.py')
	execfile('mods/base/proficiency/fight/fight.py')
	execfile('mods/base/proficiency/gather/gather.py')
	body.proficiencies = Proficiencies()
	body.proficiencies.proficiencies.append(craftProficiency)
	body.proficiencies.proficiencies.append(exploreProficiency)
	body.proficiencies.proficiencies.append(fightProficiency)
	body.proficiencies.proficiencies.append(gatherProficiency)
	 
	add(body)

We are using Spriter for our character animations. Consequently, in order to create new animations or an entirely new body type then you must use Spriter. You can easily setup new sprite mappings, mentioned above at the top of the code.

A body script, such as the one above, defines the supported customizations; however, the actual customizations are defined elsewhere in their own scripts. Creating new customizations is reasonably straightforward. A customization is a very simple entity. It contains a "Custom" component which takes two parameters. The first is the name of the customization this is for and the second is if it supports recoloring or not. After that a Custom component can define any number of sprite substitutions that take place if this customization is used. Here we change what the top hair looks like and hide the bottom hair since it is unused::

	name = "Guy Hair"
	custom = Custom("Hair", True)
	custom.addSub('head/hair_top.png', 'mods/base/custom/hair/hm_hair.png')
	custom.hide('head/hair_bottom.png')
	add(custom)
 
As you can see, creating new hair styles, clothing and any other customizations is very easy!
