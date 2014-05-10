.. _templates:

The Templating System
=====================

.. _introduction:

Introduction
------------

Templates are an advanced topic. This tutorial is necessary only for
those modders who desire to not only create content, but also define
new kinds of content.

Fully understanding Templates will require an understanding of class
inheritance.

.. _creating_entities:

Creating Content Entities
-------------------------

A Content Entity (hereafter an *entity*) is created with a single .ce
file. When it comes to creating entities, all behavior is defined in
the engine, in :ref:`siege.component`, and all content is defined in
*Templates*, which are the subject of this document. For more
information on Components, see :doc:`components`.

Note that a single .ce file instantiates a single entity with specific
content passed as arguments to the class constructor. This is the main
method of creating Crea content. For more information on content
creation, see :doc:`ce_files`.

When an entity is instantiated from a .ce file, the Template class
assigns components to the entity based on which component
initialization functions that entity's superclasses call on the way up
the inheritance chain from the entity's class to the Template class
itself.

For example, a Broadsword is a Sword, a Sword is a Weapon, a Weapon is
an Item, and an Item is a Template. ``Weapon.__init__()`` calls
``self.isWeapon()``, which tells Template to assign a Weapon component
to the entity, and ``Item.__init__()`` calls ``self.isItem()``, which
does the same for an Item component. The Broadsword therefore has the
behavior of a Weapon and an Item. (``Weapon.__init__()`` also
conditionally calls other component assignment functions.)

.. _creating_templates:

Creating New Templates
----------------------

If you're familiar with the concept of and reasons for subclassing, you
know why you would create your own templates: you have entities that
you want to create that share specific features and components, and you
want to create a convenient interface for creating .ce files for these
entities.

For example, if swords didn't exist in the game, you might want to
create a Sword template with information on what swords share in
common. Let's take a look at the Sword template::

	class Sword(Weapon):
		def __init__(self, **kwargs):
			kwargs.setdefault('useTime', 38)
			kwargs.setdefault('useAnimation', ("sword", "arm_front"))
			kwargs.setdefault('useVisual', Substitute("sword"))
			if 'materials' in kwargs:
				kwargs.setdefault('subcategory', "Swords")
			Weapon.__init__(self, "Sword", kwargs.get('onUse', useMeleeWeapon), **kwargs)

With ``kwargs.setdefault('useTime', 38)``, we decide that all swords
should have a useTime of 38 unless overridden in the .ce file (or by
subclasses, if we subclass Sword).

The Weapon template is somewhat more complicated; let's look at an
excerpt from that one::

	class Weapon(Item):
		def __init__(self, category, onUse, **kwargs):
			kwargs.setdefault('unique', True)
			Item.__init__(self, **kwargs)
			if 'materials' in kwargs:
				kwargs['category'] = kwargs.get('category', "Weapons")
				self.craftable(**kwargs)
			self.isWeapon(
				category=category,
				power = kwargs['power'],
				attackType = kwargs.get('attackType', AttackType.Melee),
				damageType = kwargs.get('damageType', DamageType.Physical),
				...
			)

The most important concept in this template is calling component
assignment functions. As mentioned in :ref:`creating_entities`, the
Weapon template calls ``self.isWeapon()`` and possibly
``self.craftable()``. These functions instruct Template to assign
Weapon and Craft components respectively to the entity. However, these
functions are called with arguments that are defaults and/or overridden
by subclasses. For example, attackType is set to ``AttackType.Melee``
unless a subclass (e.g. Bow) overrides it.

As you can see, creating your own templates can be a great way to save
time in content creation and to avoid code duplication by setting
defaults and calling component assignment functions so that you don't
have to pass so many arguments in all your .ce files -- basically, all
the advantages of class inheritance!

.. _helper_classes:

Helper Classes
--------------

TODO
