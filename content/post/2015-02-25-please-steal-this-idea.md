---
author: Jesse Morgan
categories:
- Uncategorized
date: "2015-02-25T11:19:22Z"
guid: http://morgajel.net/?p=1644
id: 1644
title: Please Steal This Idea.
url: /2015/02/25/1644
---

Someone take this idea and run with it; just make sure it’s free to use. I don’t have the time for it and it’s too great not to write down.

The idea? A simple **graph paper map maker**. Nothing with fancy graphics like campaign cartographer or FantasticMapper, Just a simple graph paper mapper.

The interface consists of two major parts- the map window and the collapsible sidebar.

## Main Window

The main window looks like an unblemished minesweeper screen with a giant crosshair segmenting it into 4 parts. There is both a horizontal and vertical scrollbar. only 1/2 of the window is showing according to the scrollbars, meaning you can scroll left or right, up or down. In the bottom corner is a zoom tool similar to google maps.

## Sidebar

The sidebar appears as a small button on the left that folds out when clicked. it contains a vertical accordion menu with the following headers:

### Select

\[S\]elect mode will let you click on an object underneath the cursor. multiple clicks will cycle focus on the next item in the stack underneath the cursor. This could be a Feature, Interior Wall, or Path.

### Excavate

\[E\]xcavate has two main options to toggle between- Excavate (default) and Fill. While these are excavated, clicking on squares in the main window will either be emptied or filled in. Using the shift key reverses the active option- Excavate becomes Fill and vice versa. Excavate is the default tool when the page is loaded.

### Wall

\[W\]all would be relatively straight forward. it would only affect existing exterior walls, either in part, whole, or individual lengths. Options would include smooth, rough, and natural.

### Interior Wall

\[I\]nterior Wall has several options and two modes. The primary mode “Snap” would snap the grid between two excavated squares; the secondary mode “Free” would allow straight lines to be drawn between two arbitrary points. Clicking and Dragging will draw out a temporary path until the mouse is released. Using the shift key lets you a rectangle of interior walls. Options would include walls (default), half walls, engraved walls, magical forcefields, cliffs/ledges, ruined walls, lower level walls, walls with arrow slits, water lines, etc.

### Door

\[D\]oors would have several options, all of which snaps to and highlights a border between two squares. This would work between two dug squares or a dug and filled square (i.e. a fake/useless door). Options would include regular door (default), double door (2 squares long), secret door, portcullis, false door etc.

### Features

\[F\]eatures can be placed and resized on any map, and is layered between the floor tiles and the walls, allowing for things like puddles to be half-covered. This will contain a block of highlightable icons, which will let you draw an item that can be moved, resized, or spun. Icons include stairs (default), circular stairwell, debris, water, pit, pillar, altar, chair, throne, table, crate, barrel, fireplace, statue, well, sarcophagus, dias, bridge, carpet, etc.

Markers

### Traps

\[T\]rap behavior would be dictated by type, but would mostly act like Features. Options would include pit traps (default), spike traps, blade traps, poison gas, etc.

### Path

\[P\]ath would allow you to draw a simple line from point A to point B. This could be a dispersed “sandy” type line, a dashed, dotted or solid line of configurable width.

### Options

\[O\]ptions would contain:

- Line color (black, blue, gray)
- Grid (none, excavated areas, all areas)
- Grid Fade (100%, 50%, 25%)
- Grid Color (black, blue, gray)
- Border (click and drag region to be included in PNGs
- Show Compass checkbox
- Show scale checkbox
- Tile Pattern (none, granite, stone, etc)
- Fill Pattern (none, stone, line color, black)
- square scale (5ft, 10ft, other)

### Save

\[S\]ave would give you the option of saving the output (SVG) to google drive, locally, or exporting to PDF if a border is not defined, it will do a best guess.

## Navigation

Right click would drag the map; +/- would zoom, arrow keys would pan. The map will pan infinitely in any direction, based off the centerpoint.

Hotkeys would include:

- \[S\]elect mode
- \[E\]xcavate
- \[I\]nterior Wall
- \[D\]oors
- \[F\]eatures
- \[T\]rap
- \[P\]ath
- \[O\]ptions
- \[S\]ave
- \[ctrl+z\] undo
- \[ctrl+shift+z\] redo
- standard copy/cut/paste

So that’s the idea that’s been kicking around in my head. If you’re a UI person and interested in helping me, I’d be glad to help give guidance on functionality, but I don’t have the time to develop it myself.