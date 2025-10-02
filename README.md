# Patch Patrician 3's Resolution

I bought [Patrician 3 from GOG][].  It was pretty fun but it's 1024x768 or 1280x1024 resolutions looked dumb on my very wide screen MacBook Pro at 1440x900.  I subsequently read about several people who had patched foreign or older versions of the game.  However, none of the easy methods for patching the game (hex editor instructions, patch utilities) worked on GOG's 2.0.0.5 version, so I made this Java program to make patching GOG's latest version easy for everyone--hopefully.

[Patrician 3 from GOG]: http://www.gog.com/game/patrician_3

## How To Use This

**NOTE: I take no responsibility if this breaks your game, deletes your save games, or turns your computer into a murderous AI bent on world domination!**

This will patch the Patrician 3 game to use a resolution that you select instead of the default 1280x1024.  **The game will still say 1280x1024 in its settings** but it will actually be running at your selected resolution instead.

1. Back up your game, including all save games!
2. Download and install Patrician 3 from GOG.
3. Ensure you have a modern Java runtime (now built with Java 25 toolchain). Java 17+ should also run it, but recommended is Java 25 (current LTS used for build).
4. Download the latest `P3WideScreen-all.jar` file (fat JAR with dependencies) OR build it yourself (see below).
5. Run the JAR file (double click if associated, or `java -jar P3WideScreen-all.jar`):
     1. Select the directory where you have installed Patrician 3.
     2. Enter the resolution you'd like to use instead of 1280x1024.
     3. Hit the patch button.
6. Run Patrician 3 and select the 1280x1024 resolution.

While the menus may be at a different resolution, once you're actually playing the game, you should be at your selected resolution instead of 1280x1024.

## Building with Gradle (Java 25)

The project has been migrated from Maven to Gradle. A fat JAR is produced automatically when you run the standard build.

### Steps

```bash
# (Optional) Generate Gradle wrapper if not yet present (requires a local Gradle install once):
# gradle wrapper

# Show Gradle version (using wrapper once generated)
./gradlew --version

# Clean & build (compiles + tests + creates thin and fat JAR)
./gradlew clean build

# Fat JAR output (runnable with all dependencies):
# build/libs/p3-wide-screen-1.0.0-SNAPSHOT-all.jar

# Run the application directly via Gradle
./gradlew run

# Or run the fat JAR
java -jar build/libs/p3-wide-screen-1.0.0-SNAPSHOT-all.jar
```

On Windows (cmd):
```cmd
gradlew clean build
java -jar build\libs\p3-wide-screen-1.0.0-SNAPSHOT-all.jar
```

### Toolchain
We use Gradle's Java Toolchain set to Java 25. If you have the Foojay resolver plugin enabled (as in this repo), IDE/Gradle can auto-provision the matching JDK.

### Why remove Maven?
Because we live in 21st century!  
(Gradle configuration is simpler here and consolidates the fat JAR process without needing the assembly plugin.)

## If Things Go Wrong

To go back to regular 1280x1024:

1. Look at the log output.  This tool will tell you when it backs up a file and what name it is using for the backup.  Restore the backup P3WideScreen made of the `Patrician3.exe` file.  (Quite likely `Patrician3.exe.bak`, unless that file already existed.)  If you've lost the log output, try sorting by date and looking for the most recent `Patrician3.exe*.bak` file created in your Patrician 3 directory.
2. Delete the following files from your Patrician 3 directory:
    * `images/HauptscreenE1280.bmp`
    * `images/Vollansichtskarte1280.bmp`
    * `scripts/accelMap.ini`
    * `scripts/screenGame.ini`
    * `scripts/textures.ini`

## Credits

All credit for this goes to the people who figured out how to patch this game in the first place.  That seems primarily to be [brotkohl at some German Patrician forum][brotkohl].  Thanks to [billyplod at the Kalypso Media forums][billyplod] for translating that.  Thanks to [whatnames at the Widescreen Gaming Forum][whatnames] for trying to pull together all the various sources of information about how to patch this game, and for detailing how he applied them to the GOG release of Patrician 3.

[brotkohl]: http://www.patrizierforum.net/index.php?page=Thread&threadID=3376&pageNo=1
[billyplod]: http://forum.kalypsomedia.com/showthread.php?tid=8713&pid=86997#pid86997
[whatnames]: http://www.wsgf.org/forums/viewtopic.php?t=26206

I used the [CPR format page on the XeNTaX wiki][cpr] to figure out how to get the needed BMP and INI out of Patrician's CPR file.  (Though the format page there is incomplete, as it doesn't mention that the CPR file can contain many headers.  Check out my `CPRFile` class.)

[cpr]: http://wiki.xentax.com/index.php?title=Patrician

## About the Code

Originally written targeting Java 8; now built and tested with Java 25 using Gradle toolchains. The GUI uses MigLayout (snapshot) and SLF4J (updated to 2.0.x) with the JUL binding.

## License

Copyright (C) 2014  Dale Sedivec

**P3WideScreen** is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

**P3WideScreen** is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with **P3WideScreen**.  If not, see <http://www.gnu.org/licenses/>.
