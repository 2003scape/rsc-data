# rsc-data
collection of data for runescape classic. probabilities for drops, entity
spawn information, skill data, etc. useful for game server-sided applications or
analysis/simulations.

## install

    $ npm install @2003scape/rsc-data

## certificates.json
https://classic.runescape.wiki/w/Certificate

```javascript
{
    "items": {
        "151": 517, // original item index: certificate item index
        // ...
    },
    "certers": {
        "229": { // index of config/npcs.json
            "type": "fish", // used for NPC dialogue
            "certificates": [ // menu used for certificate -> item
                {
                    "id": 630, // item index of certificate
                    "alias": "shark" // text used in client menu
                }, // ...
            ],
            "items": [ // menu used for item -> certificate
                {
                    "id": 555, // original item index
                    "alias": "bass"
                }, // ...
            ]
        }, // ...
    }
}
```

## config/
dumped and converted .jag archive files from the official client.
see https://github.com/2003scape/rsc-config#api

## edible.json
```javascript
{
    "257": { // item id to eat
        "hits": 5, // amount of hits healed
        "result": 263, // optional resulting item ID
        // optional message(s) if different than the default
        "message": "You eat half of an apple pie"
    },
    // entries can also just be integers if they have no special behaviour
    "373": 12
}
```

## landscape/
.jag archive files used on the server for collision detection. see
https://github.com/2003scape/rsc-landscape

## locations/items.json
```javascript
[
    {
        "id": 10, // index of config/items.json
        "amount": 10, // item amount (optional and only for stackables)
        "respawn": 60000, // ms for respawn after being picked up
        "x": 147,
        "y": 3337
    }, // ...
]
```

## locations/npcs.json
```javascript
[
    {
        "id": 0, // npc index
        "x": 185,
        "y": 661,
        "minX": 175,
        "maxX": 195,
        "minY": 651,
        "maxY": 671
    }, // ...
]
```

## locations/objects.json
```javascript
[
    {
        "id": 1, // index of config/objects.json
        "direction": 0, // direction from 0-6 in TRBL/NESW order
        "x": 346,
        "y": 554
    }, // ...
]
```

## npc-respawn.json
```javascript
{
    "0": 211000, // npc index -> milliseconds for respawn after death
    // ...
}
```

## regions.json
boundaries and spawn points for different cities and areas.

```javascript
{
    // can also be array of objects if area covers multiple rectangles
    "varrock": {
        "minX": 50,
        "maxX": 444,
        "minY": 180,
        "maxY": 565,
        "spawnX": 120, // used for teleportation
        "spawnY": 504
    }, // etc.
}
```

## rolls/
probabilities for various randomized aspects, such as NPC drops or christmas
cracker rates. each roll is between 0-128, so the total weight per individual
drop table cannot exceed 128. if the total weight is less than 128, there is a
chance to drop nothing besides the entries with 0 weight.

```javascript
{
    "11": [ // npc index or table name
        {
            "id": 20, // item index
            "weight": 0 // if 0, always drop
        },
        {
            "id": 10,
            "amount": 3, // amount (multiple are dropped if non-stackable)
            "weight": 38 // numerator out of 128
        },
        {
            "reference": "herb", // reference another NPC id or table within
            "weight": 23
        },
        {
            "id": [827, 273], // drop 2 items in the same entry
            "weight": 10
        } // ...
        // implicit 43/128 nothing roll (besides 0-weighted entries; bones)
    ], // ...
}
```

## shops.json
https://classic.runescape.wiki/w/Category:Shops

```javascript
{
    "al-kharid-general": {
        "items": [
            {
                "id": 135, // item index
                "amount": 3
            }
        ],
        // the multipliers of item.price used when items are at default stock
        "sellMultiplier": 130,
        "buyMultiplier": 40,
        "delta": 3, // amount multipliers change when over/under stock
        // milliseconds until restock tick, which increases understocked items
        // by 1 annd decreases over-stocked or player-added items by 1
        "restock": 12000,
        "general": true // true if players can sell any tradeable item
    }, // ...
}
```

## skills/cooking.json
https://classic.runescape.wiki/w/Cooking

