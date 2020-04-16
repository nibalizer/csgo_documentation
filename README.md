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
// network information;

net_graph 1;

// radar hax;

cl_radar_always_centered 0;
cl_radar_scale 0.3;
cl_hud_radar_scale 1.15;
cl_radar_icon_scale_min 1;
cl_radar_rotate 1;
cl_radar_square_with_scoreboard 1;

// 1337 reticule;


cl_crosshairalpha "255";
cl_crosshaircolor "5";
cl_crosshaircolor_b "255";
cl_crosshaircolor_r "255";
cl_crosshaircolor_g "255";
cl_crosshairdot "0";
cl_crosshairgap "-1";
cl_crosshairsize "4";
cl_crosshairstyle "5";
cl_crosshairusealpha "1";
cl_crosshairthickness "1.4";
cl_fixedcrosshairgap "0";
cl_crosshair_drawoutline "1";
cl_crosshair_outlinethickness "1";

// misc
alias f9 "kill"


// audio;

alias slam_play_on "voice_inputfromfile 1; voice_loopback 1; +voicerecord";
alias slam_play_off "-voicerecord; voice_inputfromfile 0; voice_loopback 0";

// outro;

host_writeconfig;
```

[References](https://gist.github.com/KiloSwiss/a015b0620284ce74b5ed849ec599e51e)
[References](https://www.tobyscs.com/csgo-console-commands/)


Launch options
--------------


CSGO can be configured with launch options. These generally come in two flavors: dash-commands and plus-commands. Dash commands modify the behavior of the binary, plus-commands are passed to the csgo console.

Modify launch commands in Steam: Right click csgo, go to properties, click "Set Launch Options"

Current launch options: "-nojoy -console -novid -high +exec autoexec"

Console launches the console at game start. Novid disables valve/steam videos on start. HIgh enables more cpu time. +exec autoexec is telling csgo to read and process the autoexec file. (From research, this shouldn't really be necessary but it is on my system). Nojoy fixes a bug on ubuntu 18.04.



Playing Demos
-------------

CS:GO has a replay functionality called 'demos'. They can be played from th e console. Record and stop recording are manual tasks, not tied to the game score, etc. Demo files pulled from the internet often start several minutes before the real game begins.

Relative (to where?) and absoulte paths both work.

```
playdemo somedemofile.dem
playdemo C:\Users\csgo2\Desktop\somedemo.dem
```

Demo files are ship usually as ".rar" files. They can be extraced with both Linux and Windows unrar programs.

Demo speed playback can be modified with the following command (1.0 is real time):

```
demo_timescale 1.0
demo_ timescale 4.0
```



game state integration
----------------------


Valve game state integration is an api for reading live data from the game.

Config file: "C/Program File (x86)/Steam/steamapps/common/Counter-Strike Global Offensive/csgo/cfg"


The file name must start with "gamestate_integration" and end with ".cfg". Make sure the file is actually a ".cfg" file and not a ".cfg.txt" file ( windows extensions are hidden by default).

Example filename: "gamestate_integration_testnibz1.cfg"

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
   "allgrenades"    "1"           // output information about all grenades and inferno flames in the world, only valid for GOTV or spectators.
 }
}
```

Source: https://developer.valvesoftware.com/wiki/Counter-Strike:_Global_Offensive_Game_State_Integration



remote console
--------------



A remote console can be opened up and then connected to via telnet.

https://developer.valvesoftware.com/wiki/Command_Line_Options#Linux_command_options_in_Left_4_Dead_.282.29


```
-netconport 2121
```


```
ubuntu@csgo-server:~$ telnet localhost 2121
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
hi
Unknown command "hi"
echo hi
hi 
```


Example of using telnet to echo a sentence then play a demo file:

```
echo Playing demo file
playdemo C:\Users\csgo2\Desktop\cloud9-vs-renegades-train.dem
```



installing telnet on windows
-----------------------------


https://technet.microsoft.com/en-us/library/cc771275(v=ws.10).aspx



steamcmd
---------

A command line interface to steam... derpy.

https://developer.valvesoftware.com/wiki/SteamCMD

Installing csgo (csgo is 730, csgo_server is 740):

```shell
mkdir ~/cs_go
force_install_dir ./cs_go/
app_update 730 validate
```



camera and observer
-------------------

The camera can be moved around as an observer. Each number on the top of the keyboard jumps you to POV of a different player.

## campaths

Camera paths or "campaths" are scripted camera movements used to pop out of the POV and add spice to a broadcast. [video tutorial](https://www.youtube.com/watch?v=8T3rNL0rnW4&feature=youtu.be)

```
spec_pos
<some pos>
#move somehwere
spec_pos
<some pos2>
# the actual campath
spec_goto <some pos>; spec_lerpto <some pos2> 15 15
# or bind it
bind [ "spec_goto <some pos>; spec_lerpto <some pos2> 15 15"
```

[Observing Resources](https://www.reddit.com/r/GlobalOffensive/comments/54abps/my_guide_for_solo_casting_csgo/)






voting
------


With the panorama ui, they limited the votes you can call. However you can still call them via the console.

```
callvote Kick <userID>
callvote RestartGame
callvote NextLevel <mapname>
callvote ChangeLevel <mapname>
callvote StartTimeOut
callvote ScrambleTeams
callvote SwapTeams
```


playback through voice chat
---------------------------

You can play any audio (e.g. music) through the voice chat in-game.

The main entry point is the `voice_inputfromfile` command.

```
voice_inputfromfile

Default: 0
Get voice input from 'voice_input.wav' rather than from the microphone.
```

Useful for autoexec:

```
alias slam_play_on "voice_inputfromfile 1; voice_loopback 1; +voicerecord";
alias slam_play_off "-voicerecord; voice_inputfromfile 0; voice_loopback 0";
```

For the sound files: encode at 96k, 22050hz sample rate

See: https://github.com/SilentSys/SLAM

Put the file in the same directory as csgo.exe (Program Files (x86) -> Steam -> steamapps -> common -> Counter Strike Global Offensive)

