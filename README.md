# Menace-Amiga
My edit of Menace for Amiga as originally published by Jimmy Maher on "Amiga - The future was here" book

The original code is taken from:

http://amiga.filfre.net/?page_id=17

http://amiga.filfre.net/misc/Chapter8/Menace.zip


When trying to compile (with SAS/C or VBCC) and run it on my Amiga machines (A500, A1200 or A3000), it showed a lot of graphical glitches and it crashed/freezed as soon as a collision between the main space-ship and an enemy happened.

With the precious help of more Marran-Experienced-Amiga-Coders[TM] (ciao Munne!), I've managed to fix these issues and have code that compiles with VBCC and run 'almost-100%' smooth on all my Amigas, from A500 to '030-Powered' A1200.

First problem identified was the lack of 'WaitForBlitter' code.

See here why you should always wait for the blitter: http://coppershade.org/articles/AMIGA/Agnus/Programming_the_Blitter/

This resulted in a lot of flickering blobs (blitted objects) leading to a total messy graphics floating around the screen. It worked fine in WinUAE emulator since by default it has a specific chipset setting called 'Wait For Blitter', described as: "Compatibility hack for programs that don't wait for the blitter correctly, causing graphics corruption if CPU is too fast.".

Second problem was that whenever there was a collision between the main space-ship and one of the enemies, it hanged my real Amigas, or crashed WinUAE with a CPU exception code. Further investigation revealed that the code to decrease the main-ship energy bar on screen was writing to memory adresses NON-Word-Aligned. While this is allowed on 68020 CPUs, on 68000 (A1000/A2000/A500/A600) it's not allowed, leading to a CPU exception.

See:

https://amigasourcecodepreservation.gitlab.io/amiga-assembler-insider-guide/
 
http://www.easy68k.com/paulrsm/doc/trick68k.htm


Other minor fixes were made to reset the collision detection before the main game-loop, otherwise you always started with one bar of energy decreased, and some attempts were made to deal with AGA chipsed, in order to make the game run smoothly on A1200/A4000, but they haven't been thoroughly tested.


Not sure about the license that can be used to publish this code.
Jimmy Maher doesn't report a spcific license on his web-page nor in the .zip file containing his C code and the graphics data.
From his admisssion, his C code is derived from assembly code that was published in "Amiga Format" magazine:

================================================================================

"For this analysis, I am hugely indebted to a series of articles
that Jones wrote for the British magazine Amiga Format in 1990, in which
he explained many of the game’s technical workings and provided much
of its assembly-language source code, complete with invaluable comments."

"Both a full play-through of the game and a number of shorter clips
showing each layer as it is added are available on this book’s Web site, as 
are Jones’s original assembly source and my own more readable C adaptation."

================================================================================
