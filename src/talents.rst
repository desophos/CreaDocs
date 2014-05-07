.. _talents:

Talents and Skills
==================

Overview
--------

Character progression can be broken into two distinct parts: character level and talents.

The character level is strictly used for combat related things. It is used to represent your character's strength and consequently is utilized in various combat algorithms such as the damage algorithm or for determining the monster levels. Upon leveling your character will gain stats. Also, equipment will typically have a character level requirement.

Characters have talents that represent the character's proficiency in a field. Currently we have four talents planned; Arms, Magic, Gather, and Craft. By performing actions related to the talent your character will occasionally gain TP (Talent Points). For example by crafting items or learning new recipes you may gain TP for your Craft talent or hitting a monster with magic may yield TP for the Magic talent.

After accumulating enough TP your character's talent will level up. With enough TP you can spend it on purchasing and upgrading skills. Leveling up a talent grants access to new skills and skill upgrades. There will be a mixture of active and passive skills. Active skills are skills that can be added to the toolbar and used as an action such as a magic spell. Passive skills are always affecting the player such as "Defense Up".

Modding
-------

As usual, nearly all aspects of talents can be modified ranging from modifying an existing skill to creating an entirely new talent. Talents are tied to body types (covered in this post) meaning different races can have different talents.
As far as the actual modding goes, here is what the adding the talents to a character body looks like::

	body.talents = Talents()
	body.talents.talents.append(armsTalent)
	body.talents.talents.append(craftTalent)
	body.talents.talents.append(gatherTalent)
	body.talents.talents.append(magicTalent)
	
A Talent is simple and is only composed of a name, a "points to next level" list, and a list of skills::

	armsTalent = Talent("arms", [100] * 20)

Skills are a little bit more complex. First of all, skills can be passive and/or active. A passive skill is always in effect while an active skill must be used. It is possible to have a skill with both elements. Passive skills need to provide two callback functions: "enable" and "disable". Active skills need to provide the callback function "use". All skills should provide a "description" callback function::

	def enableStatUp(stat, level, user):
		user.stats.get(stat).adjust(5 * level)
	 
	def disableStatUp(stat, level, user):
		user.stats.get(stat).adjust(-5 * level)
	 
	def descriptionStatUp(stat, level):
		return "Increases a character's {} by {}.".format(stat, 5 * level)
	 
	atkSkill = Skill(name="Attack Up", icon="mods/base/talent/arms/attack_up.png", costs=[100] * 5)
	atkSkill.enable = partial("ATK", enableStatUp)
	atkSkill.disable = partial("ATK", disableStatUp)
	atkSkill.description = partial(descriptionStatUp, "Attack")
	armsTalent.skills.append(atkSkill)
	 
	defSkill = Skill(name="Defense Up", icon="mods/base/talent/arms/defense_up.png", costs=[100] * 5)
	defSkill.enable = partial("DEF", enableStatUp)
	defSkill.disable = partial("DEF", disableStatUp)
	defSkill.description = partial(descriptionStatUp, "Defense")
	armsTalent.skills.append(defSkill)
	
The "enable" function is called when the skill is activated. This is when the player first get the skill or when the character is loaded. The sibling function, "disable" is used when the skill needs to be deactivated. This is typically when the character is being saved to disk but it also makes it possible to temporarily disable passive skills with monster abilities. The "description" function is called when we need to display the tooltip for the skill. Having it be a function makes it dynamic so it can be dependent on any variable - such as the level of the skill. The "use" function is not featured here but it is more or less the same as the previous functions. It is called when the skill is being used.

Creating skills and entirely new Talents is definitely some more advanced modding. However, with the complexity comes some awesome power and I am anxious to see what skills players come up with.
