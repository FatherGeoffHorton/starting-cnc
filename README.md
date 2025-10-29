# I have a CNC router. Now what?

You’ve unboxed it. You were able to cut the supplied sample file. Now what do you do?

I’ve been 3D printing for years and tried a few years ago to get into CNC routing but have up in frustration. CNC hobbyist software is sadly lacking compared to 3D printing. All I got with my machine was some outdated software (you really don’t want to use Candle) and a “Good luck\! Enjoy carving\!”

I’d start reading and everyone was talking about spoil boards and plunging rates and a dozen other parameters, none of which made any sense to me. (You do want a spoil board, but not initially.)

I decided to try again, and I’m passing along what I’ve learned (and am learning) to you.

Everything in here works for me, but I make no promises. Be sure to follow all safety precautions, and if anything breaks or anyone gets hurt, that’s on you.

## The overview

You have something–a picture, perhaps–that you want to carve into a block of wood. You need a program that takes your picture or model or whatever and turns it into a series of commands for the router to follow. You need a program that takes those commands and sends them to the router (unless you have a router that can read an SD card). And the router itself has a program (aka firmware) that takes the commands you send and tells the actual hardware what to do.

The first program, to speak more precisely, generates a tool path or tool paths for the router to follow. As the name suggests, a tool path is the path that the spindle will take over the object being machined. Tool paths are usually expressed in G-code (which was invented for CNC machining before being adopted by 3D printers). 

The next program takes the generated G-code and sends it to the router. Your router may offer an SD card slot that can read saved G-code files, which means it’s got an internal program to perform this function. Even if yours has this, it can be helpful to have a more complete program to use as an external G-code sender.

Finally, the router itself has its own firmware. For an entry-level hobbyist machine, it’s almost certainly GRBL (That’s not an acronym for anything; that’s just its name, because, and it’s pronounced *gerbil*.). The biggest reason you need to know this is that you’ll have to tell the other programs to do things the GRBL way.

What does this all look like in practice? That’s what we’ll look at next.

## From picture to G-code

Here’s a line drawing of a happy puppy (thanks to ChatGPT). How do I turn this into a nicely engraved memento? This same process will work with scanned pictures and the like–more on that later. (By the way, simpler is usually better. When you try this on your own pictures, if you’re having trouble getting good results, upload your picture to your favorite AI and ask for a simplified version.)

This picture is in PNG format; you may have one in JPG instead. CNC preparation software tends to like SVG formatted files. Some CNC software will offer to process a PNG or JPG for you, but results may vary. I’ve had good luck using [Adobe’s SVG converter](https://www.adobe.com/express/feature/image/convert/svg). (I receive no compensation for any of the products I mention here).

For this first run, I’m using OpenBuilds CAM ([httthe ps://cam.openbuilds.com](https://cam.openbuilds.com)). Clicked on “Open Drawing” and load the SVG file. You’ll notice that the shading has all disappeared; we’ll deal with that later. For now, we need to resize image to fit the piece of wood we’re using. I’m practicing on 80mm/80mm cheap wood that I got from Hobby Lobby, so I need to make sure that the puppy is no larger than this. To make that happen, click on the ruler in the toolbar at the left side of the window, hover the mouse over the drawing until it turns blue, and click. It offers the choice of resizing, so set the longer dimension to 80mm; the shorter one shrinks proportionately. Then click on the image again and choose Position, and move the lower left corner to near the origin.

Now it’s time for the program to figure out where the tool head travels to engrave the picture. In the top right, click on the box next to HappyPuppy.svg (or whatever you named your file). This selects all the vectors (a.k.a. lines) of the image. They turn red to show that they’re selected. Then click on the green button that says “Configure toolpath”. 

In the dialog box that pops up, name the path “First” (or whatever you want). For “Type of Cut”, select “CNC: Vector (no offset).” The standard bits that come with beginner routers have a cutting width of 3.2, nominally, but for surface engraving like we’re doing here, we don’t get close to that width, so try 1 mm. Choose your spindle RPM, cut depth, and feed rate based on the documentation that either came with the printer or at least is found on the manufacturer’s website. Once you’re set, click on the green button that says “”Apply and Preview Toolpath”.

We want our puppy’s eyes, nose, and mouth to be filled in. First clear the current selection by clicking on the box next to the puppy file name until it’s clear. Then hover over each eye and click, using whatever your computer uses for multi-select. Do the same for the nose. (Note that you lose the tongue when you do this). Click on “Configure toolpath” again, and this time select “CNC: pocket.” The rest of your settings should stay the same. “Apply and Preview Toolpath.”

Click on “Save G-Code”. Once that’s done, save your workspace.

To get an idea if things are in good shape, go to NC Viewer ([ncviewer.com](http://ncviewer.com)). Load the gcode file you saved in the previous step. I loaded up the puppy, and it looks good.

For some reason, OpenBuilds doesn’t include the command to turn on the spindle\! Open the gcode file in your favorite text editor and add `S17000 M3` as the seventh line, below .`G0 Z3; move to z-safe height`. As the comment suggests, this gets the spindle up to a safe height before starting it.

If you’re ready to print, you can skip to the next section while I talk about scanned/photographed images.

### Getting a picture from a scan/photo

If you took a photo, the lighting is almost certainly uneven. To fix this, you’ll need photo editing software for this: GIMP, Photoshop (Elements), Affinity Photo, etc. Whatever you use, once you have your picture, use the threshold function. Unless your picture is really weird, setting the slider to 50% should work fine.

Once you’ve done this, save your file as a PNG and take it to the SVG converter website above. If the resulting file looks good, you’re all set. If lots of lines are missing, you need to make them broader. Load your image back into your photo editor. On GIMP, use Erode; on Affinity, use Filters/Blur/Maximum Blur (though this doesn’t seem to work as well as GIMP’s erode); and on Elements, use

Once the SVG converter produces acceptable output, you’re ready to go.

## From G-Code to carving

We’ve arrived at the moment of truth: Will this work? 

Please be safe\! Set up your machine as the manufacturer suggests. Position the point of the engraving bit at the bottom left of the engraving area, just touching the surface. You can (and will) want to do this differently sometimes in the future, but this is good enough for now. Depending on how your machine is set up, you can use the front panel to move the spindle, use an app on your phone, or use a control program on your PC/Mac/Linux computer. You’re looking for the Jog function.

Once the bit is in place, take a deep breath, make sure everything is safe, and start carving.
