# steam-compatdata-symlinker-thing

## What is this?
This is a simple BASH script that creates human-readable symlinks for compatdata directories. This makes it super easy to find the right compatdata directory, without having to look up the game's ID first.

This script also reports broken symlinks, and unused common, compatdata, and shadercache directories.

**NOTE: This script can ONLY create symlinks, report broken symlinks, and report unused directories. Any actions to be taken with these reports is outside the scope of this script. It's up to YOU to decide what should be done and YOU must do it manually.** 

## Example 1:
Game: `Fallout: New Vegas`

ID: `22380`

You install FO:NV in Linux via Steam. Several new directories are created:
1. `../steamapps/common/Fallout New Vegas/`
2. `../steamapps/compatdata/22380/`
3. `../steamapps/shadercache/22380/`

You run this script and confirm the prompt. This script creates the symlink: `../steamapps/compatdata/Fallout: New Vegas/`

The new symlink points to: `../steamapps/compatdata/22380/`

Both directories are present in compatdata, (`22380` and `Fallout: New Vegas`) but one is much more readable than the other.

![Screenshot_20220613_175928](https://user-images.githubusercontent.com/4404140/173459502-ec491d8c-cdac-4dc3-9b3b-9c29adfd152d.png)

Simple, right?

**NOTE: This script will perform this function for EVERY game in any steamapps directory you give it in bulk(if the game has a compatdata directory).**

## Example 2:
You uninstall FO:NV.

Steam deletes _most_ of the contents of `../steamapps/common/Fallout New Vegas/` but leaves `../steamapps/compatdata/22380/` and `../steamapps/shadercache/22380/` untouched.

You run this script again. It tells you:
```
--------------------------------
Unused common directory(s) found:
../steamapps/common/Fallout New Vegas (Fallout: New Vegas)
--------------------------------

--------------------------------
Unused compatdata directory(s) found:
../steamapps/compatdata/22380
Found symlink for 22380. It's likely Fallout: New Vegas's compatdata directory.
--------------------------------

--------------------------------
Unused shadercache directory(s) found:
../steamapps/shadercache/22380
--------------------------------
```

Then the script exits.

That's where this script's abilities end. It only makes symlinks and reports unused directories. It's up to **you** to decide what to do with these unused directories.

**NOTE: This script will perform this function for _every_ game in any steamapps directory you give it (if the game has these directories).**

## How to install:
1. Save the file [steam-compatdata-symlinker](https://raw.githubusercontent.com/techmouse/steam-compatdata-symlinker-thing/main/steam-compatdata-symlinker) to whatever directory you keep your personal scripts. (Example: $HOME/bin or ~/Scripts)
2. Give the file execution permissions. (Example: `chmod +x ~/bin/steam-compatdata-symlinker`
3. Done.

## Usage:
1. Open a terminal.
2. Type `/path/to/the/script /path/to/your/steamapps/`
3. Read the instructions on screen.

For a short explanation on the script's usage, run it with the `-h` flag.

For a longer explanation on the unused directories and why they should or shouldn't matter to you, run the script with the `-e` flag.

**NOTE: It's safe to run this script on the same steamapps directory as many times as you need. For example, if you've installed new games, uninstalled old games, or moved games to another drive since the last time you ran it.**

**NOTE: This script will not make symlinks without your confirmation first. You will be given a yes or no prompt to review what links will be created first.**

## Directory filtering:
You can add your own filters for any directories you want ignored, to the variable `filter` at the top of the script.

The proper syntax is:
`filter=("directory 1 name here" "directory 2 name here" "gameid here" "99999" "etc")`

There are no commas, names should be in double quotes, and they are separated by only a space. And ensure they are _exactly_ the name of the directory you want ignored.

Example 1: The filter "Fallout" will not filter out the game Fallout: New Vegas.

Example 2: The filter "Fallout: New Vegas" will not filter out any Fallout. Nor will it filter out the game it was intended to filter either, because the directory is named "Fallout New Vegas" not "Fallout: New Vegas".

NOTE: Filter checks aren't applied to broken symlinks.

## Custom directory overrides:
You can set directories to use manually at the top of the script. These directories will be treated identically to their non-custom counterparts. These settings are optional. Leave them blank to go with the defaults.

**NOTE:**
Don't end paths with a slash ("/") and use `$HOME` to reference the home directory.

Example: commondir="$HOME/Games/mycustomsteamapps/common"

## Compatibility problems:
### Non-Steam games:
As of right now, there doesn't seem to be any reliable way to detect and track data for non-Steam games. Things may change in the future, but this seems to be something I can't automate for now.

But you can still manually make symlinks of your own for them. And if you need help finding which compatdata directory belongs to the non-Steam game you're looking for, open a terminal and type:

`find ~/path/to/your/steamapps/compatdata/ -iname *program*name*here* 2>/dev/null`

Just use `*` instead of spaces. If the program wrote user data to the prefix, then that should find it.

### Games who's titles are all numbers:
The only other problem I can foresee would be with a game who's title is all numbers. For example, the game [10,000,000](https://store.steampowered.com/app/227580/10000000/).

This script would create a symlink in compatdata that uses the game's title, which is all numbers, and this could create compatibliity problems for any game who's game ID matches the first game's title.

But in order for this to happen, you would need to own and install two games that can create this kind of conflict. And they would have to be installed in the same steamapps directory. And both would need to be Windows only. And 10,000,000 has a Linux port so even that example wouldn't work. And even if it was Windows only, there are still commas in the title, so again that example wouldn't work either. So this is a very specific situation.

In short, the odds of this happening are very, very slim, almost to the point of being comical, but I still feel a responsiblity to make you aware of it. Not impossible, but still extremely improbable.
