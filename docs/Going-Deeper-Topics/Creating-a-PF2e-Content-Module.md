# Making a content module for the PF2E system on Foundry VTT

With Pathfinder Infinite's release many game designers are starting to put content out, and putting that content into a VTT can be daunting. But game designers, both professional and amateur, can see the benefit of making their content readily available to VTT users. This was the idea that motivated the PF2e Wayfinder and PF2e Expansion Pack modules, allowing users to add their content to the game easily with a direct link back to the store pages to support the talent making that material. This guide aims to take you from a passing familiarity with Foundry to releasing your own content module. It will go over the data entry tools that we use to save time for the system, as well as best practices for organizing and future proofing your module.

While this guide is written to help even someone new to Foundry succeed, it can't cover every eventuality and I don't want to write 5 pages on every small detail. If something is unclear please reach out on the Foundry discord in the `#module-development` channel, they are the experts in this and this guide is meant to only give the basic information. It will help to have some knowledge of HTML (very little is needed, just enough to use a few tags and close them properly) or at least a willingness to experiment.

This guide can also be useful for those wanting to do data entry for the system.

## Helpful tools

This guide can be followed without installing anything extra and just using notepad, Foundry, and your browser. However, that doesn't mean that it is ideal to only use those things.

The Quick Insert module in particular will be quite helpful in speeding things up for you. It allows you to quickly search or replace text with a link search every compendium (The default keybind is control-space but this can be changed in module settings).

You will in all likelihood want a better text editor than notepad. Heavy duty editors like Word won't work for this task as they are set up to write human readable documents and we want to make things readable by a computer instead. Developers will all have their favorite editor for this task, I personally use Sublime Text but it is likely overkill for this purpose, but Notepad++ will serve most users perfectly (both can be downloaded for free, Sublime has a free trial of unlimited length. Sublime text is notably more complex because it can do more). No matter what editor you decide on it will be helpful to have a json linter in them. These typically are available as add-ons. For Sublime text this add on is JsFormat, and for Notepad++ the JSTool plug in is what you want. The system's rule element UI has a built in editor that will verify JSON as well, if you don't want to download any additional software.

Next, we maintain a script to aid with data entry. This has a section dedicated to its use further down.

Two monitors. Not a joke, flipping from window to window can eat up a ton of time, might be worth a $10 cable to connect that old monitor to your laptop to have the extra screen space.

Multiple clipboards can also help. Being able to recover the thing you copied last can be a huge time save, on Windows you can enable this in the system clipboard settings and use it with windows-v.

One more tool that I find very helpful for big data entry tasks is Auto Hot Key (AHK). AHK lets you make your own macros, to perform routines with a key command. This simple script below can be saved as a .ahk file and after installing AHK and running the file hitting `control-g` on your keyboard will send the text after `Send,` out. This can save time if you constantly have to type the same text over and over on a huge project.

```plaintext
^g::
Send, <div data-visibility="gm">
return
```

## Step 1: Preparing the module

The easiest way to set up the module is using Foundry's built in module maker (v11 and up only). On the module's tab of Foundry you'll see a small button to create a module.

