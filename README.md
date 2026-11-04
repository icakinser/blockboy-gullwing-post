# Gullwing Post

*A first-person flight game made with [BlockBoy Game Foundry](https://github.com/icakinser/Blockboy-Game-Foundry), a block-based game engine.* This repo holds the playable game, its cartridge, and a step-by-step tutorial that rebuilds it from an empty cartridge.

<p align="center"><img src="screenshots/player.png" width="420" alt="Gullwing Post running in the BlockBoy player"></p>

**Gullwing Post** is a first-person flight game. Wren flies the island mail over the Saltmarrow Isles: take off from Harbour Field on the home island, fly through the eight beacon rings of the mail route in any order, and land on the strip on top of Lantern Rock in the east. Lantern balloons are worth points. Hitting the ground anywhere but a runway costs a heart and restarts at the last checkpoint ring.

1. **Harbour Field (west):** Wren starts parked on the runway, facing east.
2. **The Teeth (north-west):** three snowy peaks. One ring threads the gap between them and one floats high above.
3. **Gannet Gorge (centre):** two long rock ridges with a channel between them. Two rings sit low inside it.
4. **Old Ember (north-east):** a volcano with a ring over its crater.
5. **The Patchwork (south):** a farm island with a mill pond. Two rings sit low over the fields and the sea.
6. **Lantern Rock (east):** a flat-topped cliff with the landing strip on top. It opens once all eight rings are in.

## Play it

- **In your browser:** download [`index.html`](index.html) and open it. The whole game is in that one file and needs no install or internet connection.
- **On the web:** turn on GitHub Pages for this repo (Settings > Pages > Deploy from branch `main`, folder `/`) and the game plays at `https://icakinser.github.io/blockboy-gullwing-post/`.
- **In BlockBoy:** open [`gullwing-post-cartridge.json`](gullwing-post-cartridge.json) with the editor's **Import** button to see and edit every object, block and rule. The game also ships with BlockBoy itself under **NEW > GULLWING POST**.

The controls are listed in [Controls in the finished game](#controls-in-the-finished-game) below.

## Screenshots

| | |
|---|---|
| <img src="screenshots/01-harbour-field.png" width="400" alt="Parked on Harbour Field, ready for take-off"><br>Parked on Harbour Field, ready for take-off | <img src="screenshots/02-the-teeth.png" width="400" alt="The Teeth"><br>The Teeth |
| <img src="screenshots/03-gannet-gorge.png" width="400" alt="A ring and a lantern balloon in Gannet Gorge"><br>A ring and a lantern balloon in Gannet Gorge | <img src="screenshots/04-old-ember.png" width="400" alt="Old Ember, the volcano"><br>Old Ember, the volcano |
| <img src="screenshots/05-the-patchwork.png" width="400" alt="The Patchwork farm island"><br>The Patchwork farm island | <img src="screenshots/06-lantern-rock.png" width="400" alt="Lantern Rock and the landing strip"><br>Lantern Rock and the landing strip |

## What's in this repo

| File | What it is |
| --- | --- |
| `index.html` | The standalone game, exported from BlockBoy |
| `gullwing-post-cartridge.json` | The game's cartridge, for loading into the BlockBoy editor |
| `screenshots/` | The pictures in this README |

## Tutorial: build Gullwing Post step by step

**Gullwing Post** is a first-person flight game. Wren flies the island mail over the Saltmarrow Isles: take off from Harbour Field on the home island, fly through the eight beacon rings of the mail route in any order, and land on the strip on top of Lantern Rock in the east. Lantern balloons are worth points. Hitting the ground anywhere but a runway costs a heart and restarts at the last checkpoint ring.

1. **Harbour Field (west):** Wren starts parked on the runway, facing east.
2. **The Teeth (north-west):** three snowy peaks. One ring threads the gap between them and one floats high above.
3. **Gannet Gorge (centre):** two long rock ridges with a channel between them. Two rings sit low inside it.
4. **Old Ember (north-east):** a volcano with a ring over its crater.
5. **The Patchwork (south):** a farm island with a mill pond. Two rings sit low over the fields and the sea.
6. **Lantern Rock (east):** a flat-topped cliff with the landing strip on top. It opens once all eight rings are in.

This tutorial rebuilds the whole game from an empty cartridge, with every block setting. The five flight blocks are covered in depth in [the block reference](https://github.com/icakinser/Blockboy-Game-Foundry/blob/main/docs/BLOCK-REFERENCE.md#flight).

**What you will learn:** how the cockpit view turns a top-down map into 3D land; a plane hero (bank, climb, throttle, stall, crash); hills, peaks, ridges, a volcano, mesas and a lake made from Landform objects; runways to take off from and land on; rings, balloons and checkpoints floating in the sky; a finish line that only counts once you have landed.

**Time:** about 2 hours.

---

### Before you start

- **Keep the finished game open for comparison:** in BUILD mode click **NEW** and choose **GULLWING POST**. Play it, then save your own work to a separate file.
- **How the 3D works.** You still build on the flat map, seen from above. The hero's spot on the map is the point under the plane; the plane's height is kept by **Flight controls**. When you press Play, **Cockpit view** draws the map as land:
  - an object with **Landform** raises the ground under its rectangle (or flattens it into a lake);
  - an object with **Runway** lays a flat landing strip;
  - every other object with a sprite stands up on the ground facing you, or floats at its **Sky height**.
- **Coordinates.** Every object gives `x, y, w × h` (top-left corner and size). Map **y grows south**, so a small y is north. Heights (altitude, landform height) are in the same pixels as the map.
- **Size limit.** One object can be at most one screen: `320 × 240`. Bigger islands are several landforms overlapping; where they overlap, the higher one wins.
- **Kinds.** Each kind comes with starter blocks; the steps say which to keep. Remove a block with the **×** on its card. Scenery here is mostly **Spikes** with the **Hazard** block removed: an object with no blocks is just a picture.
- **Settings** are written as `Setting = value` using the inspector's names; anything not mentioned stays at its default.

#### Controls in the finished game

| Key            | Does                                             |
| -------------- | ------------------------------------------------ |
| Space or Z (A) | Open the throttle (faster)                       |
| X (B)          | Close the throttle; on the ground, brake         |
| Up             | Climb; on the runway above stall speed, lift off |
| Down           | Dive                                             |
| Left / Right   | Bank and turn; on the ground, steer              |

Up also counts as A on the keyboard, so the throttle only opens while Up is not held.

---

### Part 1: Set up the scene

1. **NEW > EMPTY GAME.** Delete the ground strip: a flight game has no floor.
2. Rename the scene **SALTMARROW ISLES**, **W = 2000**, **H = 1200** (the biggest a scene can be).
3. **CAMERA = Follow player.** (The editor still scrolls the map; in play the cockpit takes over.)
4. Palette: **COPY** a built-in palette, name it **Gullwing**: Ink `#10233a`, Dark `#2f5a6e`, Light `#8fb8a0`, Paper `#eef2dc`. **USE FOR THIS LEVEL**. The 3D view uses these four shades for the land and sky, with dithering in between.
5. **INTRO = FLY THE 8 RINGS · LAND ON LANTERN ROCK.**

The map, roughly:

|                   | x 0 to 600          | x 600 to 1200       | x 1200 to 2000      |
| ----------------- | ------------------- | ------------------- | ------------------- |
| **y 0 to 450**    | open sea            | The Teeth           | Old Ember           |
| **y 450 to 850**  | Home island (start) | Gannet Gorge (west) | Gannet Gorge (east) |
| **y 850 to 1200** | open sea            | The Patchwork       | Lantern Rock (end)  |

---

### Part 2: Wren and her plane

1. Select the hero, rename **Wren**, `196, 594, 10 × 10`. (Small, so the plane fits through the rings.)
2. Remove **Gravity, Run, Jump, Double jump, Dash, Stomp attack, Dash attack**. Nothing should move the hero except the plane.
3. Add **Flight controls** at the top:

   | Setting                       | Value     |
   | ----------------------------- | --------- |
   | Top speed                     | 150       |
   | Stall speed                   | 45        |
   | Turn rate at full bank        | 70        |
   | Steepest climb                | 20        |
   | Up / Down                     | Up climbs |
   | Start altitude (off a runway) | 120       |
   | Start heading                 | 90 (east) |
   | Ceiling                       | 340       |
   | Safe landing speed            | 80        |
   | Ground                        | Sea       |

   **Ground = Sea** makes everything outside the islands water. A hero placed on a Runway starts parked on it; anywhere else it starts in the air at the start altitude.

4. Add **Cockpit view**: `Field of view = 75`, `View distance = 800`, `Haze = 0.3`, `Cockpit = Canopy and instrument panel`, `Radar range = 800`.
5. **Damage + respawn**: `Max hearts = 3`, `Invulnerable after hit = 1.5`, `Knockback = 0`. Keep **Core collector, Exit reader, Checkpoint reader**.
6. Sprite: a small plane seen from above (wings across the middle, tail at the bottom). You only see it on the editor map; in play you are inside it.

Press **Play** now. You should see the sea all the way to the horizon, a cockpit panel, and the compass reading 090. With no runway yet, Wren starts in the air: try banking and climbing. Hitting the sea is a crash, so climb before you try anything else.

---

### Part 3: Map tiles for the land

Landform objects are not drawn in 3D (the land is), but they show on the editor map, so give each one a tiled sprite that says what it is. Draw one 16 × 16 tile each in **SPRITE** and set **ON BIG OBJECTS = Repeat as tiles**:

| Tile    | Look                              | Used for                     |
| ------- | --------------------------------- | ---------------------------- |
| Turf    | green with a few lighter flecks   | grass hills                  |
| Stone   | grey with cracks                  | rock and snowy peaks         |
| Dunes   | sand with wavy stripes            | beaches, slopes, gorge floor |
| Furrows | yellow-green rows                 | fields                       |
| Canopy  | dark green blobs                  | forest                       |
| Waves   | blue with small wave marks        | lakes                        |
| Tarmac  | dark grey with a short white dash | runways                      |

Save one landform of each look as an entity, so you can stamp them out and only change the size and settings.

---

### Part 4: The home island and Harbour Field

Every landform is a **Spikes** object with **Hazard removed** and **Landform** added. Place them in this order: the island first, then the runway, so the runway flattens the hill under it.

| Name             | x, y, w × h           | Height | Shape           | Surface | Roughness |
| ---------------- | --------------------- | ------ | --------------- | ------- | --------- |
| Home island      | `120, 480, 320 × 240` | 34     | Hill            | Grass   | 0.5       |
| Home island west | `40, 400, 200 × 240`  | 46     | Hill            | Grass   | 0.5       |
| Gull Head        | `40, 630, 160 × 140`  | 80     | Hill            | Rock    | 0.6       |
| Harbour woods    | `200, 425, 200 × 90`  | 24     | Mesa (flat top) | Forest  | 0.2       |
| Home beach       | `350, 650, 140 × 80`  | 6      | Mesa (flat top) | Sand    | 0.1       |

**Harbour Field:** Spikes `160, 586, 260 × 26`, Hazard removed, add **Runway** with `Height above sea = 20`. The centre line runs along the longer side, east-west. Wren at `196, 594` now sits on it, so she starts parked.

Scenery (Spikes, no blocks, sprite only). These stand up in 3D, as tall as they are wide:

- **Harbour wind sock** `200, 568, 12 × 12`: a pole with a striped sock.
- **Cottage 1–3** `240, 620`, `262, 624`, `300, 622`, all `16 × 16`: a house with a dark roof.
- **Gull Head light** `90, 670, 40 × 40`: a 32 × 32 sprite of a striped lighthouse standing in the middle of the frame, with two frames (lamp lit, lamp dim, 0.6 s) so it blinks.
- **Pine 0–5** `14 × 14`, along the woods: `236, 452`, `262, 470`, `288, 452`, `314, 470`, `340, 452`, `366, 470`.

**Playtest:** hold **Space** until the speed passes 45, then hold **Up**. "AIRBORNE" means you are flying. Bank around and look back at the island: the woods, cottages and lighthouse should stand on the land.

---

### Part 5: The Teeth

Peaks with surface **Rock, snow on top** get white caps above about 60% of their height. A wide low hill underneath makes foothills.

| Name            | x, y, w × h           | Height | Shape | Surface           | Roughness |
| --------------- | --------------------- | ------ | ----- | ----------------- | --------- |
| North Tooth     | `650, 130, 220 × 220` | 210    | Peak  | Rock, snow on top | 0.5       |
| Crooked Tooth   | `820, 280, 200 × 200` | 170    | Peak  | Rock, snow on top | 0.6       |
| Little Tooth    | `560, 320, 160 × 160` | 110    | Peak  | Rock              | 0.5       |
| Teeth foothills | `620, 210, 320 × 240` | 40     | Hill  | Grass             | 0.6       |

The order of overlapping landforms does not matter: at every point the highest one wins.

---

### Part 6: Gannet Gorge

Two **Ridge** landforms side by side make a gorge. A ridge runs along the longer side of its rectangle, so these wide, thin rectangles make east-west walls. Each wall is two pieces because one object can be at most 320 wide.

| Name                  | x, y, w × h            | Height | Shape           | Surface | Roughness |
| --------------------- | ---------------------- | ------ | --------------- | ------- | --------- |
| Gorge north wall      | `990, 555, 320 × 90`   | 140    | Ridge           | Rock    | 0.5       |
| Gorge north wall east | `1280, 545, 280 × 90`  | 150    | Ridge           | Rock    | 0.5       |
| Gorge south wall      | `1000, 725, 320 × 90`  | 130    | Ridge           | Rock    | 0.5       |
| Gorge south wall east | `1290, 735, 280 × 90`  | 140    | Ridge           | Rock    | 0.5       |
| Gorge floor           | `1130, 610, 320 × 160` | 14     | Mesa (flat top) | Sand    | 0.2       |

The channel between the walls is about 80 px wide at the bottom. Fly through it at 50–70 altitude.

---

### Part 7: Old Ember

| Name         | x, y, w × h            | Height | Shape   | Surface | Roughness |
| ------------ | ---------------------- | ------ | ------- | ------- | --------- |
| Old Ember    | `1530, 160, 260 × 240` | 240    | Volcano | Rock    | 0.4       |
| Ember slopes | `1500, 180, 320 × 240` | 50     | Hill    | Sand    | 0.5       |

A **Volcano** is a cone with a crater: the top 20% of the middle drops down, so a ring floating just above the rim sits over a hole.

---

### Part 8: The Patchwork

| Name            | x, y, w × h            | Height | Shape                    | Surface | Roughness |
| --------------- | ---------------------- | ------ | ------------------------ | ------- | --------- |
| Patchwork west  | `610, 890, 300 × 220`  | 22     | Mesa (flat top)          | Fields  | 0.2       |
| Patchwork east  | `900, 910, 280 × 200`  | 18     | Mesa (flat top)          | Fields  | 0.2       |
| Mill Pond       | `840, 945, 120 × 90`   | 0      | Lake (flattens to water) | Water   | 0.4       |
| Patchwork downs | `560, 1000, 160 × 120` | 50     | Hill                     | Grass   | 0.5       |

A **Lake** flattens everything under its oval to water at sea level, whatever order you place it in.

Scenery: **Cottage 4–6** at `820, 940`, `1000, 950`, `1090, 1060`; **Mill vane** (a wind sock) at `960, 1060`; **Pine 10–17** (`12 × 12`) in a row along the south shore from `700, 1080`, every 52 px east, nudged 14 px south every other tree.

---

### Part 9: Lantern Rock and the landing strip

| Name           | x, y, w × h            | Height | Shape           | Surface | Roughness |
| -------------- | ---------------------- | ------ | --------------- | ------- | --------- |
| Lantern Rock   | `1690, 850, 280 × 220` | 70     | Mesa (flat top) | Grass   | 0.15      |
| Lantern skirts | `1670, 840, 320 × 240` | 30     | Hill            | Rock    | 0.6       |

**Lantern strip:** an **Exit** `1720, 950, 220 × 26`. Keep **Locked exit** (`Cores needed = 0`, meaning all of them) and add **Runway** with `Height above sea = 70`, the same as the mesa top, so the strip is level with the cliff.

While flying, the hero only touches an object at that object's own height. A runway is flat on the ground, so the Locked exit only counts once the plane has landed on it. Flying low over the strip does nothing.

Scenery: **Lantern wind sock** `1760, 932`; **Lantern light** (lighthouse) `1890, 990, 40 × 40`; **Cottage 7** `1800, 1000`; **Pine 30–34** (`14 × 14`) from `1712, 870`, every 30 px east, alternating 12 px south.

---

### Part 10: The eight rings

Each ring is a **Core** `28 × 28` with **Collectible core** (`Points = 500`) and **Sky height** (`Altitude` as below, `Reach = 12`, `Bob = 2`). The plane collects one only when it crosses the ring on the map **and** is within 12 px of its altitude, so you have to fly through it, not over it.

Ring sprite: a round hoop with an empty middle, with a second, brighter frame (0.3 s) so it glows.

| Name   | x, y         | Altitude | Where                        |
| ------ | ------------ | -------- | ---------------------------- |
| Ring 1 | `626, 586`   | 70       | just east of the home island |
| Ring 2 | `826, 316`   | 150      | the gap between the Teeth    |
| Ring 3 | `986, 236`   | 250      | high over the Teeth          |
| Ring 4 | `1066, 676`  | 60       | gorge entrance               |
| Ring 5 | `1406, 676`  | 50       | deep in the gorge            |
| Ring 6 | `1646, 266`  | 280      | over Old Ember's crater      |
| Ring 7 | `1346, 1006` | 40       | low over the sea, south      |
| Ring 8 | `826, 986`   | 50       | over the Patchwork fields    |

The radar on the panel shows rings as blinking dots, turned so straight ahead is up. A dot on the radar's edge is further away than the radar range.

---

### Part 11: Checkpoint rings

A **Checkpoint** `28 × 28` with **Checkpoint flag** and **Sky height** (`Reach = 14`, `Bob = 0`). Use the plain ring sprite in blue so it looks different from the beacon rings. After a crash, Wren restarts at the last one she flew through, in the air, facing the way she was flying.

| Name                  | x, y        | Altitude |
| --------------------- | ----------- | -------- |
| Checkpoint: Teeth     | `546, 346`  | 120      |
| Checkpoint: Gorge     | `986, 676`  | 90       |
| Checkpoint: Patchwork | `1186, 946` | 80       |

---

### Part 12: Lantern balloons

Ten **Heart** objects `18 × 18` with **Healing removed**, **Score on touch** (`Points = 150`) and **Sky height** (`Reach = 12`, `Bob = 6`). Sprite: a round balloon with a little basket underneath; make every third one blue.

| #   | x, y         | Altitude |
| --- | ------------ | -------- |
| 1   | `511, 511`   | 90       |
| 2   | `691, 551`   | 140      |
| 3   | `991, 471`   | 120      |
| 4   | `1231, 681`  | 70       |
| 5   | `1531, 511`  | 200      |
| 6   | `1791, 511`  | 160      |
| 7   | `1551, 931`  | 90       |
| 8   | `1171, 1111` | 60       |
| 9   | `691, 831`   | 100      |
| 10  | `391, 851`   | 60       |

---

### Part 13: Playtest the whole game

1. Hold **Space** to about 60 speed, then **Up** to lift off. Level out at about 70 and fly east through Ring 1.
2. Bank left (north) toward the Teeth. Fly through the blue checkpoint ring, then thread the gap for Ring 2 at 150. Climb to 250 for Ring 3 above the peaks.
3. Turn south-east to the gorge. Come down to 90 for the checkpoint, then 60 and 50 for Rings 4 and 5 between the walls. Climb out before the end.
4. Turn north to Old Ember and climb to 280 for Ring 6 over the crater.
5. Head south-west, drop low over the sea for Ring 7 at 40, through the Patchwork checkpoint, and Ring 8 over the fields.
6. Fly east to Lantern Rock. Line up with the strip from the west, close the throttle with **X** until the speed is under 80, and let the nose drift down. Keep the wings level. "TOUCHDOWN" then the win screen.

Watch the panel: **SPEED** and **ALT** gauges on the left, the artificial horizon in the middle (it banks and pitches with you), the throttle lever, three warning lamps (pull up, landed, too slow) and the radar. "PULL UP" blinks when you are close to the ground and sinking.

#### Troubleshooting

| Problem                                   | Fix                                                                                                    |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| The game shows the flat map, not 3D       | Wren needs **Cockpit view**                                                                            |
| Wren falls or walks instead of flying     | Remove Gravity, Run and Jump from Wren                                                                 |
| Wren starts in the air, not on the runway | Her rectangle's middle must be on the Runway object                                                    |
| Space does nothing while Up is held       | Up counts as A too, so the throttle only opens without Up. Let go of Up, open the throttle, then climb |
| A ring is not collected                   | Fly through its middle at its altitude (the panel's ALT within its Reach)                              |
| Flying over a tree or house crashes       | Only the ground crashes the plane; touching scenery does nothing unless it has blocks                  |
| Landing always crashes                    | Slower than Safe landing speed, wings level, a gentle descent, and on the Runway itself                |
| The exit does nothing on landing          | All eight rings first; check they are Core objects with Collectible core                               |
| A landform will not get bigger            | One object is at most 320 × 240: overlap two                                                           |
| The game runs slowly                      | Lower View distance in Cockpit view                                                                    |

---

### Ideas to take it further

- A **Time limit** turns it into an air race: add **Time limit** to the hero and a `coreBonus` so each ring buys more time.
- Swap **Ground = Desert** and use Mesa landforms in Sand and Rock for a canyon run.
- Set **Cockpit = Heads-up display only** for a cleaner arcade look, or **None** for a bare view.
- Give **Cockpit view** to a top-down hero with **Top-down move** and no Flight controls: you walk through the map at eye level, which makes a first-person maze.
- Put a **Sky height** on a **Wave spawner**'s spawns or on moving objects with **Patrol path** to make targets that drift through the sky.

