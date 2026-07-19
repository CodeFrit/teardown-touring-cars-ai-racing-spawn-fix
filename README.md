# Teardown Touring Cars (AI Racing) - Spawn Fix

An unofficial compatibility fix for the Teardown Workshop mod **"Teardown Touring Cars (AI Racing)"**.

This repository contains a modified version of AVF\_AI\_V3.lua that fixes the spawning issues affecting the original mod.

## 

## Installation

1. Locate your installed copy of **Teardown Touring Cars (AI Racing)**.



   Default location:

   Steam\\steamapps\\workshop\\content\\1167630\\2668638370\\



2. Go to the scripts folder
3. Find the original AVF\_AI\_V3.lua file.
4. Make a backup of the original file.
5. Replace it with the fixed version from this repository.
6. Launch Teardown, navigate to the mod manager, select the mod and start a race.

## 

## What this fixes

This patch fixes spawning problems reported by users, including:

* Spawning outside of the vehicle.
* Spawning under the map / in invalid locations.
* Races failing to start correctly due to spawning issues.

Only AVF\_AI\_V3.lua has been modified.

## 

## Why this exists

I originally encountered the same spawning issue while trying to use this mod.

After investigating the Lua scripts, I traced the problem to AVF\_AI\_V3.lua and created a small fix to restore proper spawning behavior.

This patch is provided so players can continue using the original mod without encountering the same issue.

## 

## Investigation

#### Reproducing the Issue

The issue was reproduced by selecting the mod through the game's mod manager and testing many of the available maps within it. The spawning issue occurred consistently during these tests, confirming that the behavior was not limited to a specific map or isolated scenario.

Further testing showed that the issue was limited to the vehicle spawning behavior. When the vehicle was entered into manually, the mod functioned as expected, and remained playable despite the spawning failure.

#### Reviewing the Code

With the scope of the issue identified, I referenced the Teardwon modding API documentation to identify functions related to player position transformation. Using this information, I searched the project's Lua scripts for relevant references and identified a call to setPlayerVehicle(). This function became the starting point for tracing the spawning logic and determining where the failure was occurring.

#### Forming a Hypothesis

Since the mod functioned correctly in earlier game versions, I suspected the issue was caused by a change in the game's initialization order rather than the spawning logic itself. My initial hypothesis was that newer game versions seem to now return a handle to the vehicle before the underlying vehicle is fully initialized, whereas earlier versions appeared to block until the vehicle was ready.

To test this hypothesis, I deferred the execution of the setPlayerVehicle() call until the vehicle was ready, allowing me to determine whether the timing of the API call was responsible for the spawning failure.

#### Implementing the Fix

The deferred approach resolved the issue. The setPlayerVehicle() call was moved into the tick() function, where a lightweight hook periodically checked whether the vehicle referenced by the provided handle had become available.

Once the vehicle was successfully initialized, the player was placed into it using setPlayerVehicle(). After completing this action, the hook automatically disabled itself, preventing any unnecessary checks from continuing during gameplay and preserving performance.



## 

## Credits

Original mod:
**Teardown Touring Cars (AI Racing)** - [https://steamcommunity.com/sharedfiles/filedetails/?id=2668638370](https://steamcommunity.com/sharedfiles/filedetails/?id=2668638370)



Original creator:
**el Boydo**



This is an unofficial community-made fix.

The original mod and its code belong to their respective creator(s). This repository only provides a compatibility fix for AVF\_AI\_V3.lua

## 

## Notice

If this patch helped you, consider leaving a ⭐ on this repository. It helps others find the fix and shows that the work is useful.

Please note that this is not an actively maintained project. I created this patch to fix the current spawning issue, but I do not plan to provide ongoing updates or actively support future changes to the original mod.

If the original mod is updated in the future, this patch may no longer be needed.