```javascript
{
    // the item IDs of uncooked items that gauntlets can affect, and how much
    // they increase the roll chances
    "gauntlets": {
        "369": 1.05, // ...
    },
    "uncooked": {
        "133": { // uncooked food index
            "level": 1, // required level
            "experience": 120, // experience received on success
            "cooked": 132, // cooked item index, received on success
            "burnt": 134, // burnt item index, received on failure
            "roll": [128, 512], // x/256 of success at level 1 and 99
            "range": false // is a range required?
        }, // ...
    },
    // inventory item combinations (use item on item)
    "combinations": [
        {
            "level": 35, // required level to perform
            "item": 320, // the first item index to be used on...
            "with": 321, // the second item index
            "result": 323, // the result item index of combining items
            "knife": false, // do we kneed a knife to do this?
            "message": "You add tomato to the pizza"
        }, // ...
    ]
}
```

## skills/crafting.json
https://classic.runescape.wiki/w/Crafting

```javascript
{
    // gem cutting; results of using a chisel on uncut gems
    "cutting": {
        "157": { // uncut gem item index
            "level": 43, // required level
            "experience": 430,
            "id": 161 // cut gem item index
        }, // ...
    },
    // leather sewing: result of using leather with needle+thread in inventory
    "leather": [
        {
            "level": 14,
            "experience": 100,
            "id": 15, // item index of completed leather item
            "alias": "Armour" // text used in client menu
        }, // ...
    ],
    "pottery": [
        {
            "level": 4,
            "unfired": {
                // item index from using item on pottery wheel
                "id": 278,
                "experience": 60
            },
            "fired": {
                // item index of from using unfired item on pottery oven
                "id": 251,
                "experience": 40
            },
            "alias": "pie dishes" // text used on menu prompt
        }, // ...
    ],
    "silver-jewellery": [ // silver bar on furnace
        "moulds": [386, /* ... */], // the moulds for each item below
        "items": [
            { "level": 16, "experience": 200, "id": 44 }, // ...
        ],
    ],
    "gold-jewellery": [ // gold bar on furnace
        // same as silver-jewellery
    ],
    "stringing": { // string on unstrung amulet, unstrung -> strung
        "1027": 1028, // ...
    },
    "glassblowing": [ // using glassblowing pipe on molten glass
        {
            "level": 46,
            "experience": 210,
            "id": 611, // item index of result
            "alias": "orb" // text in menu prompt
        }, // ...
    ],
    "battlestaves": { // using powered orb item on battle staff item
        "612": {
            "level": 62,
            "experience": 500,
            "id": 615
        }, //...
    }
}
```

## skills/fishing.json
https://classic.runescape.wiki/w/Fishing

```javascript
{
    "spots": {
        "193": { // fishing spot object index
            "net": { // object command name
                "tool": 376, // item index required to fish (net, rod, ...)
                "bait": null, // stackable item index depleted after success
                "fish": {
                    "349": { // item index of raw fish caught using the command
                        "level": 1,
                        "experience": 40,
                        "roll": [48, 256]
                    }, // ...
                }
            }, // ...
        }, // ...
    }
}
```

## skills/fletching.json
https://classic.runescape.wiki/w/Fletching

```javascript
{
    "bows": {
        "14": [ // log item index
            // shortbow, longbow
            {
                "level": 5,
                "experience": 20, // experience gained for cutting and stringing
                "unstrung": 227, // item index after using knife on log
                "strung": 189 // item index after using string on unstrung
            },
            {
                "level": 10,
                "experience": 40,
                "unstrung": 276,
                "string": 188
            }
        ], // ...
    }
}
```

## skills/herblaw.json.
https://classic.runescape.wiki/w/Herblaw

```javascript
{
    "herbs": {
        "165": { // unidentified herb item index
            "level": 3,
            "experience": 10,
            "id": 44 // ID of identified herb
        }, // ...
    },
    "unfinished": { // results of using identified herb on vial of water
        "444": { // identified herb item index
            "level": 3,
            "id": 454 // unfinished potion item index
        }
    },
    "potions": { // results of using items on unfinished potions
        "454": { // unfinished potion item index
            "270": { // secondary item index
                "level": 3,
                "experience": 100,
                "id": 474 // finished potion item index (3 dose)
            }
        }
    }
}
```

## skills/magic.json
https://classic.runescape.wiki/w/Magic

```javascript
{
    "spellExperience": [
        // experience gained on successful spell. index corresponds to
        // config/spells.json
        88, // ...
    ]
}
```

## skills/mining.json
https://classic.runescape.wiki/w/Mining

