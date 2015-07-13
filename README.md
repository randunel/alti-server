# alti+server
Altitude game server wrapper to enable enhanced modes of game play.

## Files

  * **README.md**  This file

  * **INSTALL.txt**   A list of the quick steps to install this software
 
  * **UNLICENSE.txt**  This software is in the Public Domain, this file
    states this

  * **alti+server**  The perl code which starts the altitude game engine
    and monitors it for events

  * **custom_json_commands.txt**  The custom commands which allow people to
    interact with the server

  * **small_custom_json_commands.txt**  A smaller version of the custom
    commands which don't help users but allow for more maps to be loaded.

## Alti+ game types

### Normal Games
All normal game types will run just as normal on `alti+server` systems.
They can be run with all default values, or spiced up by changing
camera or plane scale, gravity, etc.  It is up to the server admins.

### Capture the Flag
There are number of options for game types which are different from what
you would normally see. Capture the flag games have a powerup which is
located in an area heavlily protected by one team. The power should only
be accessible to the other team. The server game mode must be "Ball",
as each capture increases the goal count. One variety of Flag capture
is a 1-life version, where teams go head-to-head and is very similar to
1dm. Another allows re-spawns and is timed. This mode type should favor
the defensive team, and a single capture (goal) should be sufficient for
winning. Capture the flag games can also be configured to allow both teams
to score. Hear, the game would be similar to tbd, but withou a bomb to
pickup. This would hopefully get teams to work together better, instead
of people grabbing the bomb and flying off by themselves without support.

### Races
Races use specially assigne powerups to act as checkpoints.  Each time
the first player on a team crosses a checkpoint, a goal is scored.  For
this reason, the map must be configured for game type *ball*.  Most servers
are setup to have 6 goal win.  Races are best accomplished with 2 laps and
3 checkpoints, with players starting so that they cannot easilly access
the winning (6th) checkpoint until the end of their first lap (starting in
a pit row might work).

A to B races would also be easy, with checkpoints along the way, and the
6th checkpoint at the end.  If a final (winning) checkpoint is all that
is desired in an A to B, then setting `/set ballScore -1 -1` would also
help accomplish this game type.

### Large Map Coops
Many new spawn options are available with the `alti+server`. Players
can be configured to spawn where they last died, to make continuation
easier. If that is too immidiate, players can spawn at the place of their
last pickup, or health. Coops can also be made more exciting by the use
of devices like portals, to teleport players from one area to another.

## Additional Commands
The below list of commands are available to players to interface with and
manage Alti+ game maps.

### Extended Commands for Players

  * **/switch**<br>
    This command is useful for Coop maps, it allows users who are assigned
    to the wrong team to spectate and choose the other side.  This command
    should usually not be needed, as the correct side should be set by the
    server or map maker with the `/set assignTeam [left|right]` command.

  * **/spawnZone [zone]**<br>
    When a player has a non-standard spawn zone, they can reset their spawn
    zone to the original map default by running `/spawnZone start`.
    Otherwise, this command works only if the server is in zone spawning
    mode. It allows players to choose which `zone` they will respawn into
    when they die. Also, when players execute this command, they will
    automatically be removed from the map and a new spawn will be initiated
    in `zone`. All bars and powerups will be lost.

  * **/sz [zone]**<br>
    This is an alias for `/spawnZone`

  * **/restartMap**<br>
    This command is a front-end to `/balanceTeams`. If you are the only
    person on a map, it will balanceTeams without the need for a vote. If
    you are on a 1de, 1dm, etc. map and are one of the last two players
    still alive, you can run `/restartMap` and his will `/balanceTeams`
    without a vote.

  * **/mesg [status]**<br>
    The server has a private messaging feature designed to aid players in
    meeting goals or to talk privately with the map designer. Howerver, to
    ensure this feature is not abused, this feature can be disabled via
    this command and it's argument.
    `status` can be one of `Yes`, `No`, or `?` to
    turn messaging on, off, or query your message setting respectively.
    This setting is persistent.

  * **/talk [player] [text ...]**<br>
    Send a private message to `player`. You do not have to be friends
    with `player` to do so, but they can disable messaging with the
    `/mesg No` command. Abusers will be banned.

