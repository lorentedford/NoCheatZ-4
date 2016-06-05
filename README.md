# NoCheatZ-4
Source Engine serversided anti-cheat plugin. (CS:S, CS:GO).
___

# Table of content
___

1. [Goal](#Goal)
2. [Current Project Status](#Status)
3. [Features](#Features)
    1. [Systems](#Systems)
        1. [Detection Systems](#detection-systems)
        2. [Blocking Systems](#blocking-systems)
        3. [Other Systems](#other-systems)
    2. [Available Command](#command)
4. [Contributing](#Contributing)
    1. [Installing a Game Server](#Installation)
    2. [Cloning the repository](#Cloning)
    3. [Compiling the project](#Compiling)
        1. [With Linux](#Linux)
        2. [With Windows](#Windows)
        3. [With Mac](#Mac)
    4. [What to do with the code ?](#what-to-do)

<a name="Goal"></a> 
# Goal
___
The goal is to make an anti-cheat that can be used with any type of gameservers.


* Because it can be used in competition it must be fast, consistent and safe.

* And because it can be used with others servers it must also be configurable, easy to use and open-source.


Supported games will be [CS:S](http://store.steampowered.com/app/240), [CS:GO](http://store.steampowered.com/app/730), [CS:P](http://cspromod.com/), [Black Mesa](http://store.steampowered.com/app/362890) and maybe more.

Currently [CS:S](http://store.steampowered.com/app/240) is the base of this project.

<a name="Status"></a> 
# Current Project Status
___

* **Status** : [Testing / Fixing](https://github.com/L-EARN/NoCheatZ-4/issues)

<a name="Features"></a> 
# Features
___

<a name="Systems"></a> 
## Systems
___

<a name="detection-systems"></a> 
### Detection Systems
___

* ConCommandTester : Test and block player commands for common exploits on Source Engine servers such as rcon password stealing.
* ConVarTester : Test and detect players using old ConVar bypassers. **Can link the value to the server if the ConVar exists on the server**
* EyeAnglesTester : Provides good detection against badly coded aimbots and spinhacks.
* JumpTester : Detects bunnyhop programs and scripts (**Is able to make the difference**)
* ShotTester : Detects crosshair triggerbots and robotic shot button spam.
* SpamChangeNameTester : Detects when a players changes his name too often (_Is now fixed internally by the Source Engine_)
* SpamConnectTester : Detects when a client is flooding the connection (Trying to connect and disconnect too much)
* SpeedTester : Detects SpeedHacks using CUserCmd and tickrate ratio.
* ValidationTester : Forces a client to be validated by Steam before being able to join the game.

<a name="blocking-systems"></a> 
### Blocking Systems
___

* AntiFlashbangBlocker : Send a extra white screen and stop transmit to fully blind players.
* AntiSmokeBlocker : Players inside a smoke cannot see others. **Players can't see thru smokes even when r_drawparticles is 0**.
* BadUserCmdBlocker : Reject malformed or cheated CUserCmds.
* WallhackBlocker : Player visibility is tested against the walls.
* RadarHackBlocker : Block aimbots and ESPs that use the radar positions.

<a name="other-systems"></a> 
### Other Systems

* AutoTVRecord : Automatically spawn the SourceTV. Automatically record the game when at least one huan player is in game (*Rather than record when the server is empty ...*). **Detection logs also store the tick of the current record for easier reviewing**.
* BanRequest : Some bans are delayed by 20 seconds to try to detect more things.
* ConfigManager : Store informations about the different games the plugin can be used for ([config.ini](https://github.com/L-EARN/NoCheatZ-4/blob/master/server-plugin/Res/config.ini))
* Logger : Outputs NoCheatZ logs into `logs/NoCheatZ_4_Logs/`

<a name="command"></a> 
## Available command
___

`ncz [system] [args ...]`

* Print status of systems : `ncz`
* You can disable a system using `ncz [system] disable` or `ncz [system] off`
* Enable a system using `ncz [system] enable` or `ncz [system] on`
* Print additionnal informations if available (**Will slow down the server**) `ncz [system] verbose 1`
* Print more informations if available (**Will slow down the server**) `ncz [system] verbose 2`
* Stop that verbose thingy that slow down the server for nothing `ncz [system] verbose 0`
* Add a ConVar to be tested by ConVarTester : `ncz ConVarTester AddRule sv_cheats 0 SAME_AS_SERVER` means the clients must have the same value as the server
* Reset default rules of ConVarTester : `ncz ConVarTester ResetRules`

<a name="Contributing"></a> 
# Contributing
___

<a name="Installation"></a> 
## Installing a Game Server
___
See https://developer.valvesoftware.com/wiki/SteamCMD

* It is recommanded, with Windows, to install your server into his own directory.
The Visual Studio project launches a commandfile (to auto-update the server before running the debugger) that expects steamcmd to be installed at C:\steamcmd\steamcmd.exe

* For Linux, please make a new user using adduser. You will also have to copy the files manually to your server after each builds.

```bash
login anonymous
force_install_dir C:\steamcmd\CSS
app_update 232330 validate
```

<a name="Cloning"></a> 
## Cloning the repository
___

`su` to your srcds user.

```git
git clone git://github.com/L-EARN/NoCheatZ-4.git
cd NoCheatZ-4
git submodule update --init
```

<a name="Compiling"></a> 
## Compiling the project
___

<a name="Linux"></a> 
### With Linux
___

1. `su` to root.

2. Install your compiler tools :

```sh
dpkg --add-architecture i386
apt-get update
apt-get install build-essential gcc-multilib g++-multilib ia32-libs lib32gcc1 libc6-i386 libc6-dev-i386 autotools-dev autoconf libtool gdb screen
```

3. `su` to your srcds user.

4. With your console, go to the `server-plugin` directory and type `make debug`

The binary and all other stuff will go in `NoCheatZ-4/Builds/[CONFIG]/addons/NoCheatZ`

You can copy-paste the entire content of the `Builds` directory into the root directory of your gameserver.
(e.g. `cp /R ./NoCheatZ-4/Builds/Release/* /home/user/steamcmd/CSS/cstrike` )

5. If you want to run the server using gdb, create a script in `steamcmd/CSS` with something like this :

```sh
#!/bin/sh

export LD_LIBRARY_PATH=".:bin:$LD_LIBRARY_PATH"
gdb --args ./srcds_linux -console -game cstrike -steam_dir ../ -steamcmd_script ../steamcmd.sh -insecure +map de_dust2 +rcon_password cderfv
```

<a name="Windows"></a> 
### With Windows
___

1. Make sure you installed steamcmd at `C:\steamcmd\steamcmd.exe`

2. Download and extract [libprotobuf src 2.5.0](https://github.com/google/protobuf/archive/v2.5.0.zip) in server-plugin\SourceSdk\Interfaces\Protobuf\

3. Open the solution with Visual Studio 2015

4. Choose your configuration.

5. Click build. 
         
<a name="Mac"></a> 
### With Mac
___

* You need to setup the makefile to use Clang, and also implement parts of code in the project that are not already translated for OSX.

> I don't eat apples so I can't really do it myself ...

<a name="what-to-do"></a> 
## What to do with the code ?
___

* Whatever you want but first make sure to have your own branch a/o fork.

* Feel free to report using [Issues](https://github.com/L-EARN/NoCheatZ-4/issues) if you see anything you think is not good.
