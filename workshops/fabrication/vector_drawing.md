# Vector Drawing With Inkscape
 
Inkscape is a free vector graphics editor for GNU/Linux, Windows and macOS. You can download Inkscape [here](https://inkscape.org/release/).


## Set Up

1. Download [GlowforgeTemplate.svg](cuts/GlowforgeTemplate.svg)
1. Open GlowforgeTemplate.svg using Inkscape
1. Choose `File > Save Template...`, call it "Glowforge Template" and make sure `Set as default template` is checked
1. Close the file
1. Select `File > New` — you should see a light orange grid


## Configuration and Creation

For our purposes, we will primarily be using the "Pen Tool", which you can select from the menu on the left.

Make a shape by clicking to define its vertices and then close it by clicking where you started.

While the shape you just created is highlighted, choose `Object > Fill and Stroke...`. Under the sub-panel `Fill` choose the `X` for no fill. Under `Stroke style`, set your line width to 0.010. Keep these settings for everything that you do.

Make note of the "snapping" menu in the upper-right corner, which will be helpful when you want your drawing to conform to the grid (or not). 

Use + (actually =) and - on your keyboard to zoom in and out.


## Layers and Laser settings

Different colors in your .svg will be treated as different layers in the Glowforge interface. While we're just making cuts, everything should be the same color.

However, you also have the option to "score". This is when the laser doesn't fully cut through the material, which is useful for making diagrams and decorations on the surface of your instrument. Choose a different line color for lines you want to score so that they show up on their own layer in Glowforge, for which you can select scoring.

Additionally, there is an option, "engrave". Because this uses a lot of resources (time, laser, filter, etc), we will avoid engraving, except for when it is helpful attaching parts, such as with the 3" speaker below. For these sections, you'll use a different color as a fill, indicating the area to be engraved.




## Examples

1. Holes that fit a panel pushbutton, a panel switch, and a potentiometer/knob: [switches_slider.svg](cuts/switches_slider.svg)
1. Holes that fit a 5mm and 8mm LEDs: [leds.svg](cuts/leds.svg)
1. Box demonstrating puzzle sides: [box.svg](cuts/box.svg)
1. 3" speaker: [speaker.svg](cuts/speaker.svg)



