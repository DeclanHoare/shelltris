
#Shelltris

Shelltris is a Tetris clone written in Bash by David G. Atwood.
This version has been modified by me to have compatibility with more
systems (by trial and error, as I have no idea what causes the problem).
It uses a small C helper, getch, which must be built for your operating
system.  A Makefile is included - CFLAGS can be added there. Tweak as
needed, then do a "make".  The shell script expects the getch file to
be in the current directory.

Once you have built the helper, you can play the game by changing into
the shelltris directory and typing:

	./shelltris.sh

It will calibrate itself for a few seconds, then ask you to hit 'p' to play.


##KEYS

|key|function|
|---|--------|
|,|rotate counterclockwise|
|.|rotate clockwise|
|z|move left|
|x|move right|
|space|move piece faster|
|q|end the game immediately|


##KNOWN ISSUES:

* No high score support.
* Timing loops are highly sensitive to changes in CPU performance (a problem
  that is basically unavoidable, unfortunately).
* In spite of attempts to calibrate the timers, there is still
  variation from machine to machine.
* The script must execute itself several times with different flags to do
  calibration.  This means that the path to the script is hard-coded
  within the script itself.  It is currently hard-coded to assume the
  script is in the current directory.
* Clearing rows doesn't work if the terminal is too big.
* Clearing rows on a small-enough terminal causes all of the blocks
  already on the screen to change appearance (not sure if this is a bug
  or intentional).

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


##PERFORMANCE:

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
on dgatwood@mac.com and he'll try to get them into future versions
of the software.  The same applies if you come up with interesting
features.

Beyond that, you may use portions of this code in other software,
so long as it results in a substantially different piece of software,
and so long as you provide reasonable credit in the software and any
accompanying documentation.


##REVISION HISTORY:

|Posting Date|Vers.|Notes|
|------------|-----|-----|
|2007-12-12|1.0|First public release.|
|2007-12-18|1.1|Linux compatibility fixes.|
|2017+|git|see commit history|
