# Armies and attack strategies

What to bring, how to set it up, and what BasePilot does with it.

## Before you start

- **Be on the Home Village.** Either switch there yourself, or pick *Home Village* in the app
  and let the bot switch for you.
- **Use the default deployment bar layout.** Two rows is fine. BasePilot locates troops by
  matching their icons in that bar, so a customized layout can hide them.
- **Put the Valkyrie army at the top of your Saved Recipes.** See below for why the position
  matters.

## The Valkyrie army

![Saved Recipes showing Army 1: 42 Valkyries, 11 Earthquake spells, and 1 Log Launcher, with
Queen, King, Warden, and Royal Champion and their pets](army-valkyrie.png)

**42 Valkyries · 11 Earthquake spells · 1 Log Launcher**, plus your heroes and pets. That fills
336/352 housing, 11/11 spell slots, and 1/3 siege capacity.

The 11 Earthquakes aren't arbitrary: BasePilot clicks exactly 11 earthquake points per raid, so
a full spell bar means every point it places gets a spell.

**Save it as a recipe and keep it at the top of the list.** If Valkyries aren't already in your
deployment bar, BasePilot opens Saved Recipes, finds the Valkyrie row, and clicks the **Use**
button next to it — but it only looks for that button within a short vertical distance of the
row. A recipe further down the list can scroll out of range, and army loading fails.

> **The one hard rule:** BasePilot finds troops by matching their icon in the deploy bar. If the
> troop you selected on the Run page isn't in your army, the attack aborts immediately and the
> log reads `Troop <name> not found!`. Same for spells — no Earthquake in your spell slots means
> the spell step is silently skipped.

## How Valkyries deploy

Valkyries, Sneaky Goblins, and Super Minions share one routine. Per raid, in order:

1. **Select the troop** in the deploy bar. Abort if its icon isn't there.
2. **Drag-deploy around the perimeter.** One continuous press-and-drag from a randomly chosen
   corner, through four corner segments, then release — this empties the selected slot in a line
   along the base edges. Start corner and direction are randomized every raid (at 16:9 it starts
   left or right; at 16:10 it may also start from the top), and each segment's duration is
   jittered ±10%.
3. **Super Dragon**, if it's in the bar.
4. **Siege machine** — Log Launcher first, else Siege Barracks.
5. **Heroes**, in random order each raid: Queen, Warden, Royal Champion, King, Prince, Dragon
   Duke. Whichever are present get dropped, then clicked a second time to fire their abilities.
6. **Earthquake spells.**

Because step 2 is a drag rather than individual taps, exact troop count matters less than
filling the slot — the drag spreads whatever you're carrying along the edges.

## Earthquake placement

BasePilot clicks **11 earthquake points** per raid, in one of two patterns
(*Settings → Earthquake*):

- **Curve placement** (default) — samples an arc through the left/top/right anchor points with
  ±100px of jitter per point.
- **Random placement** — random points inside the region between that arc and a horizontal line
  40% up the frame. Falls back to curve placement if that region comes out degenerate.

It clicks all 11 points regardless of how many Earthquakes you're carrying; once you're out, the
remaining clicks land harmlessly.

## Other strategies

**Edrags** use a different routine: **12 Electro Dragons** placed individually around the diamond
perimeter, 0.2s apart, rather than dragged. Heroes and spells follow as above.

**Sneaky Goblins** and **Super Minions** use the same drag routine as Valkyries.

**Builder Base:** Baby Dragon is the only supported strategy. Night Witches is listed in the UI
but still under development.
