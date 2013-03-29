texturecache.py
===============

Utility script to manage the XBMC texture cache.

Usage:

 x) Extract - dump - all or filtered cached textures information. Default
 field separator can be overridden by specifying an alternate value in
 properties file (see below). Filter expression must be valid WHERE clause.
 Option X will extract only those rows that have missing cached artwork.

 s) Search texture cache for specific partial url names, dumping results.
 Option S will return only results for those items that no longer have a
 matching cached artwork.

 d) Delete one or more rows from the texture cache database, along with
 any associated cached artwork.

 r) Walk the Thumbnails folder, identifying those cached items that are no
 longer referenced in the SQL db, with option (R) to automatically purge
 the orphaned files.

 c) Refresh cache with all artwork. Specify one of the following as an
 optional parameter, otherwise all but songs will be refreshed:

      albums, artists, movies, tags, sets, tvshows, songs

 When specifying a class to refresh, a third optional argument can be provided
 to restrict (filter) the movie/show/album etc. that are processed.

 C) Similar to (c), but will remove artwork from the cache to ensure
 artwork is always refreshed. Mandatory class and filter required to limit
 accidental processing, though a filter of ".*" will match everything.

 j) Query media library by class with optional filter, returning JSON results.

 J) As per j), but include additional JSON audio/video query fields as specified
 in the properties file, using comma-delimited lists. Additional fields can have
 a significant impact on performance.

 qa) Locate movies/tags/tvshows with missing artwork, plot, mpaa certificates, that
 have been added within qaperiod days. Add qa.rating=yes to properties file for
 rating property to be included in QA tests.

 qax) Same as (qa), but will perform remove/rescan of media for items that fail QA.

 p/P) Prune Cache: Identify items in the texture cache that don't exist in the media
 library. These items are typically artwork image previews and other non-essential
 assets downloaded by addons that can be safely removed from the texture cache.
 The p option will display the items that could be removed, while the P option will
 perform the same checks but then also physically remove qualifying items from the
 texture cache. Due to the amount of processing involved this is quite a slow process.

 To refresh the cache the Webserver service must be enabled without a
 username or password.

 To allow JSON access, you must enable "Allow programs on this system to control XBMC"
 in System -> Services -> Remote control. Also enable "Allow other programs..." if
 accessing JSON functions of a remote XBMC client (ie. xbmc.host is not localhost).

Properties File:

 A properties file, texturecache.cfg, will be read if found in the same
 folder as this script. It can be used to override the default field
 separator, the location of the XBMC userdata folder, the location and name
 of the Texture db (relative to the XBMC userdata folder), and the Thumbnails
 folder (also relative to the XBMC userdata folder).

 Example texturecache.cfg (showing default values):

   sep = |
   userdata = ~/.xbmc/userdata
   dbfile = Database/Textures13.db
   thumbnails = Thumbnails
   xbmc_host = localhost
   webserver.port = 8080
   webserver.singleshot = no
   rpc_port = 9090
   extrajson.albums  = None
   extrajson.artists = None
   extrajson.songs   = file
   extrajson.movies  = trailer, streamdetails, file
   extrajson.sets    = None
   extrajson.tvshows.tvshow = None
   extrajson.tvshows.season = None
   extrajson.tvshows.episode= streamdetails, file
   qaperiod = 30
   qa.rating = false
   cache.castthumb = false
   cache.ignore.types = image://video, image://music
   logfile = None

Dumped data format:

 rowid, cachedurl, height, width, usecount, lastusetime, lasthashcheck, url