```javascript
{
    "pickaxes": {
        "156": { // pickaxe item index
            "level": 1,
            "attempts": 1 // attempts the pickaxe performs before success
        }, // ...
    },
    "gem": [ // gem roll rates (1/256 to get a gem from a rock)
        {
            "id": 160, // item index
            "weight": 64  // roll out of 128
        }, // ...
    ],
    "rocks": {
        "100": { // game object index
            "level": 1,
            "experience": 70,
            "roll": [48, 256],
            // item index, or array of weighted item entries (for gem rocks)
            "ore": 150,
            "respawn": 5400, // milliseconds until depleted turns into original
            "depleted": 9 // game object index of rock after success
        }, // ...
    }
}
```

## skills/prayer.json
https://classic.runescape.wiki/w/Prayer#Bones

```javascript
{
    "buryExperience": { // bone id -> experience received
        "20": 15, // ...
    }
}
```

## skills/smithing.json
https://classic.runescape.wiki/w/Smithing

```javascript
{
    "smelting": {
        "169": { // smelted bar item index
            "level": 1,
            "experience": 25,
            "ores": [{ "id": 150 }, { "id": 202 }]
        }, // ...
    },
    // there's always the same menu tree for every bar
    "smithing": {
        // amount of bars needed for each item below, using the menu tree order
        "bars": [
            [1, 1, [1, 2, 2, 2, 3], [1, 3], 1], // ...
        ],
        "items": [
            "169": { // smelted bar item index
                // base experience - multiplied by amount of bars required
                "experience": 50,
                "items": [ // follows the same tree as smithing.bars above
                    {
                        "level": 1,
                        "id": 62,
                        // only specified to override or if this is an
                        // additional menu (which bronze and steel bars have)
                        "bars": 1,
                        // only specified to override the bar experience rate.
                        // give constant experience for this item only
                        "experience": 100
                    }, // ...
                ]
            }, // ...
        ]
    }
}
```

## skills/thieving.json
https://classic.runescape.wiki/w/Thieving

```javascript
{
    "pickpocket": {
        "11": { // npc index
            "level": 1,
            "experience": 32,
            "roll": [180, 240],
            // item indexes received on success (optionally weighted)
            "items": [{ "id": 10, "amount": 3 }],
            // dialogue on failure
            "exclaimation": "Oi what do you think you're doing"
        }, // ...
    },
    "stalls": {
        "322": { // stall object index (with steal-from command)
            "level": 20,
            "experience": 96,
            // reward for successful theft (optionally weighted)
            "items": [{ "id": 200 }],
            // how long it takes to steal from the depleted stall to respawn
            "respawn": 8000,
            "owner": 326, // the shopkeeper of the stall
            // npc indexes of nearby NPCs who prevent theft
            "guards": [322, 321]
        }, // ...
    },
    "chests": {
        "334": {
            "level": 13,
            "experience": 30,
            "items": [{ "id": 10, "amount": 10 }],
            "lockpick": false,
            "respawn": 10000
        }, // ..
    },
    "doors": {
        "93": { // index of config/wall-objects.json
            "level": 7,
            "experience": 15,
            "lockpick": false, // do we need a lockpick in inventory?
            // positions where we can open the door freely
            "exits": [{ "x": 539, "y": 531 }]
        }, // ...
    }
}
```

## skills/woodcutting.json
https://classic.runescape.wiki/w/Woodcutting

```javascript
{
    "axes": {
        "12": 1.5, // item index -> roll multiplier
        // ...
    },
    "trees": {
        "0": {
            "level": 1,
            "experience": 100,
            "roll": [64, 200],
            "log": 14, // item index received on success
            "respawn": 40000, // milliseconds until stump turns back into tree
            "stump": 100 // object index tree turns into when cut
        }, // ...
    }
}
```

## wieldable.json
wieldable item bonuses and skill requirements. the wielded slots are specified
in *config/items.json*.

```javascript
{
    "0": { // item index
        "female": false, // can only female characters wear this?
        "animation": 118, // the client-sided animation index
        "armour": 0,
        "weaponAim": 8,
        "weaponPower": 6,
        "magic": 0,
        "prayer": 0,
        "requirements": { // optional
            attack: 30 // skill names -> base level required
        }
    }, // ...
}
```

# license
[RuneScape Classic Wiki](https://classic.runescape.wiki):
> Content on this site is licensed under
> [CC BY-NC-SA 3.0](https://creativecommons.org/licenses/by-nc-sa/3.0/legalcode);
> additional terms apply.
> RuneScape and RuneScape Old School are the trademarks of Jagex Limited and
> are used with the permission of Jagex.

original game data definitions (in *config/*):
> &copy; 2001-2002 Andrew Gower and Jagex Ltd

CC-BY-SA-4.0
https://creativecommons.org/licenses/by-sa/4.0/legalcode
