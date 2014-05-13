.. _pyd_usage:

Working with siege.pyd
======================

With the Siege module (siege.pyd) you are given access to the game's 
engine without having to run the game. This makes it possible to do 
"import siege" from Python which enables modders to run scripts and 
quickly test things out.

Here are Dropbox links to the current (0.8.0 omega) siege.pyd:

| Windows: https://www.dropbox.com/s/yg9k4mb36zzqngk/siege.pyd
| Mac: https://www.dropbox.com/s/8w30dqmbu40li0s/siege.so
| Linux: https://www.dropbox.com/s/drbvzaccgjlmioc/siege.so

There are two things this module requires to run properly. First, it 
needs to be in the same folder as crea.exe. Second is that the 
crea/modules/ path must be added to sys.path::

	import sys 
	sys.path.append("modules")
	import siege
	
You can also add this more permanently by adding the full path to 
your PYTHONPATH environment variable.

With the siege module you have full access to all objects that you 
can find in :doc:`the API docs <index>`.

load_crea_content.py
--------------------

Here is a script I use to load Crea content. It will work from any 
directory on Windows, Mac, or Linux. If Crea is not installed with 
Steam, make sure you pass the Crea install directory as an argument 
to the load() function (don't worry about this if you don't know 
what it means)::

	import sys, os, os.path

	def get_crea_dir():
		"""Find the Crea Steam install directory based on current OS"""
		if os.name == 'nt':
			# must run script from the same drive as the Crea install
			return '%s\\Program Files (x86)\\Steam\\steamapps\\common\\Crea' \
					% os.path.splitdrive(os.getcwd())[0]
		elif os.name == 'posix':
			return os.path.expanduser('~/.local/share/Steam/SteamApps/common/Crea')
		else:
			raise OSError('Unsupported OS: %s' % os.name)

	def load(argv=None):
		if argv is None:
			argv = sys.argv
			
		# if a directory is passed, use it as the install directory;
		# otherwise, find the Crea Steam install directory
		if len(argv) > 1 and os.path.exists(argv[1]):
			crea_dir = argv[1]
		else:
			crea_dir = get_crea_dir()

		# switch to the Crea install directory
		os.chdir(crea_dir)

		# add the necessary paths
		sys.path.append(os.getcwd())
		sys.path.append('mods')
		sys.path.append('modules')
		sys.path.append(os.path.join('modules','lib-dynload'))

		# now we can import siege after adding the paths
		import siege
		from siege import game, Locale
		from siege.util import TileVector
		from siege.world import World

		# load content and systems
		game.network.setupPassthrough()
		game.content.discover("mods")
		subsystem.registration(game)
		
		# optionally, create a world so we can initialize entities
		# this also reloads content
		World.create(game, 'test', TileVector(256, 256), 0)
	
		# set up localization
		Locale.setLocale("en", game.content.getPackages().getOrdered())

	if __name__ == '__main__':
		load(sys.argv)

Example usage script
--------------------

Here is an example using the siege module to generate multiple 
worlds, copying the log and map file over into another folder. I use 
this script whenever I'm making changes to world generation such as 
seeing the frequency of treasure chests or placement of Way Crystals::

	import os, sys, shutil, glob
	from load_crea_content import load

	load()

	from siege import game, subsystem
	from siege.log import Log
	from siege.util import TileVector
	from siege.world import World

	if os.path.exists("maps"):
		shutil.rmtree("maps")
		for log in glob.glob("crea.log*"):
			try:
				os.remove(log)
			except WindowsError:
				pass
	os.mkdir("maps")

	if len(sys.argv) > 1:
		amount = int(sys.argv[1])
	else:
		amount = 5
	
	for currentValue in range(amount):
		Log.initialize()
		world = World.create(game, "", TileVector(2048, 1024), 0)
		terraform = game.terraform
		terraform.generate('world', realm=world.realm)
		while terraform.isGenerating():
			terraform.resume()
		World.reset()
		Log.shutdown()
		shutil.move("map.png", os.path.join("maps", "{}.png".format(currentValue)))
		logs = glob.glob("crea.log*")
		for log in logs:
			output = log.replace("crea", str(currentValue))
			shutil.move(log, os.path.join("maps", output))
