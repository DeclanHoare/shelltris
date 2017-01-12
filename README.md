
#Shelltris

Shelltris is a Tetris clone written in Bash by David G. Atwood.
This version has been modified by me to fix some bugs and enhance the
game's functionality, although since I don't have a Mac OS 10 computer
I may have broken compatibility with that operating system in the
process.
It uses a small C helper, getch, which must be built for your operating
system.  It can also take advantage of a configuration directory at
/var/games/shelltris if it is present and writable.  Both of these
can be handled by Autotools by running:

`autoreconf -i`
`./configure`
`make`

as yourself and then

`make install`

as root, but if you wish to bypass the Autotools system, you can build
getch by simply running
`make getch`
which will allow you to play the game locally.

If you opted to build getch and play the game locally, you can run it
with:
`./shelltris.sh`
in the shelltris directory. If you instead installed it system-wide, you
can run it with:
`shelltris`
anywhere.

The first time you run Shelltris, it will calibrate itself for your
environment to make sure it runs at a reasonable speed. If you have
issues later on, you can recalibrate by running Shelltris with the
`-calibrate`
parameter.

##KEYS

|key  |function                |
|-----|------------------------|
|,    |rotate counterclockwise |
|.    |rotate clockwise        |
|z    |move left               |
|x    |move right              |
|space|move piece faster       |
|q    |end the game immediately|


##KNOWN ISSUES:

* Timing loops are highly sensitive to changes in CPU performance (a problem
  that is basically unavoidable, unfortunately).
* In spite of attempts to calibrate the timers, there is still
  variation from machine to machine.
* Clearing rows causes all of the blocks already on the screen to change 
  appearance (I'm not sure if this is a bug or intentional).
* Occasionally, when selecting the next piece, a message might appear saying:
  `/usr/local/bin/shelltris: line 1149: warning: command substitution: ignored null byte in input`
  This message will overwrite the playing area and not go away until a new piece moves over it.
  This is already supposed to be suppressed with 2> /dev/null, but this does not work with local
  for some reason.

##IDEAS:

* Music using speaker-test? - there is a function to play a tone:
   _alarm() {
     ( \speaker-test --frequency $1 --test sine )&
     pid=$!
     \sleep 0.${2}s
     \kill -9 $pid
   }
  My main concern is adding this into the timer. Obviously the call
  would have to be backgrounded, but there would still be a slight
  delay.

##DECLAN'S CHANGES:

My Arch desktop can't play the original version of this game, although it works
fine on Debian and Ubuntu. After a bit of debugging, I found that the cause of
the issue was with four lines in the calibrate_timers function, all formatted
like:

`2>/tmp/drawtesttime time -p ./shelltris.sh -testdraw`

For some reason, on Arch, the time command was being treated as an external one,
and since it only exists as a builtin, it failed. I changed it to:

`{ time -p ./shelltris.sh -testdraw ; } 2>/tmp/drawtesttime`

This allows the construct to work properly on Arch. I guess it might be related
to zsh being my primary shell on my Arch machine, too, but I don't know why that
would be a problem, and running the unmodified game from a bash shell doesn't
work either. I am absolutely bewildered by this problem but it's fixed now.

In addition:

* There was one part of the code which assumed that the board was 23 lines high,
  breaking line clearing for any taller terminals. I changed it to adapt to the
  size of the terminal like the rest of the code.
  
* A high score system has been added. The high score is displayed when you lose,
  and if you get a higher one, it is replaced. The score is stored in
  /var/games/shelltris/highscore.sh. If /var/games/shelltris is not writable,
  ~/.config/shelltris will be used instead.

* The Makefile has been entirely replaced with an Autotools-based build system,
  which also creates the newly necessary files in /var/games and installs the
  script itself, making the game easy to install system-wide.

* The directories the game is installed in and run from no longer matter.

* Calibration information is now saved to /var/games/shelltris/calibrate.sh
  and automatically loaded on subsequent runs. If /var/games/shelltris is
  not writable, ~/.config/shelltris will be used instead.



##PERFORMANCE:

###Mac OS 10
For maximum performance on slower machines, it is recommended to use Mac OS X v10.5
over previous versions.  Terminal 2.0 seems to be dramatically faster than
previous versions (on the same hardware) for every drawing test.
Your mileage may vary.

##LEGAL STUFF:

This software is provided AS-IS with NO WARRANTY.  If something goes
wrong, you're on your own.  This software may be freely distributed,
provided that notices of copyright and authorship remain unmodified.

If you find bugs and are able to fix them, please either make a
pull request to my repository or e-mail them to David G. Atwood
on dgatwood at mac dot com and he'll try to get them into future versions
of the software.  The same applies if you come up with interesting
features.

Beyond that, you may use portions of this code in other software,
so long as it results in a substantially different piece of software,
and so long as you provide reasonable credit in the software and any
accompanying documentation.


##REVISION HISTORY:

|Posting Date|Vers.|Notes                     |
|------------|-----|--------------------------|
|2007-12-12  |1.0  |First public release.     |
|2007-12-18  |1.1  |Linux compatibility fixes.|
|2017-01-12  |1.2  |See "DECLAN'S CHANGES".   |
|2017+       |git  |see commit history        |