### Extended Commands for Admins and Map makers

  * **/sum [crc32|md5|sha1|sha256]**<br>
    Generate a checksum (message digest) of the *.altx* map file for the
    current map.  The purpose of this command is to help map makers verify
    the current map is the right version.

  * **/logPickups**<br>
    Display the *X* and *Y* coordinates to the screen for each `powerupPickup`
    or `powerupAutoUse` for your plane.  The purpose of this command is to
    help identify powerups for use with *portals*, *doors*, and different
    spawn types.  These are defined more below.

  * **/location**<br>
    Display the *X* and *Y* coordinates for the plane where it is about to
    The purpose of this command is to help identify locations for spawn
    points for for use with *portals*, *zones*, etc.
    These are defined more below.

  * **/help [setting]**<br>
    Display a help message for `setting`.  If a `setting` has a
    `/list`, `/set`, `/unset`, `/add`, `/del`, or `/upload`
    command associated with it, then it will also have a `/help` command
    to provide more detail on what is required.

  * **/list [setting]**<br>
    Display the current value or values for `setting`.  For a list of
    settings, see the `/set` and `/add` commands below.

  * **/unset [setting]**<br>
    Remove current value or values for `setting` and restore them to the
    default behaviour.  For a list of
    settings, see the `/set` commands below.

  * **/set ballScore [left] [right]**<br>
    Set the initial ball score for *ball* mode games to `left` and `right`.
    These values must be integers.  If they are negative, then that value
    will be subtracted from the winning score value.

  * **/set assignTeam [left|right|none]**<br>
    Specify that players must join either the `left` or `right` team,
    depending on this setting.  You can also use `<` and `>` as
    shorthand for `left` and `right` respectively.  This feature is most
    useful in Coop game types, where every player should be on one team.
    Setting this value to `none` is equivalent to ``/unset assignTeam``,
    the setting will be removed and the server returned to its default
    behaviour.

  * **/set planes [Loopy|Bomber|Explodet|Biplane|Miranda] [...]**<br>
    Specify that only the list of planes provided here are allowed on this
    map.  When players attempt to spawn with the wrong plane, they will
    be benched and the server will display a message explaining the valid
    plane list.  Nicknames whale and randa are also allowed.

  * **/set gravity [0|1|2|3]**<br>
    Set the default gravity configuration for this map. This setting will
    be kept between server restarts for this map. If this map is removed
    from the server, the setting will be lost. This command can only be
    used to update the current map in-use. The numeric arguments correspond
    to the argument for the `/testGravityMode`.

  * **/set cameraScale [40-300]**<br>
    Set the camera scale (zoom) level from anywhere between `40` and `300`.
    The default is `100`%, scale up or down from there.
    This setting will automatically call `/testCameraViewScale` to set the
    current scale, and it will update the server to make this setting each
    time the current map is loaded in the future. This setting will persist
    across server restarts. To set it back to the default of `100`,
    you can `/unset cameraScale` or `/set cameraScale 100`.


  * **/set planeScale [40-300]**<br>
    Set the plane scale level from anywhere between `40` and `300`.
    The default is `100`%, scale up or down from there.
    This setting will automatically call `/testPlaneScale` to set the
    current scale, and it will update the server to make this setting
    each time the current map is loaded in the future. This setting will
    persist across server restarts. To set it back, you can
    `/unset planeScale` or `/set planeScale 100`.

  * **/set spawnType [died|powerup|health|zone|near|portal|normal]**<br>
    This specifies the type of respawning when a user dies, or requests
    to spawn. The type can be any of the following:
    *died* (the location of last death),
    *powerup* (the location of their last powerup of any kind),
    *health* (the location of their last health powerup),
    *zone* (zone based spawning where uses can cange their spawn zone on demand),
    *near* (nearest zone to a player when they die),
    *portal* (pre-defined specified health powerups cause a player to immediately respawn at a defined location),
    or *normal* (at the normal, map defined start).
    If users get into a bind, they can use `/sz start` to spawn in the
    normal location.

  * **/set gameType [coop|flag|race|zone|std|normal]**<br>
    Normally the type of a game is ascertained by te file name.
    If that cannot be done properly, the game type can be set via this
    mechanism.

  * **/add portal [name] [px],[py] [sx],[sy],[sa]**<br>
    When a player auto-uses the health powerup located at `px`,`py`, then
    they are automatically transported to `sx`,`sy` and spawn at the
    angle `sa`.  See Angles for more information.

  * **/del portal [name]**<br>
    Delete the *portal* named `name` for this map.

  * **/add zone [name] [team] [sx],[sy],[sa]**<br>
    Create (or overwrite) a spawn zone named `name`. When invoked, spawn
    users at coordinates `sx`,`sy` and at angle `sa`.
    Players must use `/spawnZone name` to change their spawn zone to this
    location when *spawnMode* is set to `zone`.  Players will automatically
    spawn at the closest zone to their death when *spawnMode* is set to
    `near`.  See Angles for more information on angle specifications.

  * **/del zone [name] [team]**<br>
    Delete the *zone* named `name` for this map for `team`.  You can specify
    `left`, `right`, `<`, `>`, or `*` for the team.  `*` means both
    teams.

  * **/add powerup [name] [team] [x] [y]**<br>
    Specify that a special powerup for `team` should be at the coordinates
    `x`,`y`. If this is picked up by the correct `team`, then that
    `team` the behaviour is defined based on *gameType*. `team` can be one of
    `left`, `right`, `<`, `>`, or `*` (for both teams).  For exampe, if
    *gameType* is set to `flag`, then the team which picks up the powerup
    earns a goal, and the players respawn.

  * **/del powerup [name] [team]**<br>
    Delete the special powerup `name` for `team`.

