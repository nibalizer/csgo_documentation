CSGO Docs
=========


Counter-Strike Global Offensive has many extensibility and integration features. Unfortunately these aren't documented in any one place, and the documentation that does exist leaves out details around getting them set up. This document is one (more) place with documentation on various elements. The intended audience is developers working with csgo.


console
-------


CSGO has a 'developer console' that can be used to query the game or configure the ui. It can be enabled in game settings, then usually the '`' key will bring it up.


autoexec
--------

An autoexec.cfg file is a list of console commands that will be run whenever csgo starts. Common uses for this are configuring the reticule (crosshairs), configuring minimap (radar), configuring the client to show fps/network information, and setting aliases. "Jumpthrow" and other aliases are configured via this. (jumpthrow has since been banned in competitive play)


Autoexec path: "/C/Program Files (x86)/Steam/userdata/<my-steam-id-number/730/local/cfg"

Autoexec contents:

```
// network information
net_graph 1

// radar hax
cl_radar_always_centered 0
cl_radar_scale 0.3
// radar zoom in-out script
bind ???KP_plus??? ???incrementvar cl_radar_scale 0.25 1.0 0.05???;
bind ???KP_minus??? ???incrementvar cl_radar_scale 0.25 1.0 -0.05???;
cl_hud_radar_scale 1.15
cl_radar_icon_scale_min 1
cl_radar_rotate 1
cl_radar_square_with_scoreboard 1

// 1337 reticule
cl_crosshairalpha "255"
cl_crosshaircolor "5"
cl_crosshaircolor_b "255"
cl_crosshaircolor_r "255"
cl_crosshaircolor_g "255"
cl_crosshairdot "0"
cl_crosshairgap "-1"
cl_crosshairsize "4"
cl_crosshairstyle "5"
cl_crosshairusealpha "1"
cl_crosshairthickness "1.4"
cl_fixedcrosshairgap "0"
cl_crosshair_drawoutline "1"
cl_crosshair_outlinethickness "1"

// outro
host_writeconfig
```


Launch options
--------------


CSGO can be configured with launch options. These generally come in two flavors: dash-commands and plus-commands. Dash commands modify the behavior of the binary, plus-commands are passed to the csgo console.

Modify launch commands in Steam: Right click csgo, go to properties, click "Set Launch Options"

Current launch options: "-console -novid - high +exec autoexec"

Console launches the console at game start. Novid disables valve/steam videos on start. HIgh enables more cpu time. +exec autoexec is telling csgo to read and process the autoexec file. (From research, this shouldn't really be necessary but it is on my system)


game state integration
----------------------


Valve game state integration is an api for reading live data from the game.

Config file: "C/Program File (x86)/Steam/steamapps/common/Counter-Strike Global Offensive/csgo/cfg"


Example file:


```
"Observer All Players v.1"
{
 "uri" "http://192.168.0.5:3000"
 "timeout" "5.0"
 "buffer"  "0.1"
 "throttle" "0.1"
 "heartbeat" "30.0"
 "auth"
 {
   "token" "REDACTED"
 }
 "data"
 {
   "provider"            "1"
   "map"                 "1"
   "round"               "1"
   "player_id"           "1"
   "allplayers_id"       "1"      // Same as 'player_id' but for all players. 'allplayers' versions are only valid for HLTV and observers
   "player_state"        "1"      
   "allplayers_state"    "1"      
   "allplayers_match_stats"  "1"  
   "allplayers_weapons"  "1"      
   "allplayers_position" "1"      // output the player world positions, only valid for HLTV or spectators. 
   "phase_countdowns"    "1"      // countdowns of each second remaining for game phases, eg round time left, time until bomb explode, freezetime. Only valid for HLTV or spectators. 
 }
}
```


