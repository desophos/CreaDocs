.. _components:

Component-Based Engine
======================

Overview
--------

The engine that powers Crea is known as a Component-based engine. For anyone interested in modding Crea, this is a crucial aspect of the game to understand. Everything in Crea is made up of entities, and an entity is a container for holding multiple components. A component is common logic encapsulated.

Let's say we want to represent a table in Crea. We would start with a blank entity, then add a render component and direct it to use the table sprite. Having a render component enables the entity to be rendered – in other words, now the table will show up in the game. Next we add an item component which enables the table to be carried in a character’s inventory. Finally we want to add a craft component, which enables the table to be crafted.

Modding
-------

As you can see, an entity is the sum of its contained components. It’s a pretty simple process of using components like building blocks to create something new. A component-based engine is by nature very modular. There are currently 30 different components and growing. Here are some component examples.

| :class:`RenderComponent` – Used for rendering the entity
| :class:`AnimationComponent` – Enables the entity to be animated (requires render component)
| :class:`ItemComponent` – Treats the entity as an item making it possible to add to inventories
| :class:`CraftComponent` – Makes it possible to craft the entity (requires item component)
| Interactive – There are several ways to interact with an entity. Right click on it to “interact” or “use” it from your toolbar are just two examples.
| :class:`PlacementComponent` – Enables an item to be placed into the world from inventory (requires item component)
| :class:`MonsterComponent` – Treats the entity as a monster.

Sometimes, as a modder, you’ll find yourself in need of a component that does not exist – at least not yet! It is entirely possible to create a new type of component. Components have a simple interface and are created in a python script just like the rest of the content.