### Extended Commands for Super Admins
Super admin functionality is set in the `alti+server` code itself.  The
`superAdmin` setting can be set to either
`admin` (all admins can perform super admin functionality),
`console` (super admin functionality is limited to the console), and
`list` (where an array `@SUPER_ADMINS` contains a list of vaporIds which
are allowed to execute super admin functionality.  The purpose of the
distinction between normal Altitude admin functionality and Super Admin
functionality is that super admins can change the maps on a server and restart
it.  There may be users who are admins to set ban and kick, and can also
`set assignTeam` to keep the game going, but should have access to restart
the server.

  * **/restartServer**<br>
    Restart the server. This will shut the currently running server down,
    and start a new one. This should be used to load new maps or to re-load
    the configuration after a map has been deleted. If an update to an
    existing map is made, the server does not need to be restarted.

  * **/removeMap [map]**<br>
    Remove the map named `map`. The map file will be removed from the server,
    and it will also be removed from the maplist and rotation.
    This will not be fully implemented until the server is restarted,
    which should always be done after removing a `map`.

  * **/upload altitudegame [map]**<br>
    Upload the map `map` FROM http://altitudegame.com/map/ and save it on
    the server in the maps directory. The name `map` must be exacly the
    same as the linkable text of the altitude game map site.
    The most recent version of the map will be uploaded to the server
    from altitudegame.com. If the uploaded map is not currently listed in
    the server configuration file, it will be added. However, the server
    must be restarted for the map to be added to the maplist and rotation.

  * **/upload ladder [map]**<br>
    Upload the map `map` FROM http://altituderank.com/ and save it to
    the server in the maps directory. The name `map` must be exacly the
    same as the name of the file but without the *.altx* extension.
    If the uploaded map is not currently listed in the server configuration
    file, it will be added. However, the server must be restarted for the
    map to be added to the maplist and rotation.

  * **/upload tinyupload [key]**<br>
    Upload the map stored on http://s000.tinyupload.com/ with the *file_id*
    of `key` and save it to the server in the maps directory.  The key
    should be a 20 digit decimal number, you do not need to provide the
    rest of the URL.  Get the map name from the tinyupload website, and
    name the map in the maps directory from that scraped web content.
    If the uploaded map is not currently listed in the server configuration
    file, it will be added. However, the server must be restarted for the
    map to be added to the maplist and rotation.

  * **/upload url [map] [url]**<br>
    Upload a map to the server FROM `url` and save it as `map`.
    You do not need to specify an extension for the map, nor do you need
    to specify a location. The map will be stored in the correct location
    under the maps directory on the server. The `url` given may be an
    HTTP redirect, tinyurl, etc. The `url` given cannot require user
    interaction, or contain a javascript based redirect. If you attempt
    to upload a file from a location with a javascript URL, the HTML
    containing the javascript will instead be uploaded. This will be detected,
    and the upload will be aborted. If the uploaded map is not currently
    listed in the server configuration file, it will be added. However,
    the server must be restarted for the map to be added to the
    maplist and rotation.

### Console only commands

  * **/showPlayer [player]**<br>
    Print information on `player`

  * **/shutdown**<br>
    Shutdown the Altitude game engine, then `alti+server` itself


## Angles
Various interfaces in `alti+server` allow you to specify angles for
spawning.  When specifying angles, the following table lists the
allowed values:

  * **0 to 80**<br>
    The angle in degrees starting from 0(facing right) to 180(facing left)

  * **-1 to -179**<br>
    The downward facing angles in degrees starting from -1 (very nearly
    facing right) to -179 (very nearly facing left). The angle of -90 is
    straight down.

  * **181 to 359**<br>
    The downward facing angles in degrees. These numbers are converted
    automatically to the correct -1 to -179 values where 181 is equivalent
    to -179, 270 is equivalent to -90, and 359 is equivalent to -1.

  * **right|up|left|down**<br>
    These directions are converted to 0, 90, 180, and -90 respectively.

  * **e|ne|n|nw|w|sw|s|se**<br>
    Compass directions which are converted to
    0, 45, 90, 135, 180, -135, -90, and -45 respectively.

## Custom Awards

  * **Training Wheels Award**<br>
    This award goes to the player who crashes the most. 

  * **Kenny McCormick Award**<br>
    This award goes to the player who dies the most, named after
    South Park character who can't make it to the end of an episode.

  * **Ball Hog Award**<br>
    This award goes to the player who holds onto the ball the most.
    Though slightly derogative souding, this can be a compliement.

  * **Sledge Hammer Award**<br>
    This award is named after the 1980's TV character who shot first and
    didn't bother to ask questions. To paraphrase:
    guns don't kill people, bullets kill people.
    This is for most damage dealt.

  * **Tank Award**<br>
    This award goes to the player who manages to sustain the most damage.
    Every team needs a good tank.

  * **Best Ratio**<br>
    This award is given to the player who has the best final ratio
    of Kills to Deaths. Players late to the game may scoop this award.

  * **Front Runner**<br>
    This award is given to the player who has is the first person on
    their team to get to the most checkpoints.  This award is designed for
    a game type of `race`.

## Snarky Server
The server can be configured to emit snarky comments from time to time by
setting the code OPTION `snarkChance` to a positive value.  Setting this
value to 20 (for example) should give each player a 1 in 20 change for
receiving a snarky comment for things for which a comment like this may
occur.  The most commong snarky comments are for plane configuraion choices
and occur when a plan spawns.  So, if a `snarkChance` is set to 20 and a
player dies 20 times in one game, it is likely that they will receive a
snarky comment (if their plane choices have a preconfigured comment).

Setting `snarkChance` to 0 disables this feature, and no snarky comments will
be emitted by the server.

## Minimum Level Requirements
If the `requireLevel` OPTION is set, then the server will not allow players
to spawn who don't meet the minimum requirements.

New players (Ace 0) must unlock all their planes (level 21) to join a game.

Recently aced players (Ace 1 - Ace 10) must unlock all their first loopy
blue perk (level 8).

## Managing Aggressive Behaviour
The server can be configured with the `bullyKick` OPTION to warn, then
eventually kick players who behave in overly aggressive ways towards other
players.  Language which is often used in offensive statements is looked
for, and after 10 utterances, the aggressive player is kicked.

## More information
See the MAP QA website for more information on using the server:
http://mapqa.glaciated.org/
