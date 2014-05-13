.. _overwriting_logic:

Modding Existing Game Logic
===========================

There is currently no built-in modding functionality for replacing 
existing Crea logic, but Python is dynamic enough for us to add or 
replace logic while the game is running. In order to do this, we 
have to change our mod's registration.py file.

An example is a good way to illustrate the method of 
post-registration logic replacement. In mods/mymod/registration.py::

	# We have to set up an event listener so that our replacement 
	# function runs after the game systems have been registered,
	# i.e. after the logic has all been loaded. 
	game.events['registration_finished'].listen(replaceCreaLogic)
	
	# Our replacement function will do the work of actually replacing
	# and/or adding any logic we want.
	def replaceCreaLogic():
		# game._____ is the path to the module we want to modify.
		# We can change existing functions,
		game._____.oldCreaFunction = myNewFunction
		# change existing variables,
		game._____.oldCreaVariable = myNewVariable
		# or even add new functions.
		game._____.anotherNewFunction = anotherNewFunction

	# Of course, we have to define our own logic.
	
	def myNewFunction(...):
		...

	myNewVariable = ...

	def anotherNewFunction(...):
		...

This method allows much more customization for modders. Actual 
support for modding existing Crea logic may be added in the future, 
but for now this method is sufficient.