![image](https://github.com/foundryvtt/pf2e/assets/80183198/3f99efd5-efbe-42a3-879d-2fe76f81a7d0)

![image](https://github.com/foundryvtt/pf2e/assets/80183198/2e6c550e-1d94-45c9-bcc3-273096ed125b)

Fill out the details and add compendiums you need then enable it in your world and you're good to go. If you don't want to use this, want more control over the module, want to see what that module maker is doing, or are using v10 still see the instructions below for setting it up manually.

We want to set up the skeleton of the module first, so that we can add content to the module and link items as we go. mxzf on the Foundry discord has created a webtool that will auto generate a module skeleton for you to start with (https://fgen.lffg.org/module/create). If you want to make it manually or just want to see what each of the fields do then follow these steps: To start we will make a new folder to hold the module. We'll call our example module `PF2e Example` and name the folder `pf2e-example`, put this folder directly in the Foundry module folder as shown below.

![module_1](uploads/a8ffebfe15621b7cfd296a280b5649c1/module_1.PNG)

Next we will open that folder and add a new subfolder called `packs`, this is where all the database files will live. If you have art you will be including you can also make an `art` folder (make sure you can distribute it on VTT, not all licenses extend to all uses of media). These folders don't have to be named this way, but this is the standard format for modules so most of the advice you'll find will use these names. Now we need to make our `module.json` file.

![module_2](uploads/655743eefa4826173cedc56f16b6ac5f/module_2.PNG)

This **must** be there to be seen by Foundry. It is what tells Foundry what the module is and how to load the content from it. Once made lets paste this into the file:

```json
{
  "id": "pf2e-example",
  "title": "PF2e Example",
  "description": "An example module.",
  "url": "https://github.com/ExampleRepo/PF2e-Example",
  "version": "1.0",
  "authors": [
    {"name": "Author 1"},
    {"name": "Author 2"}
  ],
  "compatibility": {
    "minimum": "10.285",
    "verified": "10.285"
  },
  "relationships": {
    "systems": [{
      "id": "pf2e",
      "type": "system",
      "manifest": "https://github.com/foundryvtt/pf2e/releases/latest/download/system.json",
      "compatibility": {
        "minimum": "4.0.6"
      }
    }]
  },
  "packs": []
}
```

If you are not familiar with JSON format this serves as a way to see how they work before using them to provide some of the automation for the module. Change the author name to your name, removing the second author if needed. You will also want to set the PF2e system compatibility under relationships to the version of PF2e you made the module on, same for the core Foundry compatibility under the compatibility section. While you do not have to set minimum system or core versions it is a good idea to set them to the version you built the module on. This is because the actors and items will be using the schema of that version and are not guaranteed to be backwards compatible. There's a lot of other attributes to the `module.json` that we are not including. Most notably there is no manifest, url, or download attribute. These can be added later to allow the module to be downloaded through the Foundry interface and updated from a repo. For now let's just concern ourselves with making sure the module appears in Foundry. Launch (or relaunch, not refresh) Foundry and you should see the module in your installed modules list.

Now that our module is registering with Foundry let's add some packs so we can start filling them.

Edit `module.json` and find the line `"packs": [],`. We are going to fill that in with all the details of each pack

```json
"packs": [
    {
      "label": "Bestiary",
      "type": "Actor",
      "name": "example-bestiary",
      "path": "./packs/example-bestiary",
      "system": "pf2e",
      "package": "pf2e-example",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "ownership": {
        "PLAYER": "LIMITED",
        "ASSISTANT": "OWNER"
      }
    },
    {
      "label": "Feats",
      "type": "Item",
      "name": "example-feats",
      "path": "./packs/example-feats",
      "system": "pf2e",
      "package": "pf2e-example",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "ownership": {
        "PLAYER": "OBSERVER",
        "ASSISTANT": "OWNER"
      }
    },
    {
      "label": "Spells",
      "type": "Item",
      "name": "example-spells",
      "path": "./packs/example-spells",
      "system": "pf2e",
      "package": "pf2e-example",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "ownership": {
        "PLAYER": "OBSERVER",
        "ASSISTANT": "OWNER"
      }
    },
    {
      "label": "Maps",
      "type": "Scene",
      "name": "example-maps",
      "path": "./packs/example-maps",
      "system": "pf2e",
      "package": "pf2e-example",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "ownership": {
        "PLAYER": "LIMITED",
        "ASSISTANT": "OWNER"
      }
    },
    {
      "label": "Wrath of the Mitflits",
      "type": "JournalEntry",
      "name": "example-wrath-of-the-mitflits",
      "path": "./packs/example-wrath-of-the-mitflits",
      "system": "pf2e",
      "package": "pf2e-example",
      "banner": "systems/pf2e/assets/compendium-banner/red.webp",
      "ownership": {
        "PLAYER": "LIMITED",
        "ASSISTANT": "OWNER"
      }
    }
]
```

When you launch Foundry as long as the module.json is properly formatted Foundry will auto generate missing compendium files in the module directory.

Here I've included examples of packs of several different types. Packs can only contain one type of thing, an `Actor` compendium cannot contain items for example. It is worth noting that in Foundry terms an item is something that goes onto an actor. So equipment, feats, spells, ancestries, classes, effect, etc are all types of "item". This list would add a compendium that contains actors, two different item compendiums (feats and spells), maps, and journal entries. Now that the compendiums are made and linked in the module.json we can verify that the compendiums appear in Foundry (make sure to enable the example module in your world).

### Setting up Compendium Folders

Foundry v11 added new features that allow for modules to define custom folder structures for modules and systems. It's recommended you take advantage of these to hel keep the compendium tab cleaner. You can even add these module.json changes to a v10 compatible module and it will just serve as additional functionality for users on v11.

To add folder you add a new `"packFolders": []` array to the top level of the module.json (outside the `packs` array itself).

```json
"packFolders": [
  {
      "name": "Companion Compendia",
      "sorting": "m",
      "packs": [
        "ac-ancestries-and-class",
        "ac-feats",
        "ac-advanced-maneuvers",
        "ac-equipment",
        "ac-features",
        "ac-evolution-feats",
        "ac-eidolons",
        "ac-construct-breakthroughs",
        "ac-construct-companions",
        "ac-support"
      ]
  }
]
```

This is a simple example from the PF2e Companion Compendia module. This puts everything in a single folder. For the vast majority of content modules this will be all that is needed. The `sorting` line tells Foundry whether sorting should be automatic or manual. In this case we've set manual, so the compendiums will appear in the folder in the order they are listed. setting the argument to `a` instead will automatically sort them alphabetically. You can split the compendiums into further folders, like this example from the PF2e system itself.

```json
"packFolders": [
  {
    "name": "Pregenerated PCs",
    "sorting": "m",
    "packs": [
      "iconics",
      "paizo-pregens"
    ]
  },
  {
    "name": "Effects",
    "sorting": "m",
    "packs": [
      "bestiary-effects",
      "conditionitems",
      "campaign-effects",
      "equipment-effects",
      "feat-effects",
      "other-effects",
      "spell-effects"
    ]
  }
]
```

Each folder is its own object (In Javascript and JSON an "object" is denoted by being wrapped in `{}`, while "arrays" are wrapped in `[]`. The `packFolders` array is filled with objects). Because the system has more than 10,000 items to organize we have split content up into multiple folders. If you are making a very large module you may want to take this approach.

If your module is very complicated and large you can even make subfolders. Almost no module should require this however. The PF2e system for example defines its creatures folder like this

```json
"packFolders": [
  {
    "name": "Bestiaries",
    "sorting": "m",
    "folders": [
      {
        "name": "Adventure Paths",
        "sorting": "m",
        "packs": [
          "abomination-vaults-bestiary",
          "age-of-ashes-bestiary",
          "agents-of-edgewatch-bestiary",
          "blood-lords-bestiary",
          "extinction-curse-bestiary",
          "fists-of-the-ruby-phoenix-bestiary",
          "gatewalkers-bestiary",
          "outlaws-of-alkenstar-bestiary",
          "kingmaker-bestiary",
          "quest-for-the-frozen-flame-bestiary",
          "strength-of-thousands-bestiary",
          "stolen-fate-bestiary"
        ]
      },
      {
        "name": "Rulebooks",
        "sorting": "m",
        "packs": [
          "pathfinder-dark-archive",
          "book-of-the-dead-bestiary",
          "hazards",
          "lost-omens-impossible-lands-bestiary",
          "lost-omens-highhelm-bestiary",
          "lost-omens-mwangi-expanse-bestiary",
          "lost-omens-monsters-of-myth-bestiary",
          "lost-omens-travel-guide-bestiary",
          "npc-gallery",
          "vehicles",
          "blog-bestiary"
        ]
      },
      {
        "name": "Standalone Adventures",
        "sorting": "m",
        "packs": [
          "fall-of-plaguestone-bestiary",
          "malevolence-bestiary",
          "menace-under-otari-bestiary",
          "one-shot-bestiary",
          "shadows-at-sundown-bestiary",
          "the-enmity-cycle-bestiary",
          "the-slithering-bestiary",
          "troubles-in-otari-bestiary",
          "night-of-the-gray-death-bestiary",
          "crown-of-the-kobold-king-bestiary"
        ]
      },
      {
        "name": "Pathfinder Society",
        "sorting": "m",
        "packs": [
          "pfs-introductions-bestiary",
          "pfs-season-1-bestiary",
          "pfs-season-2-bestiary",
          "pfs-season-3-bestiary",
          "pfs-season-4-bestiary"
        ]
      }
    ],
    "packs": [
      "pathfinder-bestiary",
      "pathfinder-bestiary-2",
      "pathfinder-bestiary-3"
    ]
  }
]
```

This large and complicated example is a single folder called "Bestiaries", with Bestiary 1, 2, and 3 directly in that folder. Then it defines subfolders to contain the compendiums for adventure paths, rulebooks, etc.

### Setting up Module Images

With Foundry v11 we also have custom images for both module thumbnails and compendium modules. The module thumbnail will show on the main Foundry page when looking at installed modules. You can set it in module.json with the new `media` array. This setup image should be 600 x 400 pixels.

```json
"media": [
  {
      "type": "setup",
      "caption": "Pathfinder 2e Companion Compendia",
      "thumbnail": "modules/pf2e-animal-companions/art/preview.webp"
}]
```

There are also the compendium background banners. You can see these in the `packs` example above, being set to one of the system's images.

```json
"banner": "systems/pf2e/assets/compendium-banner/red.webp"
```

You can, of course, make your own and include it in your module. These images are shown in two places at slightly different aspect ratios. Best practice is to make the images 696 x 168 pixels and expect them to be slightly horizontally clipped in some instances. You can define different banners for different compendiums, or leave that property out entirely and Foundry will use the default images for that compendium type.

### Setting up Traits, Damage Types, and Skills

You can pre-register traits in the system inside your `module.json` by adding a structure for flags.  If you will be heavily relying on custom traits this will prevent you from having to shove multiple traits into the custom field on items, it will also make the items work nicely and give fancy descriptions.

**Be sure to change "pf2e-expansion-pack" to your module name. Trait keys must be in kebab-case. Do not use capital letters in the key.**

```json
"flags": {
  "pf2e-expansion-pack": {
    "pf2e-homebrew": {
      "baseWeapons": {
        "nashi-bomblobber": "Nashi Bomblobber",
        "nashi-flamebelcher": "Nashi Flamebelcher"
      },
      "creatureTraits": {
        "bugbear": "Bugbear",
        "dragonkin": "Dragonkin"
      },
      "damageTypes": {
        "radiation": {
          "label": "Radiation",
          "category": "energy",
          "icon": "fa-radiation"
        }
      },
      "equipmentTraits": {
        "body-paint": {
          "label": "Body Paint",
          "description": "<p>A body paint is...</p><p>Second Paragraph...</p>"
        },
        "cartridge": {
          "label": "Cartridge",
          "description": "Alchemical cartridges are..."
        }
      },
      "featTraits": {
        "bugbear": "Bugbear",
        "yroometji": "Yroometji"
      },
      "spellTraits": {
        "tanuki": "Tanuki"
      },
      "weaponTraits": {
        "capacity-2": {
          "label": "Capacity 2",
          "description": "PF2E.TraitDescriptionCapacity"
        },
        "return": {
          "label": "Return",
          "description": "A weapon with this trait..."
        }
      }
    }
  }
}
```

Notice that you can simply use `"trait-slug": "Trait Name"` to register the trait, but you can also use an object structure to provide a description.  These descriptions show up on strikes, items, feats, etc when hovered over on a sheet.  This lets you give descriptions to new traits inside the system in a very sleek way.

![image](uploads/467097e7937ae013a2af1c7bc33ee57a/image.png)

You can use the system translation keys (or even your own if your module supports that, though that is outside the scope of this document). The above example registers capacity 2 to use the system's capacity trait description at a number that doesn't exist in the system. These traits will not have automatic function however, for example you could register `Fatal d20` but this will not automatically add a d20 on critical hits.  For things like that rule elements would be needed on the items to mimic a working trait.

Damage types can be registered, but they will not show up if no `category` is set, the category must be either `energy` or `physical`. You can set an icon using the Font Awesome fonts, you can search on the Font Awesome website (https://fontawesome.com/search) to find the icons you want. The PF2e system includes the free version of FA6 only.

As of PF2e 6.1, new skills can be registered by modules. Each skill needs a label and attribute defined for it, like follows
```json
"pf2e-homebrew": {
  "skills": {
    "additional": {
      "computers": {
        "label": "Computers",
        "attribute": "int"
      },
      "piloting": {
        "label": "Piloting",
        "attribute": "dex"
      }
    }
  }
}
```

The full list of registerable traits and items are as follows:

```json
[
        "classTraits",
        "creatureTraits",
        "damageTypes",
        "featTraits",
        "languages",
        "magicSchools",
        "skills",
        "spellTraits",
        "weaponCategories",
        "weaponGroups",
        "baseWeapons",
        "weaponTraits",
        "equipmentTraits",
        "shieldTraits",
]
```

For a full example of how this is used see the module.json of the PF2e Expansion Pack module.json file.

### Mapping Compendium Art

If your module contains art to map to the *system* compendiums, such as a pack of top down tokens for monster art, you can use the module.json to tell the system where to find your mapping file, and the mapping file contains the explicit mapping by id and compendium for each creature you want to add art to. This is no longer a PF2e exclusive feature, and uses a core Foundry method that differs from how the system had implemented it.

In the mdoule.json under flags add a section for your module and what systems it maps to, like in this example from the `pf2e-tokens-bestiaries` premium module.

```json
"compendiumArtMappings": {
  "pf2e": {
    "mapping": "modules/pf2e-tokens-bestiaries/map-pf2e.json",
    "credit": "<em>Portrait, token, and subject artwork from Pathfinder Tokens: Bestiaries.</em>",
    "priority": 10000000
  }
}
```

The mapping file itself is then formatted separately in a file (in the above example called `map-pf2e.json` but you can name it anything you want. For each compendium and for each creature in that compendium you provide an object, like so:

```json
"pf2e.pathfinder-bestiary": {
  "fgsDAeZHVbHRhSE8": {
    "actor": "modules/pf2e-tokens-bestiaries/portraits/bestial/mythological/cockatrice.webp",
    "token": {
      "randomImg": true,
      "texture": {
        "src": "modules/pf2e-tokens-bestiaries/tokens/bestial/mythological/cockatrice.webp",
        "scaleX": 1.2,
        "scaleY": 1.2
      },
      "ring": {
        "enabled": true,
        "subject": {
          "texture": "modules/pf2e-tokens-bestiaries/subjects/bestial/mythological/cockatrice.webp",
          "scale": 1.5
        }
      }
    }
  },
  "zUvgAbgeQH5t6DWs": {
    "actor": "modules/pf2e-tokens-bestiaries/portraits/divine/angel/choral.webp",
    "token": {
      "randomImg": true,
      "texture": {
        "src": "modules/pf2e-tokens-bestiaries/tokens/divine/angel/choral.webp",
        "scaleX": 1.2,
        "scaleY": 1.2
      },
      "ring": {
        "enabled": true,
        "subject": {
          "texture": "modules/pf2e-tokens-bestiaries/subjects/divine/angel/choral.webp",
          "scale": 1.5
        }
      }
    }
  }
}
```

As you might expect, this is an object that is difficult and tedious to build by hand. It may help to map the art by hand in Foundry then use a script in console to return the desired object.

### Customizing journal CSS

It's possible to create a custom CSS style for your journals. The actual how-to of CSS is far beyond the scope of this wiki, however custom journals with a very light styling touch were included as an example in the Hopefinder module (https://github.com/TikaelSol/hopefinder) for others to copy from and learn. The actual code for injecting the styling is small enough that most motivated module makers, even ones not familiar with JS can get it working. Good luck with the CSS though.

## Step 2: Inputting the content

This will be the longest step, while making a module work in Foundry the first time can be a daunting task especially if you've never worked with computers at this level it usually doesn't take longer than actually entering the data into foundry itself. This will be the section that details the use of the data entry script(s), but first I want to give some general advice.

**Take Breaks** If you sit down to enter your 2 classes, 10 ancestries, and 75 monsters into Foundry understand that it is going to take a substantial amount of time. Even when you are rather quick at data entry it can easily take an hour to put in 4 or 5 NPCs, and more complicated one can take an hour in and of themselves. You don't want to sit hunched over a keyboard for 16 hours on a weekend to rush something out. Do work in batches and set goals.

**Make Backups!** if you spend 30 hours setting up a complex module brimming with content nothing is worse than your computer crashing, getting stolen, or even you just messing up the last steps of the module and overwriting the completed module with a blank copy from the online repository. Do yourself a favor and make backups, while your foundry world should also have the data you don't want to have to do the last steps again.

You will be making new content in Foundry itself, directly in the world by clicking the new item button in the item directory sidebar. To that end it will help to organize yourself first. You don't have to do data entry this way, but the idea is to set yourself up to do at little repetitive work as possible. When I sit down to do a large amount of data entry I first pre make all of the items/actors as blank entries. For example, when inputting a new ancestry or class we are going to be adding potentially dozens of feats. So I make a single feat, name it `1` set the level to 1, give it the traits of the ancestry or class, set the feat type to ancestry/class/etc feat as appropriate then change the feat image to `feats.webp` (the book image) from the default feat image (it's not this by default because the 'feat' item handles more than just pf2e feats, it's also ancestry and class features, etc). Once this feat is set up I count out the feats I need: 5 level 1, 4 level 2, .... I then make 5 copies of the feat by right clicking and copying, change the feat's name to 2 and level to 2 then make 4 copies of that, change the name and level to 4, .... This gives me all the feats I need with numerical names so the empty feats are sorted.

For actors make a blank actor with the right name, level, and traits and put them in a folder for unfinished NPCs. Once this base is set up you can go from the top to the bottom moving actors out into a different folder for finished NPC. If you have a lot of NPCs try breaking them into subfolder of 10 or so to reduce scrolling.

Whatever rhythm you find that works for you try to find ways to improve it.

Lastly, as you enter data keep a notepad, Foundry journal, sticky note, etc nearby and make notes about what needs linked or finished. If you create a spell or feat and need to link a new action or effect to them in the description you cannot do this correctly until the items are in the module compendium, if you create the link beforehand then the link points to the item in your world which will be a broken link in any other Foundry world.

### Common Abilities

When entering NPCs you will find many common NPC abilities in the compendiums already. In the Bestiary Ability Glossary compendium you can find things like negative healing, +1 status bonus to saves vs magic, improved grab, frightful presence, etc. In the Bestiary Family Ability Compendium you will find abilities associated with creature families like ghost rejuvenation or golem anti-magic. These abilities are pre set up for you with blank data that you can edit to change the DCs, ranges, and damage rolls. You will also notice that if you edit these the text is provided with `@Localize`, this pulls the text of this ability from the language files of the system, meaning that these abilities will be auto translated for users of other languages. Vastly cutting down on translation overhead, but also allowing us to immediately correct typos in the text by editing the source text stored in the system's `en.json` file. Please use these abilities when making NPCs instead of making your own copies with identical text.

Note that PC feats cannot be put on NPCs, if an NPC has a PC ability or feat you can copy the text from that feat but do not drag the feat to the NPC actor. The feat will not be added, this stems from NPCs simply being simpler actors and the rules of PF2e being such that NPCs may mimic PCs but they are not built with the same guidelines or rules.

### Action and Feat Categories

On the details tab of actions and feats you will find different options to select. You can select the action type and number of actions, set the action category, and set traits. The sidebar on feats lets you pick the type of feat (skill, class, ancestry, class feature, etc) as well as setting the level of the feat. Make sure these details are correct, if the feat is an action then setting it as such automatically creates the action on the actions tab for the character. The thing that normally trips up people here is the action category. An action that adds +2 to AC seems defensive but note that we define the action category by the region of the NPC statblock the ability appears in.

![module_7](uploads/1445311ca9c6fb6ba69b03448d86c886/module_7.PNG)

This diagram shows the 3 regions of the NPC sheet, and how the actions on them are categorized. If we implement a "Paizo Style" NPC sheet the organization will use this data to sort itself.

### The Data Entry Script

<https://github.com/TikaelSol/PF2e-Foundry-Data-Entry>

To speed up our lives we have a script to do the heavy lifting for us. This does not copy an entire actor into Foundry, rather it takes the raw text from their abilities and makes the formatting nicer to be displayed. There are three ways to run this script: The standalone program, using the python script, or a Jupyter notebook (Jupyter or Binder). The standalone program requires no installation and is simply the python version of the script with a UI applied and wrapped in an exe file using pyinstaller. Simply download dataEntry.exe from the github link above and run it.

![image](uploads/412e882f9f1971b4d2521f014c8b1991/image.png)

Paste the text on the left side and click reformat text. The input text can be cleaned up or altered as you want before reformatting it. There are options to handle more specific content, such as some font elements used in certain third party PDFs, animal companion or eidolon stat blocks, or ancestry descriptions (for detecting the "You might..." and similar text). These may not work on every PDF and can stay unchecked if you aren't sure or are getting bad results from them.

If you want to modify the script to handle your specific content faster the .py file is available and can be run in any python environment.

### Using the formatted text

Now that you have the formatted text open the feat/ability/etc and edit the description, then open the HTML source with the `<>` button. Past the formatted text in and audit it. Make sure the script got all the inline rolls, look at our style guide and follow the best practices laid out in that. If there are inline saves that need traits added add them, if an effect is a basic saving throw the script knows to add the `damaging-effect` trait but for non basic save damaging effects it will need to be added. The script does what it can with lists, but unordered lists lack a close `</li></ul>` set of tags, find where these go and place them. It will also help to add appropriate `<strong></strong>` tags around bolded words.

### Spell entry

For spellcasters grabbing links for a dozen spells can be tedious. To help reduce this a macro exists to help speed things up a bit by attempting to fetch the links and outputting them to chat. It's available at this gitlab snippet https://gitlab.com/-/snippets/2147203 and can be ran from inside Foundry like any macro. To use it copy the entire spell block, from the `arcane prepared spell` to the last cantrip. Then paste that into the macro window. The macro will attempt to find links to as many spells as it can and output them to chat. They can be dragged from there onto the spellcasting entry like any spell link.

### Automating the content.

The script adds most of the inline automation for you, but rule elements need to be added to automate things. This guide cannot possibly go into depths on how to automate things. Rule elements will be the main tool of automation, see the rule element guide on this wiki, and look to similar abilities or items for guidance on automation. If all else fails feel free to ask in the PF2e Foundry discord for help. I suggest using the rule element generator and keeping a scratch notepad full of rule elements you can quickly alter and copy.

## Step 3: Finishing the Module

Once you have created the content you can send it to your new compendiums. Unlock the module compendiums for editing (right click and unlock in the compendiums tab). If the content is in a folder you can right click that folder and export to compendium (be careful that you select the correct compendium). Otherwise you can drag it from the items sidebar into the open compendium window.

Once your items and actors are in compendiums you can open your to do list and begin making the links to items. Once these are all complete add any art you are using to your module and you are almost ready to make the module available.

Before sending the module off remember that you need to include the OGL license for your module, if you have licensed art you may need another license file with the correct licensing and sourcing for that. Create those files, using the files from the system or other modules as a guide (note in the OGL license how it lists each source book used and the authors).

**Now is a great time to make a backup of your module data!**

Once you have this done you can make a repo for your module. I prefer working with github, but this obviously makes the module freely available. For information about making your module premium I suggest reaching out to Foundry, this is something I don't have any experience in. The rest of this guide assumes you are using github.

Create a new github repository then open `module.json`. We are going to add some lines (remember the `,` at the end of every line except the last)

```json
  "url": "https://github.com/YourName/PF2e-Example",
  "manifest": "https://raw.githubusercontent.com/YourName/PF2e-Example/master/module.json",
  "download": "https://github.com/YourName/PF2e-Example/archive/refs/heads/main.zip"
```

Replace the `YourName` and `PF2e-Example` with your github user name and repo name respectively. This will point your module.json at the repo. Now add all your module files directly to the root of your github (you can just drag them into the repo on your browser).

Once on github move the module's folder out of the Foundry data structure, reboot Foundry and use the manifest url to install your module manually in Foundry as a test. If the module installs check that the content loads in a world. If it did you are done with all the hard work and congratulations!

The final step is to get the module listed on Foundry so anyone can install it without needing the manifest URL. Fill out this form <https://foundryvtt.com/packages/submit> on the Foundry site, it can take a few days to get back to you once submitted so at this point take a break and be patient.