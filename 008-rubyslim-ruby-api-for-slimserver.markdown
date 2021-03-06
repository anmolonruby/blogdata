--- 
:format: :markdown
:title: RubySlim - Ruby API for SlimServer
:published_on: Mon Jul 03 11:15:00 UTC 2006
---
Just a small announcement for any other Squeezebox/Soundbridge owners out there who use the [SlimServer](http://www.slimdevices.com) music server. I've started work on a Ruby API for it and you can view the latest source on the [Agile Evolved Open Source Trac](http://opensource.agileevolved.com/trac/browser/rubyslim/trunk).

You can also check out the latest source code from [Subversion](http://opensource.agileevolved.com/svn/root/rubyslim/trunk).

Here's a small code example:

	slimserver = RubySlim::SlimServer.open('localhost')
	slimserver.connect('myusername', 'mypassword')
	puts slimserver.version

	squeezebox = slimserver.players.first
	squeezebox.turn_on
	squeezebox.current_playlist.play

And in the future...a full RubyOnRails, AJAX-enabled front-end to the SlimServer for browsing your music collection, creating playlists, controlling your SqueezeBox and more. I've not checked anything in on this but I've already been experimenting with creating ActiveRecord models that interact with the SlimServer database. You could do something like this (this is actual code that I have got working):

	my_favorite_album = Album.find_by_title('The Number Of The Beast')
	squeezebox.current_playlist.load :album => my_favorite_album.id

Finally, if you have access to the SlimServer CLI API (click on technical information on your SlimServer web interface menu) you can play with any parts of the CLI API that I'm yet to add to the Ruby API by using the raw connection and API object:

	# make sure you are using the CLI port, not the web interface port
	connection = RubySlim::SlimServer.raw_connection('localhost', 9090)
	slimapi = RubySlim::SlimAPI.new(connection)

	# invoke any api commands
	slimapi.invoke("version")
	slimapi.invoke("myplayerid player status")

For those interested in the concept of [behaviour-driven development](http://behaviour-driven.org/), the whole thing is specced using [rSpec](http://rspec.rubyforge.org). Admittedly its not the greatest exercise in BDD because I'm simply wrapping an existing API in a Ruby API so there wasn't more than 1 context for most classes, but it was still great to use and the built-in mock objects API -- which seems very similar to [FlexMock](http://onestepback.org/software/flexmock) -- was very easy to get to grips with - great for mocking out those telnet connections to the SlimServer CLI.

**Update:** RubySlim is now hosted as a [RubyForge project](http://rubyforge.org/projects/rubyslim/). The 0.1 release is available as a gem.

 	$ gem install rubyslim