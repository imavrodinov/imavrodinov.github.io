---
layout: single
title: Six Months with a DLP Printer - a Retrospective
excerpt: "I got a resin 3D printer half an year ago. Let me tell you how cool it is!"
header:
  overlay_image: /assets/images/blog_media/photon_printer/anycubic_splash.jpg
  overlay_filter: 0.1
  show_overlay_excerpt: true
related: true
read_time: true
---

## The current state of affordable 3D printing

Additive manufacturing has seen drastic improvements in both popularity as well as capabilities over the last few years.

FDM 3D printers are quite common nowadays - low end machines start at around $300 and can get you up and running in no time. Most don't even require heavy duty assembly - you pull the machine out of its box, set it up on a desk, and you're off to the races.

Plastic fed printers do have some major drawbacks, though - the process isn't particularly fast and is prone to a multitude of complications. Mechanical and/or software related issues can happen often and usually result in wasted time and materials. Also, no matter how fine you print, layer lines would still be visible.

The software required to slice models (that's the process of converting 3D models into .gcode which is what printers work with) is a whole 'nother thing. It's unnecessarily complicated and has more fields, sliders, checkboxes, and buttons than NASA space equipment.

Luckily, FDM isn't the only type of technology used to create three dimensional objects for industrial and hobby applications. With over a dozen different technologies available on the market right now, one in particular is an excellent option to get into right now. And it won't break the bank either.

## On DLP technology

DLP stands for digital light processing. That's a fancy way of saying that this type of 3D printers use a powerful UV diode to polymerize liquid resin. Once cured, the resin hardens and sticks to the build plate. The exact areas where exposure needs to happen are projected through an on board LCD screen with 2K resolution. Depending on the type of material, finished results can be either left as is, used for taking silicone molds, or be metal cast.

## Here's my new toy - the Anycubic Photon

![photon_printer]({{ "/assets/images/blog_media/photon_printer/photon_printer.jpg" | absolute_url }})

So, this is the Photon. I ordered it online and it arrived in a bit over two weeks. At less than $500, it's an excellent machine I can't get enough of right now. Best birthday gift ever!

To give you a basic idea of how compact this thing is relative to its build volume, here is the official spec sheet:

![photon_specs]({{ "/assets/images/blog_media/photon_printer/photon_specs.jpg" | absolute_url }})

### Design, build quality, and usability

The Photon is a relatively straightforward machine - there isn't much going on here. It comes preassembled - you just need to install and level the bed, load the vat with resin and that's basically it.

I have to say I'm pretty impressed with the design of this thing! It's mostly made out of metal - the enclosure looks and feels like sheet metal, and the vat, bed, and base plate are all anodized aluminum.

To keep dust and debris out of the vat, all sides are covered with protective plastic screens, which unfortunately have no added UV protection.

There's also a color touch screen on board. Kinda basic, but gets the job done. So far it seems that the Photon is not open sourced, so it's not possible to hook it up to OctoPrint and run it wirelessly.

### Resolution and dimensional accuracy

DLP printers can produce REALLY impressive results. Whatever you end up printing is going to look miles ahead of any FDM print. If done correctly, models don't even look like they've been printed!

Resolution-wise, most FDM printers can go as fine as 50 μm in the Z direction, with a standard horizontal resolution of 400 μm - that's basically the diameter of the nozzle used on the extruder. Going down to 250 μm can help with print quality, but that comes with a steep price of almost double the print time. For comparison, a regular piece of copy paper is around 100 μm in thickness.

DLP has none of that - resolution in Z height can drop down to 20 μm (from the default setting of 100), and horizontal resolution is 47 μm (that's the pixel size on the LCD screen used to shine UV onto the resin).

Here are a couple of quick test prints I made shortly after getting the Photon. The rook on the left is 2cm in height, and the castle is around 4. It's basically impossible for plastic ran printers to produce something remotely close to this quality.

![test_prints]({{ "/assets/images/blog_media/photon_printer/test_prints.jpg" | absolute_url }})

Here is another batch of test prints. These are around 1 cm in height (around half an inch), printed at 50 μm layer height.

![tiny_test_prints]({{ "/assets/images/blog_media/photon_printer/tiny_test_prints.jpg" | absolute_url }})

### No toolhead, no problem!

Toolheads are messy and carry a whole assortment of complications and problems with them - travel time, wobble, belt backlash, leaking, blockages, retractions and model scarring to name a few. Not to mention the added risk of leaving something heated to 250 Celsius unattended overnight.

That's not the case with DLP printers, tho - since there is no toolhead to go around X/Y coordinates and 'draw' models layer by layer, DLP printers produce layers simultaneously. This also means that regardless of the number of objects being printed, it would take the same amount of time for the machine to do its thing. To put this into perspective, it doesn't matter if you're making a single copy of a model, or duplicate it 20 times on the build platform - DLP printers are not slowed down by extra volume. An absolute blessing for small to medium sized production lines.

### DLP drawbacks, but they're not as scary as they sound

I won't Mona Lisa myself out of this one and have to admit that DLP printers come with three very serious drawbacks.

UV resin is not safe to handle without protection. It's actually toxic and the staple here is to wear a respirator, gloves, and eye protection when handling raw resin. That's like a full Walter White cosplay every time something needs to be printed.

Resin is also quite smelly, so a dedicated room or workshop is an absolute must. The Photon is said to have some kind of carbon based air filter on board, but that doesn't do a good enough job of minimizing smells and toxic fumes during printing.

Once complete, 3D models made of resin also need a few runs of post processing. I first run mine through a wash with isopropyl alcohol followed by some warm soapy water. This process is repeated a few times until all of the uncured resin has been washed off. Most experts also suggest an extra run of curing under direct sunlight or a UV lamp for a few minutes.

### Applications

DLP printers are not just for nerds - they're used in a few very niche markets because of how reliable and accurate they are.

The most common one is jewelry production. Specialized companies offer a dedicated type of resin that can be burned off during casting, so printing in resin and casting models into gold or silver is highly sought after.

Dental follows a similar route, but use a type of resin that turns ceramic when baked hot enough.

And in general, DLP printers are used in all sorts of mechanical and engineering projects where fine details in large quantities are needed.

That about wraps up my initial thoughts about the Photon. DLP is great if you know what you're getting yourself into. Happy printing!
