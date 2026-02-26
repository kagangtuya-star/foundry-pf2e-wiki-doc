# Introduction

Rule elements are how the PF2e system handles most of the basic automation for feats, equipment, abilities, or effects. Rule elements can add modifiers, adjust modifiers, change the token image, change actor data, add toggles to actors, give feats, and so much more.

## What is a Rule Element?

A series of instructions in JavaScript Object Notation (JSON) that can be applied to an item, when that item is then put on an actor it modifies the character sheet in some way.  It's another way to automation without coding! We use them as the preferred way to handle automation in the system, as they do not require hard coding into the actual system code. Despite looking a little odd at first glance they are much easier and more flexible to write than macros, and the system is building out a UI for rule elements to help you make them even easier.

Rule elements are stable, if you have an error or make a mistake nothing should break, but still some of the more advanced rule elements could in theory damage actors or cause Foundry to hang and require a restart. Take care when adding rule elements, make sure to do any troubleshooting/experimenting on a test actor.

We have a video going over a lot of rule element basics, which you may find to be a good supplement to this wiki:

[![Link to the introductory video on rule elements](https://github.com/foundryvtt/pf2e/assets/80183198/508a1d1f-4ad9-48c5-a132-4115d90e5f46)](https://youtu.be/aqFDiVolAxI "An introduction to rule elements for Pathfinder 2e on Foundry VTT")

## How do rule elements work?

1. By default the GM can always see the rules tab on items. If you want to allow players to see this tab as well you can enable it in the system settings.
2. Try some out, head to the compendia, and find spell effects, pick one and drag it to your character.
    - Courageous Anthem is a good place to start.
3. Open up the spell effect you chose and look at Rules Tab notice text in the boxes, that is a rule element
4. If your effect has multiple rules you will see that each rule has its own slot on the item. You can't put more than one rule in a slot.
A correctly created rule element will do the work of several lines of macro. Once you get the hang of them it allows for easy automation.

Before we start breaking them down we should clarify some terminology.

## Important Terminology

**Item** This document will often refer to `items`, items are feats/ class features, spell effects, weapons, etc. In Foundry terms an item is something that goes onto an actor. In game terms we tend to think of items as things your actor carries or wears, like weapons, armor, consumables, and the like. To help keep it clear we will refer to those as physical items if we need to specifically call them out.

**Strings, Numbers, and Booleans** A string is one or more characters wrapped in double quotes. It's important to know that in the weird world of JavaScript 2 and "2" are very different things. 2 + 2 = 4, but "2" + "2" = "22". This won't come up too often, but in general you should not wrap numbers in quotation marks unless you do want them to be a string.

A number is a number, I'll assume you can identify these.

A Boolean is `true` or `false`. Even though to us these are words, and you'd be tempted to treat them as strings, to JavaScript they are their own type of thing and do not get wrapped in quotation marks.

**Array** An array is a collection of things and in rule elements is always surrounded by square brackets `[]`. Each element in an array is separate by a comma. An array can have any kind of thing stored inside it, most commonly it will be an array of strings (`["agile", "elf", "item:tag:witch-patron"]`) but you can also store objects in an array, which can even include arrays inside the object (`[{"or": ["agile", "elf", "item:tag:witch-patron"]}]`). How these should be read will make more sense when you see examples later in this guide.

**Object** An object is wrapped in curly braces `{}` and has parameters with values. A rule element itself is a type of object, so are Foundry actors (export one and look at the JSON file that comes out). Sometimes the values of rule element parameters are themselves objects, which can make the rules appear intimidating, but we have tools to help simplify working with them. Objects can be entirely arbitrary, here's an example using all of the above value types:

```json
{
    "parameterOne": "The first parameter is a string",
    "parameterTwo": {
        "subParameter": "A subparameter of parameter two",
        "secondSubParameter": "Another subparameter"
    },
    "parameterThree": [
        "Parameter three",
        "is an",
        "array of strings!"
    ],
    "parameterFour": 4,
    "parameterFive": true
}
```

Take note that after each parameter we have a comma, except for the last parameter. Commas only go between parameters or entries in an array to tell the system to expect another thing. If you are writing large rule elements, or having trouble with a smaller one you are writing it can help to use an external tool to verify the JSON object. [JSONLint](https://jsonlint.com/) is a website that can help point out problems with your formatting of the JSON (It has nothing to do with Foundry, JSON is a commonly used data storage method for a lot of programming languages). External editors like Sublime Text, Notepad++, or Visual Studio also work with JSON data, but the use of them is beyond this guide.

**Case** Often the way you write something in a rule element matters. The two cases that will be important in writing rule elements are `camelCase` (Also called dromedary case) and `kebab-case`. Notice that all of the parameter keys in the above object example are written in camel case. This will be true of all rule elements, their parameters will be camel case, while the values of those parameters may need to be kebab case. To put something in one of these cases we should take the string and remove spaces and punctuation. For camel case we capitalize every word but the first, and for kebab case we replace spaces with dashes.

`Dave's awesome sword (of demon slaying)` would turn into `davesAwesomeSwordOfDemonSlaying` (camel) and `daves-awesome-sword-of-demon-slaying` (kebab).

**Slug** A slug is an identifying string for something in Foundry. Slugs are always kebab case, and by default the slug for an item or modifier is the kebab case version of the item name (You will see people refer to this as the 'sluggified' name). Every modifier to a roll has a slug assigned to it, and we can even reference slugs for those modifiers to alter them.

## My First Rule Element

We're going to use flat modifier rule elements for most of our examples, since they can be quite simple. In Foundry you will see that flat modifiers have a form which makes writing them much easier, but not all rules have forms yet and you can use the full JSON object to write flat modifiers just like other rule elements.

Let's start with something simple. This rule element adds a +2 item bonus to athletics rolls.
```json
{
    "key": "FlatModifier",
    "selector": "athletics",
    "type": "item",
    "value": 2
}
```

Let's break each of these parameters down:
1. Key: This is the name given to the rule element within the code. This is necessary for *all* rule elements, as it tells the system what kind of thing your rule is even supposed to be.
2. Selector: This selects what to apply your modifier to.
    - See the selectors section for an in depth discussion of these and how to find them.
    - An incorrect selector will make your rule do nothing
3. Type: This is the modifier type.
    - Since this is a flat modifier we want to apply one of the Pathfinder bonus types to it to handle stacking rules.
4. Value: This is the value of our modifier.
    - If we wanted to make this a penalty we could by just adding `-` in front of the 2.
```json
{
    "key": "FlatModifier",
    "selector": "athletics",
    "type": "item",
    "value": -2
}
```

When you make an athletics roll you will see the modifier appear with the item name as the label. But we can manually override that label if we want by adding `"label": "Some Text"` as a new parameter. In the system you will often see labels with strings like `PF2E.SkillVariant.Climb`, these are localization strings which let translators more easily translate things. For your homebrew or modules you do not need to use these, and new ones can only be added by the system or by modules adding their own localization files. Anywhere you see this you can simply replace the text with what you want it to be.

## Selectors

Most, but not all, rule elements have selector as a required field. Selectors are the first thing the system uses to filter rules out from a roll. Some rule elements, like FlatModifier, allow for multiple selectors. So you could have a single rule that applies to multiple statistics, say an effect gives a bonus to any of the skills associated with a magic tradition. You would have an array of selectors: arcana, nature, occultism, and religion.
```json
{
    "key": "FlatModifier",
    "selector": [
        "arcana",
        "nature",
        "occultism",
        "religion"
    ],
    "value": "circumstance",
    "value": 1
}
```

This lets us cut down duplicate rule elements, keeping things a bit simpler.

Selectors can restrict things to more than just a particular skill or save. We can use a selector like `shortsword-damage` to apply our rule to all damage rolls with shortswords. **Selectors must be in kebab case**, so make sure you don't have errant punctuation, spaces, or capital letters. JavaScript is a case sensitive language, so we need to appease it when we write JavaScript objects.

The best way to find what selector you want to use is to make a roll of the type you want the rule to apply to, then right click that roll in chat and choose "inspect roll".

![image](https://github.com/foundryvtt/pf2e/assets/80183198/dae75196-a1f7-4c64-8512-15ba2ee87d02)

This will give us the roll inspector window, which shows much of the context for the roll. We'll go into the other parts of this tool later, but if we look at the top of the window we see the list of all the selectors that would have been appropriate to use for that roll.

In this example of an arcana roll, we see there's a few to pick from, and which one you choose will depend on what you want the rule to do. If we want to apply it to all intelligence based skills we see the `int-based` and `int-skill-check` selectors. `int-based` would also apply to intelligence based statistics, which could include your spell attack and DC or class DC. So it's important to choose the selector that restricts it down to just the type of rolls you want.

Not all selectors can be easily found through this window, however. Things that don't involve rolls, like flat bonuses to maximum hit points, increases to your class DC, or the amount of healing you receive can be modified but without a roll to make, there's not a good way to sniff out what selector to use. Instead, we have a list of many of the other selectors that can be used that would not show up in the roll inspector (Click to expand the list):

<details id="Selector-List">
<summary>List of other selectors</summary>

fortitude-dc\
reflex-dc\
will-dc

initiative\
perception-dc\
class\
inline-dc

ac

hp\
hp-per-level\
damage-received\
healing-received

all-speeds\
land-speed\
burrow-speed\
climb-speed\
fly-speed\
swim-speed

{skill}-dc

{tradition}-spell-dc\
spell-dc
</details>

## Predicates

Selectors can only get us so far in narrowing down when a rule applies. If you have a bonus that applies to all saves vs fear for example you would use the `saving-throw` selector, but in order to restrict it to only apply to saves against items with the fear trait we add a predicate:
```json
{
    "key":"FlatModifier",
    "predicate": [
        "item:trait:fear"
    ],
    "selector":"saving-throw",
    "type":"status",
    "value":1
}
```

Predicates are an "array", which means they need to have `[]` around them. You can separate multiple items with `,` and every item in the array must be satisfied to enable the rule element.

For example, if you had a bonus that should kick in against fear effects from dragons you could use
```json
"predicate": [
    "item:trait:fear",
    "origin:trait:dragon"
]
```
to automate the bonus properly.

The things you predicate on are called "roll options". Think of each roll option as a statement about the state of the game or the actor at the time the roll was made or actor data was prepared. Predicates are tests of those statements to determine if the rule element should be active.

There are a lot of roll options to predicate off of, and we can even make new ones using the appropriately named RollOption rule element. The quickest way to see what kinds of roll options are  you can predicate on is to make an attack roll, save, or skill check then right click the roll in chat and choose "Inspect Roll", this will list all of the roll options. **Roll options are always kebab case**.

![image](https://github.com/foundryvtt/pf2e/assets/80183198/87af62d5-cd73-44f9-a391-442589bdfff2)

We can see a lot of roll options get packaged into every roll. These include things like the current proficiency of all skills, feats, class or ancestry features, attribute modifiers, as well as information about your target or the origin of the effect.

Many roll options are split into domains and subdomains, like `item:trait:fire`. This indicates that the item the effect is coming from has the fire trait. The top level domain `item` is referring to the Foundry definition of items, then all the traits of that item are put under their own roll option in the `trait` subdomain.

One set of notable roll options in this list are the `self`, `target`, and `origin` domains. These domains are specific to the actor making the roll, their target, or the origin of the effect. The system provides each actor with their own context for the roll, swapping the domains around to be correct for the context of that actor. Rule elements use the context of the creature they are on. Take an example where Valeros rolls demoralize against a goblin.

![image](https://github.com/foundryvtt/pf2e/assets/80183198/a149ab5a-9feb-4445-ad6c-e1298802d7f8)

Valeros sees `self:trait:human` since he has the human trait, and `target:trait:goblin` since his target has the goblin trait. But when the goblin's data is prepared for determining its will DC it sees `self:trait:goblin` and `origin:trait:human`, the system swapped the context around when it handed the roll options off to prepare the goblin's data. Any roll option with `self` will be handed off in the appropriate context to the other actor involved. You can't write a rule element that predicates on the target having a specific feat, because feat roll options are not in the `self` domain, but you can write a predicate that works on the target's level, because level is included in the `self` domain.

Now that we know what roll options are and how to find them, we can begin to use them to form any predicate we want. Let's take an example item that should apply a bonus to diplomacy against any creature with the elf, gnome, or fey traits. To accomplish this in a single rule we need to have a way to have the predicate return true if any of these conditions is true. We can do this with an `or` logical test object:
```json
"predicate": [
    {
        "or": [
            "target:trait:elf",
            "target:trait:gnome",
            "target:trait:fey"
        ]
    }
]
```

The `or` here returns true if any of the roll options in it are present in the roll data, and since it's the only object in the top level of the predicate the predicate would be true if the `or` returns true. `or` takes an array as its input, since it needs multiple items to act. This is not the only logic object we have access to. You can use any of the basic logical tests as part of a predicate: `or`, `nor` (not or), `and`, `nand` (not and), `not`, as well as comparisons `lt` (less than), `lte` (less than or equal to), `gt` (greater than), `gte` (greater than or equal to), and `eq` (equal to).

If you aren't used to working with formal or programming logic these may take a moment to understand, but the ones that will be the most useful are `or`, `and`, and the numeric comparisons (`gte`, `lte`, etc). Here are some more examples of how to use these.

Tests can be used alongside other roll options, for example a modifier that should apply to any unholy creature except undead would look like this
```json
"predicate": [
    "target:trait:unholy",
    {
        "not": "target:mode:undead"
    }
]
```

`not` only has a string as its value, not an array. Unlike the other tests this is not used to compare multiple things.

We can also nest the logical tests inside each other. A modifier that applies to small beasts or large humanoids would look like this
```json
"predicate": [
    {
        "or": [
            {
                "and": [
                    "target:trait:beast",
                    "target:size:small"
                ]
            },
            {
                "and": [
                    "target:trait:humanoid",
                    "target:size:large"
                ]
            }
        ]
    }
]
```

The `nor` and `nand` tests are flipped versions of `or` and `and`, if you are confused about what they would return ask if an `or` or `and` would return true, then flip that.

The numeric comparisons do something different. They cut off the last subdomain and compare it to some other provided value. Let's say you have a bonus that should become active when your deception reaches master proficiency. In the roll inspector we see `skill:deception:rank:1` in the roll options.

<img width="317" height="390" alt="image" src="https://github.com/user-attachments/assets/6b6c7bc2-afe5-4df4-a1b3-0deb00837fb1" />

The 1 means the character is trained in deception (0 is untrained, 4 is legendary). We want the rule to apply if the rank is 3 or 4. We could use the `or` test for this, but it's cleaner to use the `gte` test.
```json
"predicate": [
    {
        "gte": [
            "skill:deception:rank",
            3
        ]
    }
]
```

Here we only give the roll option up to the last subdomain as the first item in the array. The second item in the array is the number (note the lack of quotation marks) that we want to compare against. This would return true if the skill rank is master or higher.

We can also compare different roll options to each other directly with these tests. If we have a modifier that only applies when attacking a target below your level we would write that as
```json
"predicate": [
    {
        "lt": [
            "target:level",
            "self:level"
        ]
    }
]
```

We could have also written this as
```json
"predicate": [
    {
        "gt": [
            "self:level",
            "target:level"
        ]
    }
]
```

Since the two are logically equivalent.

## Actor and Item Data

Often when writing a rule element you will need to reference data that exists on an actor. We do not have a master list of actor data paths, since the actor data is a large object and the structure can change slightly on different actors.

The easiest way to find any actor data is to select a token on the map and open console with F12, then type `_token.actor`. If you don't have a token on the scene you can also get the actor data with `game.actors.getName("Actor Name")`

![image](https://github.com/foundryvtt/pf2e/assets/80183198/fe88f25e-217a-4262-89eb-eec3b1d666cc)

Most of the actor data you will want to use will be under `actor.system`, while most item data you want to reference will be `item.badge.value`, `item.level`, or under `item.flags`.

There are two ways we can reference data on an actor or item, with the `@` syntax and with the `{}` syntax. If the data we want to reference should resolve to a number, then we use `@`. If the data should resolve to a string then we use `{}`. Let's see some examples.

A common use of actor data references is actor speeds. If you look for your land speed you will find it under `@actor.system.attributes.speed` and you will see both `speed.value` and `speed.total`. The difference between these two is the `value` is the speed before any modifiers are applied, and the `total` is the speed after all modifiers. If an effect gave you a swim speed equal to your land speed you would use the BaseSpeed rule element
```json
{
    "key": "BaseSpeed",
    "selector": "swim",
    "value": "@actor.system.attributes.speed.total"
}
```

Because we want the speed to be a number, we need to use `@` syntax to do the reference, and we want the speed after all modifiers are applied to we use `total`. There is a pitfall to avoid here. Once `speed.total` is calculated, it can no longer be modified. If you look for these values in console you will see that `speed.total` initially looks like `(...)` rather than being a number. This is because that data is prepared, it's not part of the base data of the actor and so to tell Foundry to calculate it in console just click the ellipses and it will be replaced by the calculated number. You will also see a lot of things under `system` show up with these ellipses at the top level of the actor. These are called "getters", because they provide a shortcut to get some actor data. Not all data is available in a getter but you can reference them just like you can use the full path.

Actor data preparation happens in steps, and once a step is finished it's not revisited to avoid recursions. Land speed is calculated before other speeds, and that means you can set a swim or fly speed based on your land speed, but you can't alter your land speed using the total land speed. For example, an effect that cut your speed in half would not work as a single rule, since you can't modify the speed based on its fully prepared total. This is something to keep in mind when you are trying to use actor data, sometimes the data you want is not prepared yet by the time it is needed and this is a hard technical limitation.

Let's look at referencing a string. If we have a rule that adds damage to a strike based on a selection from a ChoiceSet rule element we can reference where that data is stored in the item flags.
```json
{
    "key": "FlatModifier",
    "selector": "strike-damage",
    "damageType": "{item|flags.system.rulesSelections.damage}",
    "value": 1
}
```

This reads into the item data to access the stored string. The `{item` tells Foundry you are looking on the item the rule is on, as opposed to `{actor` which would be the actor the item is on. After the right place to search is set the path on the item or actor follows a vertical pipe `|`, here we are looking in the item's flags. See the ChoiceSet section for how that flag is set.

We can also use actor or item references in predicates.
```json
"predicate": ["item:trait:{actor|system.details.ancestry.trait}"]
```

This would predicate only on items that share a trait with your ancestry. This could be used to give a bonus to orc weapons on orcs, goblin weapons on goblins, etc.

The same can be done in selectors, most commonly to restrict rules to specific weapons
```json
{
    "key": "FlatModifier",
    "selector": "{item|_id}-damage",
    "value": 1,
    "damageType": "fire",
    "predicate": [
        "target:trait:plant"
    ]
}
```

The system will automatically resolve the ID of the item for you, keeping this rule from acting on any other item, even other copies of the same item. This also works for strikes granted by BattleForm or Strike rule elements, modifying the granted strikes that came from the same effect. Adding this type of rule to consumable ammunition transfers the effect to the weapon it is loaded in, allowing for special arrows or rounds to automatically work.

### Item Origin and Other Datapaths

You may find that you need to reference things not on the item or actor in question, for example, if you wanted an effect that grants a creature a bonus to damage equal to the originating actor's charisma modifier. To reference this data you can use the `@item.origin` data that gets set when dragging items to actors. Items from the sidebar or compendium do not have origin data. Some examples of this are Marshal's Rallying Charge

```json
{
    "key": "TempHP",
    "value": "@item.origin.abilities.cha.mod"
}
```

Or Curse of Inevitable Rot

```json
{
    "key": "FlatModifier",
    "predicate": [
        {
            "or": [
                "action:treat-disease",
                "action:treat-poison",
                "action:treat-wounds"
            ]
        }
    ],
    "selector": "skill-check",
    "type": "status",
    "value": "ternary(gte(@item.origin.conditions.cursebound.value,4),-2,-1)"
}
```

Not all origin data is available, but a good deal of it is. This is not live updated, so if you apply an effect referring to `@item.origin.level` then increase the origin actor's level it will not update.

There are also paths to refer to the spell or weapon being affected by a rule: `@spell` and `@weapon` respecitvely. In Sorcerous Potency we use `@spell.level` to refer to the rank of the spell being cast (since `@item.level` would ambiguously refer to either the spell or the item the rule it on!). `@weapon` is usually used in context of weapon damage dice with `@weapon.system.damage.dice`, like with the Gravity Weapon spell effect.

A similar set of data is parent item data. This always refers to the item the rule is on, which avoids any ambiguity about what item you are referring to. This is all contained in `parent` domain roll options, rather than being referred to by an `@` call for data. These are used for more complex automation, typically with item tags.


## Advanced Value Fields

Sometimes you will have a modifier that scales with some actor or item data, but doesn't directly use the actor data. For example, if you have a feat that adds one dice at first level and increases to two dice at 11th level, or the scaling is half the level rounded down. To handle these situations we have several tools available to us.

First is the `ceil()` and `floor()` functions. These can be used directly in any resolvable field (most RE fields are resolvable now). For example
```json
{
    "key": "FlatModifier",
    "selector": "strike-damage",
    "damageType": "spirit",
    "value": "floor(@actor.level/2)"
}
```

`floor()` rounds anything in the parenthesis down, while `ceil()` rounds up. At level 1 this is `floor(1/2)` which rounds to `0`. If there is a minimum value we can include that, such as if the feat is half level (minimum 1). `"value": "max(1,floor(@actor.level/2))` here the `max()` function has multiple arguments and takes the highest of them. The floor resolves to 0 at level 1, so max takes 1. But at level 4 the floor finally resolves to 2 and max takes that over the 1.

You can also do math `"value": "max(1,@actor.level -2)"` would return level - 2 (minimum 1). Notice the space between `@actor.level` and `-2`, this is important because Foundry would interpret `@actor.level-2` as a full actor path instead of a path and a command to do math.

We can also do logical testing using `ternary()` operators. Ternaries look strange at first, they have the format `ternary(test, value if true, value if false)`. Here is a simple example of a ternary
```json
{
    "key": "FlatModifier",
    "selector": "ac",
    "value": "ternary(gte(@item.level, 3), 2, 1)"
}
```

The first input to the ternary is a logical test `gte(@item.level, 3)`. This is asking if the item level is 3 or higher. If it is the test returns true and the ternary itself will resolve to 2. If the item is level 2 or lower the test is false and the ternary returns 1. `gte`, `gt`, `lte`, `le`, and `eq` are all available as tests. You can also use `floor()` or `ceil()` to round numbers as needed.

We can also nest ternaries inside each other
```json
{
    "key": "FlatModifier",
    "selector": "ac",
    "value": "ternary(gte(@item.level, 7), 3, ternary(gte(@item.level, 3), 2, 1))"
}
```

Here we have used two nested ternary calls and `gte(@item.level, 7)` to test if the item's level is greater than or equal to 7, and if it is we get 3 as the result. If the item isn't level 7 or higher then another ternary is called, the same one we had in the last example. So now the bonus scales to 2 at level 3 and 3 at level 7.

Ternaries aren't the only way to do this kind of scaling, beginning in version 7.6 of the PF2e system we have several new tools to use, such as `match()`, `when()`, and `btwn()`.

```json
{
    "key": "Weakness",
    "type": "fire",
    "value": "match(when(lte(@actor.level, 6), 1), when(btwn(@actor.level, 7, 16), 2), when(gte(@actor.level, 17), 3))"
}
```

Since this is still a lot let's break it down a bit, first by formatting it for easier reading.

```js
match(
    when(lte(@actor.level, 6), 1),
    when(btwn(@actor.level, 7, 16), 2),
    when(gte(@actor.level, 17), 3)
)
```

`match(parameterOne, parameterTwo, ...)` takes in multiple parameters and returns the first non-null parameter. `when(condition, value)` returns the value when the condition is true, and null when the condition is false. `btwn(value, minimum, maximum)` returns true if the value is between the set minimum and maximum (this is inclusive, so in the above example it would return true on a 7 or 16). There are other, more compact ways to write this same thing, for example:

```js
match(
    when(lte(@actor.level, 6), 1),
    when(lte(@actor.level, 16), 2),
    3
)
```

Here we changed the second parameter to use the simpler `lte()`, since the overlap between the first and second parameter doesn't matter for the `match()` function. We have also replaced the third parameter with the value, since there's no need to perform a logical test for it. This means our example rule should properly be written as

```json
{
    "key": "Weakness",
    "type": "fire",
    "value": "match(when(lte(@actor.level, 6), 1), when(lte(@actor.level, 16), 2), 3)"
}
```



# Rule Elements

This section of the article covers the functions of most of the system's rule elements. They are listed in alphabetical order for reference purposes, but ActiveEffectLike is not a good rule to start reading if you are new to using rule elements. If you are learning rule elements for the first time I suggest looking at the following rules: FlatModifier, AdjustModifier, ChoiceSet, RollOption, and TokenImage.

## Active Effect Like

The ActiveEffectLike Rule Element (AE like for short) is incredibly powerful. It is more performant, flexible, and reliable than core Foundry active effects. The values it writes are done before many of the other steps of data preparation, making it safe to set data that other rule elements can read from, however this also means that you need to consider the order of data preparation for some modifications.

**AE Likes are not meant to apply modifiers to rolls, use FlatModifier for that!**

Every AE like has a mode, a value, and a path at minimum. The mode is how the AE like will modify the base data, and the specifics of how it works depends on what kind of data is at that path.

`add` mode adds to existing data. Similarly `subtract` and `remove` subtract or remove existing data.

`override` mode overrides existing data, `upgrade` and `downgrade` overrides the existing data if the number would be upgraded or downgraded by the change (1 would upgrade to 2, but 3 would stay the same).

`multiply` multiplies the data by the value.

Each mode has a default priority, which can be easily edited in the rule element UI. Things with an earlier priority act first, so if you have a change that needs to happen last move the priority higher.
```
multiply: 10
add: 20
subtract: 20
remove: 20
downgrade: 30
upgrade: 40
override: 50
```

The path is the path to the actor data to be modified. Item data cannot be modified with an AE like, for modifications to items use the ItemAlteration RE.

Some simple examples: The system does not inherently know how many feats from an archetype you have taken, but the Barbarian Resiliency feat increases your hit points based on the number of feats you've taken from the archetype. To work around this, each feat in the barbarian archetype has this rule element on it using the `add` mode to count the number of feats taken from the dedication.
```json
{
    "key": "ActiveEffectLike",
    "mode": "add",
    "path": "flags.system.barbarianArchetype.featCount",
    "value": 1
}
```

This data path did not exist before, the system creates it when the rule becomes active on an actor. If you are storing custom data like this put it under flags in actor data. The `system` namespace in flags should be safe to use for most homebrew, but you can also use `flags.world` or `flags.moduleName` if you are making a module and want to be absolutely sure there won't be any conflicts between your work and things the system may do in the future. Don't store arbitrary data like this outside of flags.

You may see references to `flags.pf2e` or `flags.sf2e`. These are usable, but `flags.system` will automatically resolve to whichever of the two systems you are using, so it is the preferred reference.

Once the data is written it is available on the actor for other rule elements (or even other modules) to us. The Barbarian Resiliency feat has this FlatModifier on it to add 3 HP for every taken feat.
```json
{
    "key": "FlatModifier",
    "selector": "hp",
    "value": "3 * @actor.flags.system.barbarianArchetype.featCount"
}
```

A common use of AE likes is increasing proficiency. If a feat makes you trained in religion you can upgrade the rank like so
```json
{
    "key": "ActiveEffectLike",
    "mode": "upgrade",
    "path": "system.skills.religion.rank",
    "value": 1
}
```

This rule element is able to use actor data, like this rule element from Acrobat Dedication

```json
{
    "key": "ActiveEffectLike",
    "mode": "upgrade",
    "path": "system.skills.acrobatics.rank",
    "value": "ternary(gte(@actor.level, 15), 4, ternary(gte(@actor.level, 7), 3, 2))"
}
```

Here we used a nested ternary expression to increase the proficiency automatically with character level. Care should be taken when using actor data with an AE like. Character level, item level, attribute modifiers, and other values not modified by rule elements or constructed as part of character data prep should be safe to use in these rules.  Referencing the rank of a skill or weapon proficiency will not be safe since these are modified by other rule elements and the order of data preparation may yield unreliable results. If you need to do this you can change the priority or even the phase of the RE. See the below section on phases and actor data preparation for more on that.

So far we have seen numeric operations from AE likes, but  they can also work with strings. For example, in actor data under `system.build.languages.granted` you will see an array of languages. You can use the `add` mode to append to this array, like in this example from the Robe of Stone which gives the actor the Petran language.
```json
{
    "key": "ActiveEffectLike",
    "mode": "add",
    "path": "system.build.languages.granted",
    "value": {
        "slug": "petran",
        "source": "{item|name}"
    }
}
```

The `remove` mode could also be used to remove a language from this list.

## Interacting with system functions ##

AE like REs can also be used to set or alter actor data used by the system for our more dynamic automation. One example of this is the automation present in feats such as such as Gang Up and Side by Side. It's not currently possible to have a RE predicate on allies being adjacent to or in melee range of a target in a generic way. The system is limited to using ally presence to add Flanking to an attack or damage roll. I.e., it can't add a circumstance bonus to the attack roll or an extra die of damage.

```json
{
  "key": "ActiveEffectLike",
  "mode": "add",
  "path": "system.attributes.flanking.canGangUp",
  "value": 1
}
```

This RE enables the actor to be considered flanking if any ally is adjacent to their target. The field `system.attributes.flanking.canGangUp` is read by the system when determining flanking. The value can either be an integer, which in this case is the minimum number of allies in melee range needed to gain Flanking, or the string "animal-companion", which provides flanking if any Animal Companion is adjacent to the target. Note that this follows the RAW: the former uses melee range while the latter uses adjacent, which is not the same if the potential flanker has Reach. In this case add does not numerically sum the values, as it usually does, but appends to a list of ways to gain ally-based Flanking. The detail that some of these abilities add Flanking, some make the target Flat-footed, and some do both, is not currently accounted for.

Other examples of this type of interaction is in the Deny Advantage feature, or on the Fetchling heritage

```json
{
  "key": "ActiveEffectLike",
  "mode": "override",
  "path": "system.attributes.flanking.flatFootable",
  "value": "@actor.level"
}
```

```json
{
  "key": "ActiveEffectLike",
  "mode": "override",
  "path": "flags.system.colorDarkvision",
  "value": true
}
```

We do not have full documentation of all the system functions that read actor data like this, and this type of automation is generally reserved for things that cannot be handled purely by rule elements.

## More Complex AE Likes ##

More complicated examples of AE like rule elements can be seen in deviant abilities. These abilities work by having "awakening" feats enhance already taken feats. So we need to know not only what feats have been taken but also which are still eligible to be awakened.
```json
{
  "key": "ActiveEffectLike",
  "mode": "override",
  "path": "flags.system.deviantAbilities.awakenedChoices",
  "priority": 10,
  "value": {
    "greater": [],
    "lesser": []
  }
}
```

This rule creates an object with two sub arrays, to work with the two levels of awakening feat. `flags.system.deviantAbilities.awakenedChoices.lesser` would return `[]` in the console now. These arrays are then appended to by rules.
```json
{
  "key": "ActiveEffectLike",
  "mode": "add",
  "path": "flags.system.deviantAbilities.awakenedChoices.lesser",
  "value": {
    "label": "PF2E.SpecificRule.DeviantAbilities.AwakenedPower.BoneSpikesReach",
    "predicate": [
      {
        "not": "awakening:bone-spikes:reach"
      }
    ],
    "value": "bone-spikes:reach"
  }
}
```

```json
{
  "key": "ActiveEffectLike",
  "mode": "add",
  "path": "flags.system.deviantAbilities.awakenedChoices.lesser",
  "value": {
    "label": "PF2E.SpecificRule.DeviantAbilities.AwakenedPower.BoneSpikesPoison",
    "predicate": [
      {
        "not": "awakening:bone-spikes:poison"
      }
    ],
    "value": "bone-spikes:poison"
  }
}
```

This adds an entry to the array under the actor flags so that `flags.system.deviantAbilities.awakenedChoices.lesser` returns `[{value1},{value2}]` where `{value}` are the value objects above. Then a ChoiceSet rule element can read this entire array to fill in its choices
```json
{
  "choices": "flags.system.deviantAbilities.awakenedChoices.lesser",
  "key": "ChoiceSet",
  "prompt": "PF2E.SpecificRule.DeviantAbilities.AwakenedPower.Prompt",
  "rollOption": "awakening"
}
```

This would then give two choices, the poison or the reach awakenings for the Bone Spikes feat. For more examples of using AE likes to store objects or arrays see the alchemist class' research fields, or automaton enhancements.

## Data preparation and "phase"

One snag you may encounter when working with ActiveEffectLike rule elements is that actor data preparation comes in separate phases. Actor data like ability score modifiers are determined early on in data prep, and so data from later steps cannot be used to modify them. For example, it would be impossible to make an effect that increases your strength score based on your current HP. The order of what data is prepared and when cannot be changed by rule elements. But when a rule element is applied *can* be changed by specifying the `phase` or `priority` of that rule element.  The phases of data prep are, in order, `applyAEs`, `beforeDerived`, `afterDerived`, and `beforeRoll`. By default an ActiveEffectLike applies during the `applyAEs` phase. But if the data you want to modify is not ready in that phase you can move the application of the AE like to a later phase. For example, if you are playing with the stamina variant and want a new feat to increase the maximum resolve you can find the data path to the maximum resolve on the actor and use a RE to add to that, but resolve is based on your key ability modifier which is not ready itself during the applyAEs phase. So this RE

```json
{
  "key": "ActiveEffectLike",
  "mode": "add",
  "path": "system.resources.resolve.max",
  "value": 1
}
```

will not work. It is saying to add 1 to a value that does not exist yet, and that 1 will be overridden by the calculation later.  Instead we can force the RE to apply at a later phase, specifically the `afterDerived` phase, as resolve is calculated just before this phase of RE application.

```json
{
  "key": "ActiveEffectLike",
  "mode": "add",
  "path": "system.resources.resolve.max",
  "value": 1,
  "phase": "afterDerived"
}
```

This is similar to the `priority` field explained above, however `priority` is the priority within the given phase. If you move the phase later then it may come too late to do other calculations. For example an AE like rule element adding 1 to strength but with the phase set as `beforeRoll` will come after resolve is calculated. So you would not see a corresponding increase to the resolve points from that. When implementing abilities that clash with data preparation order you may find it very difficult to implement them as written, and should consider falling back to a toggle using the RollOption rule element.

## Actor Traits

The ActorTraits rule element lets you add or remove actor traits.
```json
{
  "key": "ActorTraits",
  "add": ["humanoid", "human", "elf"]
}
```

```json
{
  "key": "ActorTraits",
  "remove": ["humanoid"]
}
```

## Adjust Degree of Success

The AdjustDegreeOfSuccess rule element changes the outcome of a roll from what the normal result would otherwise produce. This is useful for abilities like "when you roll a success on a fortitude save you get a critical success instead".

Adjustment can be `two-degrees-better`, `one-degree-better`, `one-degree-worse`, `two-degrees-worse`, `to-critical-failure`, `to-failure`, `to-success`, or `to-critical-success`.

Example (Juggernaut Feat):  
```json
{
    "key": "AdjustDegreeOfSuccess",
    "selector": "fortitude",
    "adjustment": {
        "success": "one-degree-better"
    }
}
```

Example (Risky Surgery Feat):  
```json
{
    "key": "AdjustDegreeOfSuccess",
    "selector": "medicine",
    "predicate": ["risky-surgery"],
    "adjustment": {
        "success": "one-degree-better"
    }
}
```

You can also increase more than one at a time, like this
```json
{
    "key": "AdjustDegreeOfSuccess",
    "selector": "fortitude",
    "adjustment": {
        "success": "one-degree-better",
        "failure": "two-degrees-better",
        "criticalFailure": "to-critical-success"
    }
}
```

The above example can be done in a more compact way, since it affects all outcomes.
```json
{
    "key": "AdjustDegreeOfSuccess",
    "selector": "fortitude",
    "adjustment": {
        "all": "to-critical-success"
    }
}
```

## Adjust Modifier

AdjustModifier allows you to take a modifier and change it before it applies. Take for example, a feat that added one damage to all strikes and doubled that bonus against unholy targets.
```json
{
    "key": "FlatModifier",
    "selector": "strike-damage",
    "value": 1,
    "slug": "some-modifier"
}
```

We could do a second flat modifier to add another 1, but it would show up twice in the damage roll dialog, and we would need to make it more complicated to avoid having two labels on rolls. Instead, AdjustModifier lets us just double it by specifying the slug to target.

```json
{
    "key": "AdjustModifier",
    "selector": "strike-damage",
    "slug": "some-modifier",
    "value": 2,
    "mode": "multiply"
}
```

This is also useful for feats that do things like breaking stacking rules like this rule from Mountain Stance that modifies the bonus from Bracers of Armor to allow for stacking of otherwise unstackable bonuses without the need to use a separate macro. We can also relabel the modifier as we modify it, in this case it will appear as `Mountain Stance w/ Bracers of Armor` when you look in the AC breakdown.

```json
{
    "key": "AdjustModifier",
    "mode": "add",
    "selector": "ac",
    "relabel": "Mountain Stance w/ Bracers of Armor",
    "slug": "bracers-of-armor",
    "value": 4
}
```

Note that this combines with the flat modifier on the Bracers of Armor

```json
{
    "key": "FlatModifier",
    "selector": "ac",
    "slug": "bracers-of-armor",
    "type": "item",
    "value": 1
}
```

The AdjustModifer searches for the bonus with the same selector with the same slug and adds 4. This supports `add`, `multiply`, and `override` so you could double a bonus, increase it by 50%, or replace it entirely with a static number as well by changing the mode. 

Modifiers can be suppressed, or referred to by type. Like in this rule that would suppress all circumstance modifiers on skill checks.
```json
{
    "key": "AdjustModifier",
    "selector": "skill-check",
    "predicate": [
        "bonus:type:circumstance"
    ],
    "suppress": true
}
```

We can also change the damage type of a modifier, such as turning the bonus damage from weapon specialization to fire damage.
```json
{
    "key": "AdjustModifier",
    "selector": "strike-damage",
    "slug": "weapon-specialization",
    "damageType": "fire"
}
```

## Adjust Strike

AdjustStrike allows you to modify the properties of an existing strike by adding a trait from the strike. You must provide a definition for the strikes to be modified. You can define the adjustment to hit specific weapon names/slugs, such as the example below which adds the trip trait to the wolf jaws strike when flanking. The definition is structured exactly like a predicate, and can contain all the same statements.
```json
{
    "definition": ["item:slug:wolf-jaws"],
    "key": "AdjustStrike",
    "mode": "add",
    "predicate": ["self:flanking"],
    "property": "weapon-traits",
    "value": "trip"
}
```

Any roll option can be used in the definition, so a rule element to add sweep to all finesse weapons can be done. Note that you want to provide the full roll option in the definition. `item:trait:finesse` instead of just `finesse`. Both `add` and `remove` are allowed modes for this rule element.

The difference between the definition and the predicate is the definition tells the system what set of strikes to apply the adjustment to, while the predicate tells Foundry when the rule should be active.

You can also add precious materials to strikes
```json
{
    "key": "AdjustStrike",
    "mode": "add",
    "property": "materials",
    "value": "cold-iron"
}
```

A common example of this is special materials on ammunition, which transfer their rules to the weapon they are loaded into, here we restrict the new material to the weapon the ammo is loaded in.
```json
{
    "key": "AdjustStrike",
    "mode": "add",
    "property": "materials",
    "value": "silver",
    "definition": ["item:id:{item|id}"]
}
```

Or alter the range of a strike
```json
{
    "key": "AdjustStrike",
    "mode": "upgrade",
    "property": "range-increment",
    "value": 60,
    "definition": ["item:slug:seedpod"]
}
```

It can also be used to add property runes
```json
{
    "key": "AdjustStrike",
    "mode": "add",
    "property": "property-runes",
    "value": "flaming",
    "definition": ["item:trait:unarmed"]
}
```

## Aura

This rule element lets you create an aura around an actor, which applies a specified effect to the relevant targets. The rule has a full UI as its form, so direct manipulation of the JSON is not generally needed but is still documented here.
```json
{
  "effects": [
    {
      "affects": "allies",
      "events": [
        "enter"
      ],
      "uuid": "Compendium.pf2e.feat-effects.Ru4BNABCZ0hUbX7S"
    }
  ],
  "key": "Aura",
  "radius": 10,
  "slug": "marshals-aura",
  "traits": [
    "emotion",
    "mental",
    "visual"
  ]
}
```

This example applies to all allies when they enter the aura, and is removed when the ally leaves the aura. 

You can change `allies` to `all` or `enemies` to change who's affected. Currently only `enter` is supported, but in the future you be able to use `turn-start` or `turn-end` to change when the effect will be applied. You can add `"removeOnExit": false` to make the effect persist after the target leaves the aura; if you skip this field, it will default to `true`. Currently there is no way to require a save before application.

Auras that directly grant conditions need an effect to contain the condition grant currently. This is an area the system will be improving on as GrantItem support is expanded.

You may also predicate who the aura is granted to, as well as determine if the originating actor is affected by the aura, such as this example from Commanding Aura that only affects allied drow

```json
{
  "effects": [
    {
      "affects": "allies",
      "events": [
        "enter"
      ],
      "includesSelf": false,
      "predicate": [
        "target:trait:drow"
      ],
      "uuid": "Compendium.pf2e.bestiary-effects.iDLu83vhWoNIE7xt"
    }
  ],
  "key": "Aura",
  "radius": 30,
  "slug": "commanding-aura",
  "traits": [
    "emotion",
    "mental"
  ]
}
```

Traits such as `"visual"` and `"auditory"` will affect what kind of walls the aura can pass through; a visual aura can pass through a wall that permits sight but forbids sound, while an auditory aura cannot. If an aura doesn't have a trait like visual or auditory, it can't pass through that kind of wall.

There are several options for controlling the appearance of an aura:
```json
{
    "key": "Aura",
    "radius": 10,
    "appearance": {
        "border": {
            "alpha": 0.5,
            "color": "#000000"
        },
        "highlight": {
            "alpha": 0.25,
            "color": "#cc28b4"
        },
        "texture": {
            "src": "path/to/some/image/or/video.webm",
            "alpha": 1,
            "scale": 1,
            "loop": true,
            "playbackRate": 1
        }
    }
}
```

Seen above are the defaults for a video file: `appearance.texture.loop` and `appearance.texture.playbackRate` are omitted if `appearance.texture.src` is an image file path. `appearance.highlight.color` dynamically defaults to the user's configured color (owning player's if applicable, otherwise the first active GM's). `appearance.texture.scale` is relative to the `radius`, with a value of 1 stretching or shrinking the asset to the aura's perimeter. `appearance.border` can be set to `null` to skip drawing it entirely. Like any rule element, if a default value is desired it can simply be omitted.

## Base Speed

In the system if you have a +5 bonus to all your speeds, that does not give you a swim, fly, or climb speed if you didn't already have them. To add a new base speed, or override an existing base speed value a BaseSpeed rule can be used.
```json
{
    "key": "BaseSpeed",
    "selector": "fly",
    "value": 30
}
```

The selector is simply the speed type: `land`, `fly`, `climb`, `swim`, or `burrow`. A higher base speed will override a lower one, so a feat that granted a 10 foot swim speed would be overridden by one that added a 20 foot swim speed.

## Battle Form

This is the most complex single rule element in the system, complex enough that it [has its own page](https://github.com/foundryvtt/pf2e/wiki/Quickstart-guide-for-the-BattleForm-rule-element)


## Choice Set

The Choice Set Rule Element is a highly flexible Rule Element, prompting a character to make a choice from an array of options. This choice is then stored as a flag on the item, which can be referenced in other Rule Elements on the same item, using the data path `flags.system.rulesSelections.flagName`.
```json
{
    "key": "ChoiceSet",
    "choices": [{
        "label": "Fire",
        "value": "fire"
    }, {
        "label": "Water",
        "value": "water"
    }, {
        "label": "Earth",
        "value": "earth"
    }, {
        "label": "Air",
        "value": "air"
    }],
    "flag": "element"
}
```

Another use is for the choices to be the uuids of items. In combination with GrantItem, this lets you select a feat then grant it.
 ```json
{
    "adjustName": false,
    "choices": [{
        "value": "Compendium.pf2e.feats-srd.Item.ZBhvJ9O8MvBFAlhq"
    }, {
        "value": "Compendium.pf2e.feats-srd.Item.P04Hw8E6WAWARKHP"
    }],
    "key": "ChoiceSet",
    "prompt": "Select a feat",
    "flag": "feat"
}
```

```json
{
    "key": "GrantItem",
    "uuid": "{item|flags.system.rulesSelections.feat}"
}
```

`"prompt"` creates a heading on the prompt, to tell the character what choice they are making.
`"adjustName"` determines whether to edit the name of the original item to include the selection made.
The choices are defined as an array. A label isn't needed if a uuid is supplied, as it will default to the item's name.
`"flag"` manually sets the flag's name for the rulesSelections data path. This is useful if you do not want it to use the item's name.

This rule element cannot be used on consumable items.

You can also use a `"rollOption"` field, to store the choice automatically as a roll option. This adds a prefix to the Choice, so `"rollOption": "prefix"` would create a roll option of `"prefix:choice"` on the actor. Other rules can then predicate on this roll option as normal.

The `actorFlag` parameter automatically sets the choice on the actor flags. For example the Magiphage ancestry feature uses this to store a flag with the magical tradition and skill from a choice under `actor.flags.system.magiphage`. Once this choice is made you can use `{actor|flags.system.magiphage.skill}` to reference the skill, even on other items. This is not necessary to use when the reference is on the same item as the choice, since `{item|flags.system.magiphage.skill}` would resolve without this parameter being set to true.

```json
{
    "actorFlag": true,
    "flag": "magiphage",
    "choices": [
        {
            "label": "PF2E.TraitArcane",
            "value": {
                "skill": "arcana",
                "tradition": "arcane"
            }
        },
        {
            "label": "PF2E.TraitDivine",
            "value": {
                "skill": "religion",
                "tradition": "divine"
            }
        },
        {
            "label": "PF2E.TraitOccult",
            "value": {
                "skill": "occultism",
                "tradition": "occult"
            }
        },
        {
            "label": "PF2E.TraitPrimal",
            "value": {
                "skill": "nature",
                "tradition": "primal"
            }
        }
    ],
    "key": "ChoiceSet",
    "prompt": "PF2E.SpecificRule.Prompt.Tradition"
}
```

Canny Acumen uses a Choice Set to pick either one of the three saves, or Perception, and then an ActiveEffectLike to upgrade our proficiency in that choice.
```json
{
    "key":"ChoiceSet",
    "choices":[{
        "label":"Fortitude",
        "value":"system.saves.fortitude.rank"
    },{
        "label":"Reflex",
        "value":"system.saves.reflex.rank"
    },{
        "label":"Will",
        "value":"system.saves.will.rank"
    },{
        "label":"Perception",
        "value":"system.attributes.perception.rank"
    }]
}
```

```json
{
  "key": "ActiveEffectLike",
  "mode": "upgrade",
  "path": "{item|flags.system.rulesSelections.cannyAcumen}",
  "value": "ternary(gte(@actor.level,17),3,2)"
}
```

Because the path is different the whole path is set as the value. But if you are making selections between things that have a common path you can just store the small section of the path that is different. This pair of rule elements would let you choose a skill then train you in that skill.
```json
{
    "choices": [{
        "label": "Arcana",
        "value": "arcana"
    }, {
        "label": "Crafting",
        "value": "crafting"
    }, {
        "label": "Nature",
        "value": "nature"
    }, {
        "label": "Religion",
        "value": "religion"
    }, {
        "label": "Occultism",
        "value": "occultism"
    }, {
        "label": "Society",
        "value": "society"
    }],
    "key": "ChoiceSet",
    "prompt": "Select a skill",
    "flag": "skill"
}
```

```json
{
    "key": "ActiveEffectLike",
    "mode": "upgrade",
    "path": "system.skills.{item|flags.system.rulesSelections.skill}.rank",
    "value": 1
}
```

Here we resolved the flag in the middle of a larger string for the path.

Choice values can also be objects. This example has you pick a terrain and gain a resistance and weakness based on that choice.
```json
{
    "choices": [{
        "label": "Forest",
        "value": {
            "resistance": "poison",
            "weakness": "fire"
        }
    }, {
        "label": "Ocean",
        "value": {
            "resistance": "fire",
            "weakness": "electricity"
        }
    }, {
        "Desert"
        "value": {
            "resistance": "cold",
            "weakness": "fire"
        }
    }],
    "key": "ChoiceSet",
    "prompt": "Select a terrain",
    "flag": "terrain"
}
```

This is paired with two other rules to add the weakness and resistance, the value for each are stored under the flag name.
```json
{
    "key": "weakness",
    "type": "{item|flags.system.rulesSelections.terrain.weakness}",
    "value": 5
}
```

```json
{
    "key": "resistance",
    "type": "{item|flags.system.rulesSelections.terrain.resistance}",
    "value": 5
}
```

If you need access to the choice value on another item you will need to either use the `rollOption` parameter, the `actorFlag` parameter, or use an ActiveEffectLike rule element to write the choice to actor data manually.

If a choice should be locked behind some condition we can add a predicate to the choices. For example this is from Battlezoo Ancestries Year of Monsters, on a feat that can be taken twice, each time picking a different unarmed strike
```json
{
    "choices": [{
        "label": "Claw",
        "value": "claw",
        "predicate": [{
            "not": "demonic-strikes:claw"
        }]
    }, {
        "label": "Jaws",
        "value": "jaws",
        "predicate": [{
            "not": "demonic-strikes:jaws"
        }]
    }],
    "key": "ChoiceSet",
    "prompt": "Select an unarmed strike",
    "rollOption": "demonic-strikes"
}
```

The first time the feat is taken the user sees both choices, and it sets a roll option for that strike. The second time it is taken the player only has one choice and the system skips the dialog, choosing the remaining option automatically.

Choice sets can also filter items from compendium for you, for example if you want to give the user a choice of any level 1 ancestry feat that matches their ancestry.
```json
{
    "adjustName": false,
    "choices": {
        "filter": [
            "item:level:1",
            "item:category:ancestry",
            "item:trait:{actor|system.details.ancestry.trait}"
        ],
        "itemType": "feat"
    },
    "key": "ChoiceSet",
    "prompt": "Select an ancestry feat"
}
```

The filter object completely replaces the array that normally is used for the choices, the array for `"filter": []` works just like a predicate does. The system will automatically remove items the actor already has from the filtered choices, so you won't see duplicate feats in the list, unless that feat is marked as being able to be taken multiple times.

When using filters it can also be useful to return the slug of the selected item, instead of the UUID. This can be done using `slugsAsValues` inside the choices object.

```json
{
    "adjustName": false,
    "choices": {
        "filter": [
            "item:ranged",
            {
                "nor": [
                    "item:magical",
                    "item:trait:consumable",
                    "item:base:composite-shortbow",
                    "item:base:composite-longbow"
                ]
            }
        ],
        "itemType": "weapon",
        "slugsAsValues": true
    },
    "key": "ChoiceSet",
    "flag": "weapon",
    "prompt": "PF2E.SpecificRule.Prompt.RangedWeapon"
}
```

ChoiceSet can also access the system's configuration lists. This rule will automatically build a list of all the skills in the game, including skills registered by modules (Such as the computers skill added in Starfinder 2e).
```json
{
  "choices": {
    "config": "skills",
    "predicate": [
      "skill:{choice|value}:rank:0"
    ]
  },
  "flag": "skill",
  "key": "ChoiceSet",
  "prompt": "PF2E.SpecificRule.Prompt.Skill"
}
```

The predicate here means that the system will return the list of skills the actor is untrained in. Note that this predicate is inside the choices object, so it applies to the choices themselves, not whether the choice set will activate.
```json
{
    "choices": {
        "config": "skills",
        "predicate": [{
            "lte": [
                "skill:{choice|value}:rank",
                2
            ]
        }]
    },
    "flag": "skill",
    "key": "ChoiceSet",
    "prompt": "PF2E.SpecificRule.Prompt.Skill",
    "predicate": [{
        "lte": [
            "self:level",
            14
        ]
    }]
}
```

The above would activate if the character is level 14 or above, and list out all the skills that are expert or lower proficiency.

Choices have several special arguments as well, which let you select from the owned items of an actor. You can allow the choice to choose any strike the actor can make (weapon or unarmed) like this

```json
{
  "choices": {
    "attacks": true
  },
  "flag": "strike",
  "key": "ChoiceSet",
  "prompt": "Select a strike"
}
```

You can combine this with a predicate as well. If you have an effect that poisons a piercing or slashing weapon or unarmed attack you could use this

```json
{
  "choices": {
    "attacks": true,
    "predicate": [
      {
        "or": [
          "item:damage:type:slashing",
          "item:damage:type:piercing"
        ]
      }
    ]
  },
  "flag": "strike",
  "key": "ChoiceSet",
  "prompt": "Select a strike"
}
```

You may want to instead use just actual weapons, potentially including handwraps of mighty blows, like this rule from Blessed Armament

```json
{
  "choices": {
    "includeHandwraps": true,
    "ownedItems": true,
    "types": [
      "weapon"
    ]
  },
  "flag": "weapon",
  "key": "ChoiceSet",
  "prompt": "PF2E.SpecificRule.Prompt.Weapon"
}
```

The `ownedItems` param here tells it to select from their owned items, and you can set the type of item in the types array. So this could be used to select an armor, equipment, a spell, etc. This example from Oil of Potency lets you pick an armor or weapon for example

```json
{
  "choices": {
    "ownedItems": true,
    "types": [
      "armor",
      "weapon"
    ]
  },
  "flag": "item",
  "key": "ChoiceSet",
  "prompt": "PF2E.SpecificRule.Prompt.Item"
}
```

Finally, you may have two choice sets that should pick from the same manually constructed list, such as picking 2 of three options. While you can ask the inverse in that case (choose the one you do not want), it is far more intuitive and extendable to build the list and use predicates to cut down choices on the second ChoiceSet, like so.

```json
{
    "key": "ChoiceSet",
    "choices": [{
        "label": "A",
        "value": "a"
    }, {
        "label": "B",
        "value": "b"
    }, {
        "label": "C",
        "value": "c"
    }],
    "prompt": "Select a letter",
    "flag": "choiceOne",
    "rollOption": "my-first-choice"
}
```

```json
{
    "key": "ChoiceSet",
    "choices": [{
        "label": "A",
        "value": "a",
        "predicate": [{
            "not": "my-first-choice:a"
        }]
    }, {
        "label": "B",
        "value": "b",
        "predicate": [{
            "not": "my-first-choice:b"
        }]
    }, {
        "label": "C",
        "value": "c",
        "predicate": [{
            "not": "my-first-choice:c"
        }]
    }],
    "prompt": "Select a second letter",
    "flag": "choiceTwo"
}
```

## Crafting Ability

The CraftingAbility rule element allows you to define a new crafting entry. Take this from Advanced Alchemy:

```json
{
  "craftableItems": [
    "item:trait:alchemical",
    "item:trait:consumable"
  ],
  "isDailyPrep": true,
  "key": "CraftingAbility",
  "label": "Advanced Alchemy",
  "maxSlots": "4 + @actor.system.abilities.int.mod",
  "slug": "advanced-alchemy"
}
```

We start with the `craftableItems` array, which defines the crafting entry as able to make alchemical consumables. This works like a filter or definition in other rule elements, accepting predicate-like entries. For example, we can set the craftableItems entry with an `or`, like in the Exemplar's Horn of Plenty ikon.


```json
{
  "craftableItems": [
    {
      "or": [
        "item:trait:elixir",
        "item:trait:potion"
      ]
    }
  ],
  "isDailyPrep": true,
  "key": "CraftingAbility",
  "maxSlots": "ternary(gte(@actor.level,16),3,ternary(gte(@actor.level,8),2,1))",
  "slug": "horn-of-plenty"
}
```

This would let you craft elixirs or potions. Just like with a predicate, the top level is an implied `and`.

The next line, `isDailyPrep` is telling the rule that it is part of your daily preparations. Currently, in order for the rule to function you must declare one of `isDailyPrep`, `isPrepared`, or `isAlchemical` to be true.

The `maxSlots` property gives us the number of remaining slots for the crafting entry. As seen in the two examples above you can use ternaries or calls to actor data to set the number of slots.

We can also declare a maximum level with `maxItemLevel`. By default this uses the actor level, so in both examples above the max level is omitted which means it will use the actor level instead.

We can also set the batch size, which requires defining what items can be made in what batch sizes.

```json
{
  "craftableItems": [
    {
      "or": [
        "item:trait:talisman",
        "item:trait:alchemical"
      ]
    }
  ],
  "batchSizes": {
    "default": 1,
    "other": [
      {
        "definition": [
          "item:trait:bomb"
        ],
        "quantity": 10
      },
      {
        "definition": [
          "item:trait:talisman"
        ],
        "quantity": 2
      },
      {
        "definition": [
          "item:trait:alchemical"
        ],
        "quantity": 5
      }
    ]
  },
  "isDailyPrep": true,
  "key": "CraftingAbility",
  "label": "Talismans and Alchemy (Especially Bombs!)",
  "maxSlots": 50,
  "slug": "talisman-go-boom"
}
```

This rule creates an entry where the batch size of regular alchemical items is 5, talismans get 2, and bombs get 10.

![image](https://github.com/user-attachments/assets/05c4c2fd-b80b-4a85-891a-07968ec1ee9d)

## Creature Size

The CreatureSize rule element lets you change the size of an actor.
```json
{
  "key": "CreatureSize",
  "reach": {
    "override": 10
  },
  "resizeEquipment": true,
  "value": "large"
}
```

The only fields that are necessary is the key and value, but `resizeEquipment` lets you also change the equipment size at the same time, and the override for reach lets you give the actor reach for flanking calculations. Remember than in PF2e reach and size are not inherently linked.

You can also give an increment as the value, for example, to reduce a creature by 1 size category (large -> medium, etc) use a value of `-1`

```json
{
  "key": "CreatureSize",
  "value": -1
}
```

You can go up multiple increments this way as well, such as increasing two categories by giving a value of `2`. The system will not reduce a creature below tiny or increase it above gargantuan.

## Critical Specialization

You can define critical specialization conditions for abilities using the CriticalSpecialization rule element. Set a predicate for the conditions they get critical specialization. Barbarian Brutality for example:
```json
{
    "key": "CriticalSpecialization",
    "predicate": ["self:effect:rage", "item:melee"]
}
```

By default these pull the critical specialization from the weapon group of the weapon, but these can be override. Spike Launcher, for example, is a firearm but uses the bow critical specialization. So this same rule element is used to tell the system that if you meet the requirements for critical specialization the text should be that of the bow.
```json
{
    "alternate": true,
    "key": "CriticalSpecialization",
    "predicate": ["item:id:{item|_id}"],
    "text": "PF2E.Item.Weapon.CriticalSpecialization.bow"
}
```

This is just a call to the localization path for the bow weapon specialization. You can type any text in this to define your own.
```json
{
    "alternate": true,
    "key": "CriticalSpecialization",
    "predicate": ["item:id:{item|_id}"],
    "text": "You knock the target back 15 feet"
}
```

You can also create damage dice and modifiers instead of text in the critical specialization RE, like this, which would emulate the knife group crit spec.
```json
{
  "alternate": true,
  "key": "CriticalSpecialization",
  "predicate": [
    "item:trait:unarmed"
  ],
  "damageDice": {
    "number": 1,
    "damageType": "bleed",
    "faces": 6
  },
  "modifier": {
    "damageType": "bleed",
    "value": "@weapon.system.runes.potency"
  }
}
```

## Damage Alteration

The `DamageAlteration` rule element is a new rule element that lets you alter damage modifiers and dice. This is different than DamageDice, which is typically used to add additional dice. The `override` property of DamageDice will eventually be migrated to DamageAlteration rule elements.

You can target dice by slug and alter different properties of them. For example, the Insight Coffee effect uses this rule to adjust the die size of an Investigator's Strategic Strike to d8

```json
{
    "key": "DamageAlteration",
    "mode": "upgrade",
    "predicate": [
        "dice:slug:strategic-strike"
    ],
    "property": "dice-faces",
    "selectors": [
        "strike-damage"
    ],
    "value": 8
}
```

We can also adjust the damage type, like this rule which changes the damage type of the deadly d8 trait's damage

```json
{
    "key": "DamageAlteration",
    "mode": "override",
    "predicate": [
        "dice:slug:deadly-d8"
    ],
    "property": "damage-type",
    "selectors": [
        "strike-damage"
    ],
    "value": "fire"
}
```

You can target things other than strike damage, for example we can override the inline damage of an ability. This rule changes the inline damage of an action called "Breath Weapon" to be acid damage when the creature is frightened.

```json
{
    "key": "DamageAlteration",
    "mode": "override",
    "predicate": [
        "item:slug:breath-weapon",
        "self:condition:frightened"
    ],
    "property": "damage-type",
    "selectors": [
        "inline-damage"
    ],
    "value": "acid"
}
```

Another example of this is from the Psychic's Oscillating Wave, which changes the damage type of mindshift abilities to their currently selected conservation of energy damage type.

```json
{
    "key": "DamageAlteration",
    "mode": "override",
    "predicate": [
        "item:tag:mindshifted",
        "mindshift:add-remove-energy"
    ],
    "property": "damage-type",
    "selectors": [
        "inline-damage"
    ],
    "value": "{item|flags.system.rulesSelections.conservationOfEnergy}"
}
```

Since `selectors` is an array you can put in more than just one selector. This complicated alteration on the NPC Grabble Forden, for example. Here we've included `inline-damage` as a selector because some of Grabble's spells include inline rolls on them and those should additionally be covered by his 

```json
{
    "key": "DamageAlteration",
    "mode": "override",
    "predicate": [
        "poison-conversion",
        "item:type:spell",
        {
            "or": [
                "damage:type:acid",
                "damage:type:cold",
                "damage:type:electricity",
                "damage:type:fire"
            ]
        },
        {
            "or": [
                "item:trait:cantrip",
                {
                    "lte": [
                        "item:rank",
                        6
                    ]
                }
            ]
        }
    ],
    "property": "damage-type",
    "selectors": [
        "spell-damage",
        "inline-damage"
    ],
    "value": "poison"
}
```

The above all use the override mode, but the other RE modes are available. For example, we can use the add mode to increase the damage dice of spells with the fire trait

```json
{
    "selectors": ["spell-damage"],
    "key": "DamageAlteration",
    "mode": "add",
    "property": "dice-number",
    "value": 1,
    "predicate": [
        "item:trait:fire"
    ]
}
```

This differs from DamageDice by adjusting the base dice of the effect, meaning that in the roll breakdown you wouldn't see this alteration as a separate listing.

We can also relabel damage, this is similar to the relabel function of AdjustModifier.

```json
{
    "key": "DamageAlteration",
    "mode": "override",
    "predicate": [
        "dice:slug:deadly-d8"
    ],
    "property": "damage-type",
    "selectors": [
        "strike-damage"
    ],
    "value": "fire",
    "relabel": "Deadly Fire!"
}
```

We can also modify the base damage of a weapon. Here we increase the base damage dice of a shortsword.

```json
{
  "key": "DamageAlteration",
  "mode": "upgrade",
  "predicate": [
    "dice:slug:base"
  ],
  "property": "dice-number",
  "selectors": [
    "shortsword-damage"
  ],
  "value": 2
}
```

This differs from simply adding a dice using a DamageDice rule because it increases the weapon's actual damage dice, similar to how a striking rune works. This will increase damage dealt by traits like deadly or fatal, or the damage from spells like gravity weapon. Unlike a striking rune this will not add the magical trait to the weapon. The system uses this method to increase the damage of versatile vials, as an example.

## Damage Dice

A Damage Dice Rule element adds additional dice to damage or critical damage rolls. This example adds 3d6 piercing damage to a critical strike with an improvised weapon.

```json
{
    "critical": true,
    "key": "DamageDice",
    "selector": "strike-damage",
    "diceNumber": 3,
    "dieSize": "d6",
    "damageType": "piercing",
    "predicate": [
        "item:tag:improvised"
    ],
    "label": "Shattering Strike"
}
```

`"critical": true` makes the dice only add on a critical hit, or add dice that apply to all hits that don't double on a critical. Rules wise anything added only on a critical is not doubled, everything else is doubled. Setting `"critical": false` makes the dice not be mutiplied on a critical hit.

`dieSize` and `damageType` can be omitted to add additional damage dice of the type and size that the weapon already has to the damage. You can also set the damage category to be `persistent`, `precision`, or `splash`. Note that DamageDice uses `category` rather than `damageCategory` like FlatModifier uses.
```json
{
    "key": "DamageDice",
    "selector": "strike-damage",
    "diceNumber": 1,
    "dieSize": "d4",
    "damageType": "fire",
    "category": "persistent"
}
```

The diceNumber or dieSize properties can be set in a ternary, like this example from Vicious Swing (formerly Power Attack)
```json
{
  "diceNumber": "ternary(gte(@actor.level, 18), 3,ternary(gte(@actor.level, 10), 2, 1))",
  "key": "DamageDice",
  "predicate": [
    "vicious-swing"
  ],
  "selector": "melee-strike-damage"
}
```

A common use of DamageDice is implementing the *bane* rune, like this for *dragon bane*:

```json
{
    "key": "DamageDice",
    "selector": "{item|id}-damage",
    "diceNumber": 1,
    "dieSize": "d6",
    "predicate": ["target:trait:dragon"],
    "label": "Dragon Bane"
}
```

Or this for *plant/fungi bane*:

```json
{
    "key": "DamageDice",
    "selector": "{item|id}-damage",
    "diceNumber": 1,
    "dieSize": "d6",
    "predicate": [
        {
            "or": [
                "target:trait:plant",
                "target:trait:fungus"
            ]
        }
    ],
    "label": "Plant/Fungi Bane"
}
```


**The following is here for documentation purposes, you should use use DamageAlteration for the following changes** These examples will be removed and the system will eventually discontinue DamageDice orverrides.

The dieSize or damageType of the base strike can also be overridden like so.
```json
{
    "key": "DamageDice",
    "override": {
        "damageType": "force",
        "dieSize": "d10"
    },
    "selector": "shortbow-damage"
}
```

The dieSize can also be upgraded by 1 step, rather than specifying the specific size to change to. This example upgrades strike damage dice by one step unless they are already d8 or higher.
```json
{
    "key": "DamageDice",
    "override": {
        "upgrade": true
    },
    "predicate": [
        {
            "lte": [
                "item:damage:die:faces",
                6
            ]
        }
    ],
    "selector": "strike-damage"
}
```

You can also use DamageDice to alter spell damage, such as this example from Life Oracle, modifying Heal spells to use a d12.
```json
{
    "key": "DamageDice",
    "override": {
        "dieSize": "d12"
    },
    "predicate": [
        "item:slug:heal",
        {
            "or": [
                "oracular-curse:stage:moderate",
                "oracular-curse:stage:major",
                "oracular-curse:stage:extreme"
            ]
        },
        "all-living-targets"
    ],
    "selector": "spell-damage"
}
```

## Dexterity Modifier Cap

Some items impose a cap on the dexterity modifier. Armor has this included in the details tab, so it's not necessary to use a rule element for that. But for other effects we can use the Dexterity Modifier Cap rule element.
```json
{
    "key": "DexterityModifierCap",
    "value": 5
}
```

## Ephemeral Effect

The EphemeralEffect rule element allows you to place temporary effects that apply only during some calculation. The use of this can be seen in the system on the Surprise Attack feature, which treats your target as off-guard if you act before them on the first round of initiative when you rolled deception or stealth for your initiative
```json
{
    "key": "EphemeralEffect",
    "predicate": [
        "encounter:round:1",
        {
            "lt": [
                "self:participant:initiative:rank",
                "target:participant:initiative:rank"
            ]
        },
        {
            "or": [
                "self:participant:initiative:stat:deception",
                "self:participant:initiative:stat:stealth"
            ]
        }
    ],
    "selectors": [
        "strike-attack-roll",
        "spell-attack-roll",
        "strike-damage",
        "attack-spell-damage"
    ],
    "uuid": "Compendium.pf2e.conditionitems.AJh5ex99aV6VTggg"
}
```

Ephemeral effects only exist during the data preparation cycle, so you will see their results in rolls but you won't see the targeted effect on the actor. The above rule affects the target of your roll, but it is also possible to send an EphemeralEffect to the origin of a roll. For example, this would make an attacker enfeebled 1 for the incoming strike.
```json
{
  "key": "EphemeralEffect",
  "affects": "origin",
  "selectors": [
    "strike-attack-roll"
  ],
  "uuid": "Compendium.pf2e.conditionitems.Item.MIRkyAjyBeXivMa7"
}
```

We can also apply an alteration to the effect
```json
{
    "key": "EphemeralEffect",
    "affects": "origin",
    "selectors": [
        "strike-attack-roll"
    ],
    "uuid": "Compendium.pf2e.conditionitems.Item.MIRkyAjyBeXivMa7",
    "alterations": [{
        "mode": "override",
        "property": "badge-value",
        "value": 2
    }]
}
```

You can also have an effect applied when the target takes damage, such as this from Holy Castigation
```json
{
    "key": "EphemeralEffect",
    "predicate": [
        "item:type:spell",
        "item:slug:heal",
        "target:trait:fiend"
    ],
    "selectors": [
        "damage-received"
    ],
    "uuid": "Compendium.pf2e.feat-effects.Item.142QRyK2aPIrJu48"
}
```

## Fast Healing and Regeneration

The FastHealing rule element can provide a reminder that you have fast healing active on the character. This rule element has a UI, which covers all the properties of the rule.

This example would give the actor fast healing equal to twice the item level for example, like the Life Boost spell.
```json
{
    "key": "FastHealing",
    "value": "@item.level * 2"
}
```

Regeneration is given by the same rule element, and can have `deactivatedBy` added as an array
```json
{
    "key": "FastHealing",
    "value": 2,
    "type": "regeneration",
    "deactivatedBy": [
        "acid",
        "fire"
    ]
}
```

## Flat Modifier

FlatModifier allows us to add flat values to checks, damage rolls, or statistics. It has a UI which covers most of the functions of the rule element, but some still require editing the full JSON object.

![image](https://user-images.githubusercontent.com/80183198/193498532-a732f5d6-e7c5-4cee-ab91-83ab173c97e5.png)

The damage selector applies to all damage rolls, including rolls from inline buttons or spells. If we want to restrict it down farther we can with the `strike-damage` selector, such as this example which adds 2 damage to any non-agile strike.
```json
{
    "key": "FlatModifier",
    "selector": "strike-damage",
    "predicate": [
        {"not": "item:trait:agile"}
    ],
    "value": 2
}
```

You can also store the selectors of a FlatModifier as an array, this has the benefit of letting you compact several rule elements into one, like this example from Courageous Anthem
```json
{
    "key": "FlatModifier",
    "selector": [
        "attack",
        "damage"
    ],
    "type": "status",
    "value": 1
}
```

Since both attack and damage get the same bonus we can combine them to the same RE. We can't include Courageous Anthem's bonus to saves though as that is predicated on fear effects, while the attack and damage bonuses are not. Predicates would apply to all selectors.

If you have multiple flat modifiers to the same selector from the same item then you need to set a `slug` property. This lets Foundry tell the difference between each of the bonuses. Say we have one item (Daredevil's Boots) with both of these modifiers on it.
```json
{
    "key": "FlatModifier",
    "selector": "acrobatics",
    "type": "item",
    "value": 1
}
```

```json
{
    "key": "FlatModifier",
    "selector": "acrobatics",
    "type": "circumstance",
    "value": 2,
    "predicate": ["action:tumble-through"]
}
```

The normal stacking rules would let these stack, and the predicate restricts it down so that acrobatics gets a total of +1 but the tumble through action specifically gets a total +3. However, when we test it we would find that only the first rule applies to our tumble through checks. The reason is because both of these modifiers are given a slug which should uniquely identify them. By default the system gives each one the slug of the item they are on: `daredevils-boots`. Since both are active on the same roll the system starts by throwing out duplicated slugs. We can avoid this by adding a unique slug to either of them.
```json
{
    "key": "FlatModifier",
    "selector": "acrobatics",
    "type": "circumstance",
    "value": 2,
    "predicate": ["action:tumble-through"],
    "slug": "daredevils-boots-tumble-through"
}
```

The actual slug we use is arbitrary, as long as it is unique. If you find yourself needing to use this though it will help to use a slug that is descriptive, as it can help make it easier to target with an AdjustModifier.

When adding damage we can specify the damage type
```json
{
    "key": "FlatModifier",
    "selector": "strike-damage",
    "damageType": "acid",
    "value": 3
}
```

Note however that `precision`, `persistent`, and `splash` are not damage types, they are a damage category. We can use `damageCategory` to set these.
```json
{
    "key": "FlatModifier",
    "selector": "strike-damage",
    "damageCategory": "precision",
    "value": 3
}
```

```json
{
    "key": "FlatModifier",
    "selector": "strike-damage",
    "damageCategory": "splash",
    "damageType": "fire",
    "value": 3
}
```

You can also interact with the badge value of an effect in a couple ways. First is directly referencing the value, for example this is the rule element on the Frightened condition
```json
{
  "key": "FlatModifier",
  "selector": "all",
  "slug": "frightened",
  "type": "status",
  "value": "-@item.badge.value"
}
```

FlatModifier also has a property to allow the effect to expire on use
```json
{
  "key": "FlatModifier",
  "removeAfterRoll": true,
  "selector": "attack",
  "type": "status",
  "value": 1
}
```

This would expire the effect after the next roll. These removals only work on checks, not on damage rolls for the time being. If the effect should only expire on a roll when the modifier is actually active (such as with Guidance) then instead of the Boolean we can use the string `"if-enabled"`
```json
{
  "key": "FlatModifier",
  "predicate": [
    "guidance"
  ],
  "removeAfterRoll": "if-enabled",
  "selector": [
    "attack",
    "perception",
    "saving-throw",
    "skill-check"
  ],
  "type": "status",
  "value": 1
}
```


## Grant Item

The Grant Item Rule Element allows an item to be granted to a character by another item. This occurs when the item is first added to the actor, and the Rule Element will not work if added to items already on a character. Grant item does not work at all on physical items.

The example here is the Shield Block feature for Fighter, to grant then the Shield Block general feat.
```json
{
    "key": "GrantItem",
    "uuid": "Compendium.pf2e.feats-srd.jM72TjJ965jocBV8"
}
```
You can also use predicates to grant an item only to certain characters. This works like other predicates, and is useful for abilities and options granted depending on the character. Here, the Perpetual Infusion class feature grants a specific item depending on subclass. 
```json
{
    "key": "GrantItem",
    "uuid": "Compendium.pf2e.feats-srd.7LB00jkh6JaJr3vS",
    "predicate": ["feature:bomber"]
}
```
```json
{
    "key": "GrantItem",
    "uuid": "Compendium.pf2e.feats-srd.fzvIe6FwwCuIdnjX",
    "predicate": ["feature:chirurgeon"]
}
```

However, due to how Grant Item works, if a character is updated to meet the predicate after the rule has been added, the item still will not be granted. You can add `"reevaluateOnUpdate":true` to cause the Rule Element to check if the character meets the prerequisites at any point, at which point the GrantItem will execute. This is useful for features that grant you additional abilities if you meet their prerequisites at any point, such as Intimidating Prowess.

You can also use Grant Item with Choice Set, to allow the character to choose an item from a list, and grant it to themselves. This can be seen on the Rule Elements for subclass selection, such as the Druid's Order.
```json
{
  "adjustName": false,
  "choices": {
    "filter": [
      "item:tag:druid-order"
    ]
  },
  "flag": "druidicOrder",
  "key": "ChoiceSet",
  "prompt": "PF2E.SpecificRule.Druid.DruidicOrder.Prompt"
}
```
```json
{
    "key":"GrantItem",
    "uuid":"{item|flags.system.rulesSelections.druidicOrder}"
}
```

If you are granting an item that has a ChoiceSet on it you can pre-select the choice to skip the dialog, for example if a feature granted you Assurance (Performance) you could use
```json
{
    "key": "GrantItem",
    "preselectChoices": {
        "assurance": "performance"
    },
    "uuid": "Compendium.pf2e.feats-srd.W6Gl9ePmItfDHji0"
}
```

Where the key `assurance` matches the flag of the ChoiceSet on the Assurance feat. If an item has multiple ChoiceSets you can specify all or some of the choices this way by adding to the `preselectChoices` object.

You can also grant conditions, and set the badge value during the grant, this for example grants the Stupefied 2 condition when either major or extreme are set as the oracle curse stage.

```json
{
    "alterations": [{
        "mode": "override",
        "property": "badge-value",
        "value": 2
    }],
    "key": "GrantItem",
    "onDeleteActions": {
        "grantee": "restrict"
    },
    "predicate": [{
        "or": [
            "oracular-curse:stage:major",
            "oracular-curse:stage:extreme"
        ]
    }],
    "uuid": "Compendium.pf2e.conditionitems.e1XGnhKNSQIm5IXg"
}
```

The `onDeleteActions` object controls the behavior on deletion of the `granter` and `grantee`. `grantee` allows you to set the behavior of the granted item, while `granter` lets you control the behavior of the granting item, in most cases `grantee` will be the property to modify. In this case `restrict` locks the condition in place as long as the effect granting the condition is active. Other options here are `cascade` which will delete both the granted condition and the parent effect if either are removed. `detach` is the default mode for the grantee, but `"granter": "detach"` will leave the condition in place if the parent effect is removed. The default case allows for the deletion of the granted effect without affecting the parent, but removing the parent removes the granted item.

We can also alter the damage of persistent damage, using a slightly more complex syntax than other alterations, like so:

```json
{
  "key": "GrantItem",
  "uuid": "Compendium.pf2e.conditionitems.Item.lDVqvLKA6eF3Df60",
  "alterations": [
    {
      "mode": "override",
      "property": "persistent-damage",
      "value": {
        "damageType": "acid",
        "formula": "2d4"
      }
    }
  ]
}
```

Alterations is an array, so multiple can be used in combination. Combining the above with a `pd-recovery-dc` alteration and an `onDeleteAction` gives us a rule that will grant persistent damage at a custom recovery DC and damage formula, which will remove the parent effect when the actor recovers, meaning you can automate an effect that lasts until the persistent damage is removed.

```json
{
  "key": "GrantItem",
  "uuid": "Compendium.pf2e.conditionitems.Item.lDVqvLKA6eF3Df60",
  "alterations": [
    {
      "mode": "override",
      "property": "persistent-damage",
      "value": {
        "damageType": "acid",
        "formula": "2d4"
      }
    },
    {
      "mode": "override",
      "property": "pd-recovery-dc",
      "value": 19
    }
  ],
  "onDeleteActions": {
    "grantee": "cascade"
  }
}
```

You can also grant a condition "in memory", this is useful when doing something that needs re-evaluated on each actor data preparation cycle. The most notable use of this is in the Oracle curse effects.

```json
{
    "allowDuplicate": false,
    "alterations": [{
        "mode": "override",
        "property": "badge-value",
        "value": 2
    }],
    "inMemoryOnly": true,
    "key": "GrantItem",
    "onDeleteActions": {
        "grantee": "restrict"
    },
    "predicate": [{
        "gte": [
            "parent:badge:value",
            3
        ]
    }],
    "reevaluateOnUpdate": true,
    "uuid": "Compendium.pf2e.conditionitems.Item.e1XGnhKNSQIm5IXg"
}

```

Here `inMemoryOnly` tells the system to grab the condition loaded in memory instead of adding it to the actor proper. This lets it grant it on a changing of the parent effect's badge value. However, these grants also cannot be further altered. So granting Frightened will lock that frightened condition onto the actor until the predicate is not true or until the granting effect is removed. So only grant things that should be immutable in this way.

## Lose Hit Points

The LoseHitPoints rule element is for effects that removes current HP from an actor. The difference between this and a negative FlatModifier to hp is that this reduces the current HP on application, effectively damaging the actor. This example deals damage equal to the actor's level when the effect is added.
```json
{
    "key": "LoseHitPoints",
    "value": "@actor.level"
}
```

However, the user could simply heal this damage like any other hit point loss. If you want to make the damage unrecoverable you can using `"recoverable": false"` such as this example from Quicksilver Mutagen.
```json
{
    "key": "LoseHitPoints",
    "recoverable": false,
    "value": "2*@actor.level"
}
```

## Item Alteration

The ItemAlteration rule element allows you to point to other items and alter specific properties of those items. For example these two rule elements from the Shield Ally feature.

```json
{
    "itemType": "shield",
    "key": "ItemAlteration",
    "mode": "add",
    "predicate": [
        "item:equipped"
    ],
    "property": "hardness",
    "value": 2
}
```
```json
{
    "itemType": "shield",
    "key": "ItemAlteration",
    "mode": "multiply",
    "predicate": [
        "item:equipped"
    ],
    "property": "hp-max",
    "value": 1.5
}
```

These point to an equipped shield and alter the max HP and hardness properties of that item. You can also modify the recovery DC of persistent damage, such as this from a Barbazu which makes persistent bleed damage require a DC 20 check to recover from.

```json
{
    "itemType": "condition",
    "key": "ItemAlteration",
    "mode": "upgrade",
    "predicate": [
        "item:damage:type:bleed"
    ],
    "property": "pd-recovery-dc",
    "value": 20
}
```

We can also add to or override the description of an item.

```json
{
  "itemType": "spell",
  "key": "ItemAlteration",
  "mode": "add",
  "predicate": [
    "item:tag:amped",
    {
      "not": "alternate-amp"
    },
    "item:slug:ghostly-shift"
  ],
  "property": "description",
  "value": [
    {
      "text": "PF2E.SpecificRule.Psychic.Amp.GhostlyShift.Base",
      "title": "PF2E.TraitAmp"
    },
    {
      "text": "PF2E.SpecificRule.Psychic.Amp.GhostlyShift.Heightened",
      "title": "PF2E.SpecificRule.Psychic.Amp.AmpHeightenedLabel.AmpHeightenedPlusTwo"
    }
  ]
}
```

This adds a note for the ghostly shift spell when the spell is amped. The value is an array of the alterations to make. Each one is its own paragraph. Description additions cannot use HTML since it is added after the HTML is already sanitized, which means that if you want a second paragraph you need to break them up into different objects in the array. The `title` property is not required, only the `text`. The name of the altering item will be added to the top of the alteration box automatically. If you want to use formatting you need to supply it using markdown instead, with commands like `**bolded text** or *italicized text*`. You can also predicate each entry in the value array.

```json
{
  "itemType": "weapon",
  "key": "ItemAlteration",
  "mode": "add",
  "predicate": [
    "item:slug:longsword"
  ],
  "property": "description",
  "value": [
    {
      "text": "This longsword is extra **sharp**",
      "predicate": [
        {"not": "item:tag:shoddy"}
      ]
    },
    {
      "text": "Bust out the duct tape, this thing is *falling apart*!",
      "predicate": [
        "item:tag:shoddy"
      ]
    }
  ]
}
```

A description override replaces the text entirely, and can use HTML. Since this will go through a resolve pass you can use a description alteration to grab values from actor data that are not normally available to inlines, such as this example from Battlezoo Dragons that grabs data set in the actor flags by AE like rule elements on other features.

```json
{
    "itemType": "feat",
    "key": "ItemAlteration",
    "mode": "override",
    "predicate": [
        "deep-breath",
        "item:slug:dragon-breath"
    ],
    "property": "description",
    "value": [{
        "text": "You breathe in deeply and release the energy stored within you in a powerful exhalation. Your dragon breath is a @Template[type:{actor|flags.system.dragonAncestry.breath.shape}|distance:{actor|flags.system.dragonAncestry.breath.distance}] and deals @Damage[(2*max(,ceil(@actor.level/2))){actor|flags.system.dragonAncestry.breath.dieSize}[@actor.flags.system.dragonAncestry.breath.damageType]] damage. Each creature in the area must attempt a @Check[type:{actor|flags.system.dragonAncestry.breath.saveType}|dc:resolve(@actor.system.attributes.classOrSpellDC.value)|basic:true] saving throw against the higher of your class DC or spell DC. You can't use this ability again for 10 minutes; starting at level 3, you instead can't use the ability again for [[/r 1d4 #Recharge Dragon Breath]] rounds.",
        "predicate": [{
            "not": "breath-shape:burst"
        }]
    }, {
        "text": "You breathe in deeply and release the energy stored within you in a powerful exhalation. Your dragon breath is a @Template[type:{actor|flags.system.dragonAncestry.breath.shape}|distance:{actor|flags.system.dragonAncestry.breath.distance}] within 60 feet and deals @Damage[(2*max(2,ceil(@actor.level/2))){actor|flags.system.dragonAncestry.breath.dieSize}[@actor.flags.system.dragonAncestry.breath.damageType]] damage of a type depending on your heritage. Each creature in the area must attempt a @Check[type:{actor|flags.system.dragonAncestry.breath.saveType}|dc:resolve(@actor.system.attributes.classOrSpellDC.value)|basic:true] saving throw against the higher of your class DC or spell DC. You can't use this ability again for 10 minutes; starting at level 3, you instead can't use the ability again for [[/r 1d4 #Recharge Dragon Breath]] rounds.",
        "predicate": [
            "breath-shape:burst"
        ]
    }]
}
```

Right now the valid list of alteration properties and the modes they support is limited to this:

| Property | Item Type(s) | Mode(s) |
|---|---|---|
|ac-bonus|armor, shield|add, downgrade, override, remove, subtract, upgrade|
|area-size|spell|add, subtract, upgrade, downgrade, override|
|badge-max|effect|downgrade, override|
|badge-value|condition, effect|add, downgrade, override, remove, subtract, upgrade|
|bulk|armor, container, book, consumable, equipment, shield, treasure, weapon|override|
|category|armor|override|
|check-penalty|armor|add, downgrade, override, remove, subtract, upgrade|
|damage-dice-faces|weapon|downgrade, override, upgrade|
|damage-dice-number|weapon|add, downgrade, override, remove, subtract, upgrade|
|damage-type|weapon|override|
|defense-passive|spell|override|
|description|action, ancestry, affliction, armor, background, backpack, book, campaignFeature, class, condition, consumable, deity, effect, equipment, feat, heritage, kit, lore, melee, shield, spell, spellcastingEntry, treasure, weapon|add, override|
|dex-cap|armor|add, downgrade, override, remove, subtract, upgrade|
|focus-point-cost|spell|add, override, upgrade|
|frequency-max|action, feat|add, downgrade, multiply, override, remove, subtract, upgrade|
|frequency-per|action, feat|downgrade, override, upgrade|
|group|armor, weapon|override|
|hardness|armor, container, book, consumable, equipment, shield, treasure, weapon|add, downgrade, multiply, override, remove, subtract, upgrade|
|hp-max|armor, container, book, consumable, equipment, shield, treasure, weapon|add, downgrade, multiply, override, remove, subtract, upgrade|
|material-type|armor, container, book, consumable, equipment, shield, treasure, weapon|override|
|name|action, ancestry, affliction, armor, background, backpack, book, campaignFeature, class, condition, consumable, deity, effect, equipment, feat, heritage, kit, lore, melee, shield, spell, spellcastingEntry, treasure, weapon|override|
|other-tags|action, ancestry, affliction, armor, background, backpack, book, campaignFeature, class, condition, consumable, deity, effect, equipment, feat, heritage, kit, lore, melee, shield, spell, spellcastingEntry, treasure, weapon|add, subtract, remove|
|pd-recovery-dc|condition|add, downgrade, override, remove, subtract, upgrade|
|persistent-damage|condition|override|
|runes-potency|weapon, armor|upgrade, override|
|range-increment|weapon|add, multiply, override, remove, subtract|
|range-max|weapon|add, multiply, override, remove, subtract|
|rarity|armor, container, book, consumable, equipment, shield, treasure, weapon|override|
|resilient|armor|upgrade, override|
|speed-penalty|armor, shield|add, downgrade, override, remove, subtract, upgrade|
|strength|armor|add, downgrade, override, remove, subtract, upgrade|
|runes-striking|weapon|upgrade, override|
|traits|action, ancestry, affliction, armor, background, backpack, book, campaignFeature, class, condition, consumable, effect, equipment, feat, heritage, kit, melee, shield, spell, treasure, weapon|add, remove, subtract|

For the `frequency-per` property the values are listed below (`PT10M` is `per 10 minutes`, etc):

```
turn
round
PT1M
PT10M
PT1H
PT24H
day
P1W
P1M
P1Y
```

## Immunity, Weakness, Resistance

You can add immunity, weakness, resistance (IWR) to a creature with rule elements.  The automation for application of damage is not complete yet but this rule element lets you easily track your IWR.  Using any of them is quite straightforward.

example (fire weakness)
```json
{
    "key": "Weakness",
    "type": "fire",
    "value": 5
}
```

But they can also be handed roll syntax to evaluate the value.

example (void resist equal to half level minimum 1)
```json
{
    "key": "Resistance",
    "type": "void",
    "value": "max(1,floor(@actor.level/2))"
}
```

Immunity can be to a damage type or a condition.

example (bleed immunity)
```json
{
    "key": "Immunity",
    "type": "bleed"
}
```

You can also set exceptions.

example (resist all except force, ghost touch, and vitality)
```json
{
    "key": "Resistance",
    "type": "all-damage",
    "value": 5,
    "exceptions": [
      "force",
      "ghost-touch",
      "vitality"
    ]
}
```

You cannot set the type to a damage type or condition that does not exist however, if you added a resistance to "radiation" damage the resistance would not appear until a new homebrew damage type was added for it.

Similarly, we can define a situation where the resistance is doubled, like so:

```json
{
  "doubleVs": [
    "non-magical"
  ],
  "key": "Resistance",
  "type": "physical",
  "value": 5
}
```

You can also use this rule element to remove an immunity. For example, if you have a creature with the construct trait the system will automatically add immunities tied to that trait to the actor. But this rule element would remove the healing immunity.

```json
{
  "key": "Immunity",
  "mode": "remove",
  "type": [
    "healing"
  ]
}
```

You can also create custom weaknesses or resistances. Such as the Channel Protection Amulet, which gives resistance 5 to the vitality or void damage from heal and harm spells.
```json
{
    "definition": [{
        "or": [{
            "and": [
                "self:mode:living",
                "item:type:spell",
                "item:slug:harm"
            ]
        }, {
            "and": [
                "self:mode:undead",
                "item:type:spell",
                "item:slug:heal"
            ]
        }]
    }],
    "key": "Resistance",
    "label": "Channel Protection",
    "type": "custom",
    "value": 5
}
```

Exceptions can also be made as custom objects, such as resistance to physical except for magical silver (dawnsilver also counts as silver).
```json
{
  "exceptions": [
    {
      "definition": [
        "item:magical",
        {
          "or": [
            "damage:material:dawnsilver",
            "damage:material:silver"
          ]
        }
      ],
      "label": "Magical Silver"
    }
  ],
  "key": "Resistance",
  "type": "physical",
  "value": 7
}
```

## Martial Proficiency

The MartialProficiency rule element creates a proficiency in a definable set of weapons. This RE from the Gunslinger class for example

```json
{
    "definition": [
        "item:category:simple",
        { "or": [
            "item:group:firearm", "item:tag:crossbow"
        ] }
    ],
    "key": "MartialProficiency",
    "label": "PF2E.SpecificRule.MartialProficiency.SimpleFirearmsCrossbows",
    "slug": "simple-firearms-crossbows",
    "value": 2
}
```

This creates an expert (2) proficiency in all simple firearms and crossbows. A separate AE-Like rule can then increase the proficiency to master (3) by referencing the slug provided by this RE like so:
```json
{
    "key": "ActiveEffectLike",
    "mode": "upgrade",
    "path": "system.proficiencies.attacks.simple-firearms-crossbows.rank",
    "value": 3
}
```

Another example from the Dwarven Weapon Familiarity feat shows how this rule can be used to treat a group of weapons as a different category, this will make the system treat each of the qualifying weapons as simple weapons.
```json
{
    "definition": [{
        "or": [{
                "and": [
                    "item:trait:dwarf",
                    "item:category:martial"
                ]
            },
            "item:base:battle-axe",
            "item:base:warhammer",
            "item:base:pick"
        ]
    }],
    "key": "MartialProficiency",
    "label": "PF2E.SpecificRule.MartialProficiency.MartialDwarfWeapons",
    "sameAs": "simple",
    "slug": "martial-dwarf-weapons"
}
```

## Multiple Attack Penalty

The Multiple Attack Penalty rule element can change the MAP Progression of specific strikes, or the character as a whole. The two sample Rule Elements implement a flurry ranger, put them on an Effect item and they'll make you hit very often
```json
{
    "key": "MultipleAttackPenalty",
    "predicate": [
        "agile", "hunted-prey"
    ],
    "selector": "attack",
    "value": -2
}
```

```json
{
    "key": "MultipleAttackPenalty",
    "predicate": [
        "hunted-prey",
        { "not": "agile" }
    ],
    "selector": "attack",
    "value": -3
}
```

Some things to be aware of when using this rule element: First is that MAP is calculated before target roll options are acquired, so you cannot change the MAP value based on your target. Second is that this rule works by presenting the system with a new MAP modifier and the system will always take the best modifier available. So to handle things that *increase* MAP you would want to use the AdjustModifier rule element with the slug `multiple-attack-penalty`.

## Note

The Note Rule Element adds additional text to the chat output of a roll. The note rule element has a UI, but full JSONs are shown here to demonstrate the underlying structure. This example adds a reminder to all reflex saves. The text in the title field will be put into `<strong>` tags automatically

```json
{
    "key":"Note",
    "selector":"reflex",
    "title": "{item|name}",
    "text":"When you roll a success on a Reflex save, you get a critical success instead."
}
```

This will post a note when you roll a reflex save. But you may want to only have this note show up on a successful saving throw, so there is an `outcome` option. As seen on the Bravery feature
```json
{
    "key": "Note",
    "predicate": [
        "fear"
    ],
    "selector": "will",
    "text": "When you roll a success at a Will save against a fear effect, you get a critical success instead.",
    "title": "{item|name}",
    "outcome": [
        "success"
    ]
}
```

Now this note will only show up on fear effects where you roll a success.
In each of these we have used `{item|name}` in the title field to auto fill in the name of the feat or feature automatically. We can go one further and use the same approach for the text itself as well, here we've added a free action icon before the description to remind the user that it is a free action.
```json
{
    "key":"Note",
    "selector":"initiative",
    "title": "{item|name}",
    "text":"<span class=\"action-glyph\">f</span> {item|description}"
}
```

This will post the entire text of the item on every initiative roll your actor makes.

## Roll Option

RollOption allows you to create new and arbitrary roll options, and lets you add them as toggles on the sheet. A RollOption RE has two mandatory fields: `domain` and `option`. These correspond to the place under `pf2e.flags.roll-options` that the property is set.  The option is the thing that gets predicated on in other rule elements. 
```json
{
    "key": "RollOption",
    "domain": "all",
    "option": "afraid-of-dragons"
}
```

This would set the roll option `afraid-of-dragons`, which another RE could pick up on.
```json
{
    "key": "FlatModifier",
    "selector": "will",
    "value": -2,
    "type": "circumstance",
    "predicate": ["afraid-of-dragons"]
}
```

You can use any domain or option, but not all types of rolls check all domains for options.  For example an Athletics roll does not check the `damage-roll` or `ac` domains.  All rolls check the `all` domain, however we want to avoid overstuffing one domain. As a general rule use `all` if unsure or if multiple types of roll will predicate off this option.  Other rule elements can be told to check non-standard domains by adding the line `"roll-options":["domain1"]"`.  Notice that the brackets indicate an array, so you can include multiple domains to check, but you must have the brackets there.

You can also use RollOption to create toggleable checkboxes.
```json
{
    "domain": "ac",
    "key": "RollOption",
    "option": "nimble-dodge",
    "toggleable": true
}
```

Like most rule elements this will take the name of the item it is on as a label, but one can be provided:
```json
{
    "domain": "all",
    "key": "RollOption",
    "option": "afraid-of-dragons",
    "toggleable": true,
    "label": "Dragons are scary"
}
```

For effects that should be defaulted to on you can set the default behavior inside the RE as well
```json
{
    "domain": "all",
    "key": "RollOption",
    "label": "PF2E.SpecificRule.Psychic.UnleashPsyche.DamageLabel",
    "option": "unleash-psyche-damage",
    "toggleable": true,
    "value": true
}
```

RollOptions with toggles can also have suboptions, which create a selectable menu to choose from sub-options. From Spirit Instinct for example.
```json
{
  "domain": "all",
  "key": "RollOption",
  "label": "PF2E.SpecificRule.Barbarian.Spirit.ToggleLabel",
  "option": "spirit-rage",
  "predicate": [
    {
      "or": [
        "class:barbarian",
        "feat:instinct-ability"
      ]
    }
  ],
  "suboptions": [
    {
      "label": "Vitality Damage",
      "value": "vitality"
    },
    {
      "label": "Void Damage",
      "value": "void"
    }
  ],
  "toggleable": true
}
```

This damage type is then referenced in an AdjustModifier rule element
```json
{
  "damageType": "{item|flags.system.rulesSelections.spiritRage}",
  "key": "AdjustModifier",
  "mode": "upgrade",
  "predicate": [
    "spirit-rage"
  ],
  "selectors": [
    "strike-damage"
  ],
  "slug": "rage",
  "value": 3
}
```

The `spiritRage` flag is set by turning the `option` field to camel case. Since flags are item data they should be camel case, while roll options are always kebab case.

Suboptions must be strings, even if the value is a number
```json
{
    "domain": "all",
    "key": "RollOption",
    "option": "extra-power",
    "suboptions": [{
        "label": "Critical Success",
        "value": "2"
    }, {
        "label": "Success",
        "value": "1"
    }],
    "label": "Extra Power",
    "toggleable": true
}
```

When referenced using `@item.flags.system.rulesSelections.extraPower` this would be coerced to a number.

Toggles can be set to `alwaysActive`
```json
{
    "key": "RollOption",
    "option": "brass-dwarf",
    "suboptions": [{
        "label": "PF2E.TraitAcid",
        "value": "acid"
    }, {
        "label": "PF2E.TraitCold",
        "value": "cold"
    }, {
        "label": "PF2E.TraitElectricity",
        "value": "electricity"
    }, {
        "label": "PF2E.TraitFire",
        "value": "fire"
    }, {
        "label": "PF2E.TraitMental",
        "value": "mental"
    }, {
        "label": "PF2E.TraitPoison",
        "value": "poison"
    }, {
        "label": "PF2E.TraitSonic",
        "value": "sonic"
    }],
    "toggleable": true,
    "alwaysActive": true,
    "label": "Brass Dwarf Resistance"
}
```

You can predicate suboptions just like you can predicate a choice in a ChoiceSet.
```json
{
    "domain": "all",
    "key": "RollOption",
    "option": "spirit-striking-damage",
    "label": "Spirit Striking Damage",
    "suboptions": [{
        "label": "PF2E.TraitAcid",
        "value": "acid",
        "predicate": [
            "feature:restless-as-the-tide",
            "feat:dominion-signifier"
        ]
    }, {
        "label": "PF2E.TraitBludgeoning",
        "value": "bludgeoning",
        "predicate": [{
            "not": "feature:whose-cry-is-thunder"
        }]
    }],
    "toggleable": true,
    "alwaysActive": true
}
```

You can also make the toggle disabled but still visible, including setting the value when disabled.
```json
{
    "disabledIf": [{
        "not": "self:effect:panache"
    }],
    "disabledValue": false,
    "domain": "all",
    "key": "RollOption",
    "label": "PF2E.SpecificRule.Swashbuckler.Finisher.Label",
    "option": "finisher",
    "suboptions": [{
        "label": "PF2E.SpecificRule.Swashbuckler.Finisher.Basic",
        "value": "basic"
    }, {
        "label": "PF2E.SpecificRule.Swashbuckler.Finisher.Confident",
        "predicate": [
            "feature:confident-finisher"
        ],
        "value": "confident"
    }],
    "toggleable": true
}
```

## Roll Twice

You can use the RollTwice rule element to automatically set fortune or misfortune effects.

```json
{
    "key": "RollTwice",
    "keep": "higher",
    "selector": "attack-roll"
}
```

By default if the source is an effect item then the effect will auto expire after a single roll is made. This behavior can be overridden by setting `"removeAfterRoll": false` in the rule element.

## Sense

A Rule Element for Feats, Spells, and Items that grant additional senses or increase the acuity of existing ones.
```json
{
    "key": "Sense",
    "selector": "darkvision"
}
```

```json
{
    "key": "Sense",
    "selector": "low-light-vision"
}
```

```json
{
    "acuity": "imprecise",
    "key": "Sense",
    "range": 30,
    "selector": "scent"
}
```

For these Rules, the Label is queried from the language database.

## Special Resource

The SpecialResource rule lets you define a special resource on the actor that will be tracked in the sidebar on the actor's sheet.

```json
{
  "key": "SpecialResource",
  "label": "Will Points",
  "max": "2 * @actor.system.abilities.wis.mod",
  "slug": "will-points"
}
```

![image](https://github.com/user-attachments/assets/52ea3dfe-ba0a-4421-b848-695fbceb2afe)

Here we define a new resource called Will Points that gives the actor a pool of points equal to twice their wisdom modifier. When choosing a slug you have to avoid using the same name as something already in existence on the actor. For example, if you wanted to call these Resolve instead, you cannot use a slug of `resolve` because that already exists on actors for the official stamina variant rule. When you rest for the night, all special resources will be replenished.

You can tie a resource to a physical item, like Alchemist's Versatile Vials

```json
{
  "itemUUID": "Compendium.pf2e.equipment-srd.Item.ljT5pe8D7rudJqus",
  "key": "SpecialResource",
  "label": "Versatile Vials",
  "level": "ternary(gte(@actor.level, 18), 18, ternary(gte(@actor.level, 12), 12, ternary(gte(@actor.level, 4), 4, 1)))",
  "max": "2 + @actor.system.abilities.int.mod",
  "slug": "versatile-vials"
}
```

We tell the system the UUID of the physical item in question, and we can also set the level of those items when they get added to the actor (Because a high level alchemist makes more potent vials).

We can also initialize the rule with less than maximum value by declaring the value.

```json
{
  "key": "SpecialResource",
  "label": "Will Points",
  "max": "max(1,2 * @actor.system.abilities.wis.mod)",
  "value": 1,
  "slug": "will-points"
}
```

Here we have set the value to 1, and added a `max()` call to ensure that the maximum number of points is at least 1. This will still refill to max on rest, however. If we want to make it so the resource does not refresh daily by setting `"renew":false`. The only options for the `renew` parameter are `"daily"` (the default) and `false`.


## Special Statistic

SpecialStatistic lets you define a new statistic on an actor. Statistics are the things that you can roll or roll against, like a skill, save, ac, etc. These can extend from an existing statistic, like the deviant abilities extending from the better of your class DC or spell DC.
```json
{
    "extends": "class-spell",
    "key": "SpecialStatistic",
    "label": "PF2E.SpecificRule.DeviantAbilities.Label",
    "slug": "deviant",
    "type": "attack-roll"
}
```

Normally your class DC does not make rolls, but if you use an inline button like `@Check[type:class]` you can make a roll with it. However, class DC doesn't make attack rolls, so an actor under the effects of Courageous Anthem would not get the bonus to attack that should be granted from that. By extending `class-spell` we not only get the proper value on all actors but we also gave it the `type` of `attack-roll` which will allow it to pick up proper automation. Now we can make inline rolls with this using `@Check[type:deviant]`. 

The type defaults to `check`, but can be `check` or `attack-roll`.

You can also extend a specific class DC, such as the kineticist class DC
```json
{
    "extends": "kineticist",
    "key": "SpecialStatistic",
    "label": "PF2E.TraitImpulse",
    "slug": "impulse",
    "type": "attack-roll"
}
```

This is needed to target just the impulse attack rolls with a Gate Attenuator.
```json
{
    "key": "FlatModifier",
    "predicate": [{
        "or": [
            "class:kineticist",
            "feat:kineticist-dedication"
        ]
    }],
    "selector": "impulse-attack-roll",
    "type": "item",
    "value": 1
}
```

SpecialStatistic can also be used to build a statistic from scratch. For example, if we wanted to create a new "Counteract" statistic that an actor is trained in we could do this with these rules.
```json
{
    "key": "SpecialStatistic",
    "label": "Counteract",
    "slug": "counteract",
    "type": "check",
    "attribute": "cha"
}
```

```json
{
    "key": "FlatModifier",
    "selector": "counteract",
    "value": "2 + @actor.level",
    "type": "proficiency",
    "label": "Proficiency"
}
```

The FlatModifier adds in the equivalent of a trained proficiency to the roll.

## Strike

This rule element is a bit trickier in that there are a lot of necessary fields. The easiest way to create your own custom strike would be to start from a finished strike like the monk's Tiger Claw strike here.
```json
{
    "key": "Strike",
    "category": "unarmed",
    "damage": {
        "base": {
            "damageType": "slashing",
            "dice": 1,
            "die": "d8"
        }
    },
    "group": "brawling",
    "label": "Tiger Claw",
    "traits": [
        "agile",
        "finesse",
        "unarmed",
        "nonlethal"
    ]
}
```

If necessary an `"ability": "int"` entry could be used for a strike that scales its to-hit with intelligence, as the "Spiritual Weapon" spell could. This works with the other 3 character ability shorthands too. To add a Flat part to the damage like an ability modifier, add an additional Flat Modifier Element.
```json
{
    "category": "unarmed",
    "damage": {
        "base": {
            "damageType": "mental",
            "dice": 1,
            "die": "d4"
        }
    },
    "ability": "int",
    "group": "brawling",
    "key": "Strike",
    "label": "Smart Punch",
    "traits": [
        "unarmed"
    ]
}
```

An optional `"range"` parameter can be given to denote the range of an attack, like this example from Sprite's Spark.
```json
{
    "category": "unarmed",
    "damage": {
        "base": {
            "damageType": "mental",
            "dice": 1,
            "die": "d4"
        }
    },
    "group": "sling",
    "key": "Strike",
    "label": "PF2E.SpecificRule.Sprite.SpritesSpark.Draxie",
    "predicate": [
        "self:heritage:draxie"
    ],
    "range": {
        "max": 20
    },
    "traits": [
        "unarmed"
    ]
}
```

If your ranged strike has an increment instead of a maximum range you can use `"increment": 30` instead of or in addition to the `max`.

The strike rule element can also be used to generate a strike with specific attack and damage modifiers, mostly useful for NPCs.
```json
{
    "damage": {
        "base": {
            "damageType": "bludgeoning",
            "dice": 1,
            "die": "d6",
            "modifier": 6
        }
    },
    "attackModifier": 10,
    "traits": ["magical"],
    "key": "Strike",
    "slug": "bone-club",
    "label": "Bone Club",
    "predicate": ["bone-club"]
}
```

If you have additional damage or attack modifiers to add to the strike from other abilities use FlatModifier or DamageDice rule elements as needed.

You can also specify the image to be used for the strike.
```json
{
    "category": "unarmed",
    "damage": {
        "base": {
            "damageType": "electricity",
            "dice": 1,
            "die": "d6"
        }
    },
    "img": "icons/magic/lightning/bolt-strike-streak-yellow.webp",
    "key": "Strike",
    "label": "Eye Beam",
    "slug": "eye-beam",
    "range": {
        "increment": 60
    },
    "traits": ["unarmed", "electricity"]
}
```

You can also specify the `baseType` if the strike should qualify as a base type of existing weapon or strike, such as with `"baseType": "longsword"`

Finally, you can replace all other strikes by adding `"replaceAll": true` or just the basic unarmed attack by adding `"replaceBasicUnarmed": true`

## Substitute Roll

The SubstituteRoll rule element allows you to manually set the result of a roll. This is mostly useful for assurance, but may be expanded to be how the system handles abilities like Devise a Stratagem, or Perfected Form.

Assurance is done in two parts, one is the SubstituteRoll and the other is an AdjustModifier to remove all non proficiency modifiers.

```json
{
    "key": "SubstituteRoll",
    "label": "PF2E.SpecificRule.SubstituteRoll.Assurance",
    "selector": "{item|flags.system.rulesSelections.assurance}",
    "slug": "assurance",
    "value": 10
}
```

```json
{
    "key": "AdjustModifier",
    "predicate": [
        "substitute:assurance",
        { "not": "bonus:type:proficiency" }
    ],
    "selector": "{item|flags.system.rulesSelections.assurance}",
    "suppress": true
}
```

The flags are set by the ChoiceSet rule element on the Assurance feat.

Another example in the system is the Devise a Stratagem effect. This effect has a formula counter of `1d20` so it rolls a d20 when added to the actor, then references that badge value to substitute if the target is marked with the TokenMark rule element. This also shows that we can force the substitution and remove it after the roll.
```json
{
    "key": "SubstituteRoll",
    "predicate": [
        "target:mark:devise-a-stratagem"
    ],
    "removeAfterRoll": "if-enabled",
    "required": true,
    "selector": "strike-attack-roll",
    "slug": "devise-a-stratagem",
    "value": "@item.badge.value"
}
```

The chat card created from this roll has a bit of extra context on it, allowing the damage button from that roll to enable the extra damage from Strategic Strike by predicating on `"check:substitution:devise-a-stratagem"` which uses the slug of the substitution.

## Temp HP

Just as the name implies this Rule element adds Temporary HP to a character the moment the effect is added. Many Temp HP Effects derive their value from player stats, the Rage Element is a prime example of how this can be done with the system. The first example is from the Aeon Stone (Pink Rhomboid).
```json
{
    "key": "TempHP",
    "value": 15
}
```

```json
{
    "key": "TempHP",
    "value": "@actor.level + @actor.abilities.con.mod"
}
```

You can also specify events for the temporary HP to be added on, like this from Arcane Cascade for the Inexorable Iron hybrid study.
```json
{
  "events": {
    "onCreate": true,
    "onTurnStart": true
  },
  "key": "TempHP",
  "predicate": [
    "feature:inexorable-iron",
    "self:weapon:melee",
    "self:weapon:hands-held:2"
  ],
  "value": "max(floor(@actor.level / 2), 1)"
}
```

These will refresh when the item is added to the actor and at the start of each turn. These are the only two events currently supported.

## Token Effect Icon

This will apply an effect icon to the token it is applied to.

By Default the Effect Icon is the Icon of the Item/feat/Effect that carries the Rule element, to change that an additional field can be used.
```json
{
    "key": "TokenEffectIcon",
    "value": "systems/pf2e/icons/spells/all-is-one-one-is-all.jpg"
}
```

## Token Image

This rule element will change the token of an actor to the boar token that is included in the system. It is useful for Wild Shape or can be customized to set expressions on PCs per effect item.

TokenImage
```json
{
    "key": "TokenImage",
    "value": "systems/pf2e/icons/bestiary-1/boar.webp"
}
```

You can also predicate the image change
```json
{
    "key": "TokenImage",
    "value": "images/doggo_form.webp",
    "predicate": ["self:effect:animal-form-canine"]
}
```

You can also specify a token scale in this rule element, this is useful for token art that breaks the borders of the square.
```json
{
    "key": "TokenImage",
    "value": "images/tokens/big-wings.webp",
    "scale": 2.5
}
```

This can also be used to apply a tint or change the alpha of a token, this could be used to simulate the effect of a token going underwater for example.
```json
{
  "key": "TokenImage",
  "value": "{actor|prototypeToken.texture.src}",
  "tint": "#3bde97",
  "alpha": 0.9
}
```

Here the value points back to the currently set token image for the prototype token. Unfortunately multiple token image rules won't work together, and Foundry will just take the last one applied.

Another property that cannot be edited in the form currently is the transition animation. You can change how the token transitions between images by specifying the animation to use, like so.

```json
{
  "key": "TokenImage",
  "value": "images/wolf.webp",
  "scale": 2,
  "animation": {
    "transition": "hologram"
  }
}
```

The list of possible animations can be found by opening console (F12 in most browsers) and entering `TextureTransitionFilter.TYPES`. As of now the list is

```
fade
swirl
waterDrop
morph
crosshatch
wind
waves
whiteNoise
hologram
hole
holeSwirl
glitch
dots
```

More may be added in the future.

## Token Light

The TokenLight rule element can be used to add light to a token with an effect. It has a full UI that makes adding it and configuring any part of it easy. But the full JSON objects are presented here to show how it works.
```json
{
    "key": "TokenLight",
    "value": {
        "animation": {
            "intensity": 4,
            "speed": 1,
            "type": "torch"
        },
        "bright": 20,
        "color": "#9b7337",
        "dim": 40,
        "shadows": 0.2
    }
}
```

```json
{
    "key": "TokenLight",
    "value": {
        "animation": {
            "intensity": 1,
            "speed": 1,
            "type": "lightdome"
        },
        "bright": 30,
        "color": "#9b7337",
        "dim": 0,
        "shadows": 0,
        "luminosity": 0.5,
        "gradual": false,
        "contrast": 0.5,
        "saturation": 0,
        "coloration": 1,
        "angle": 360,
        "alpha": 0.1
    }
}
```

The one exception to the form's completeness is the new v12 animated transitions. Those can be added with

```json
{
  "key": "TokenImage",
  "value": "modules/pf2e-tokens-bestiaries/tokens/draconic/dragons/dragon-red-adult.webp",
  "predicate": [
    "self:effect:dragon-form-diabolic"
  ],
  "scale": 2,
  "animation": {
    "transition": "swirl"
  }
}
```

The transition types are `crosshatch, dots, fade, glitch, hole, holeSwirl, hologram, morph, swirl, waterDrop, waves, whiteNoise, wind`

## Token Mark
TokenMark is a simple, yet powerful Rule Element that can be used to mark a token, which can be used as a predicate for other rule elements. For example, Devise a Strategem uses this rule element:
```json
{
  "key": "TokenMark",
  "slug": "devise-a-stratagem"
}
```

Then other rule elements can predicate on `target:mark:devise-a-strategem`
```json
{
  "key": "SubstituteRoll",
  "predicate": [
    {
      "or": [
        "devise-a-stratagem",
        "target:mark:devise-a-stratagem"
      ]
    }
  ],
  "removeAfterRoll": "if-enabled",
  "required": true,
  "selector": "strike-attack-roll",
  "slug": "devise-a-stratagem",
  "value": "@item.badge.value"
}
```

```json
{
  "key": "SubstituteRoll",
  "predicate": [
    "feat:athletic-strategist",
    {
      "or": [
        "devise-a-stratagem",
        "target:mark:devise-a-stratagem"
      ]
    },
    {
      "or": [
        "action:disarm",
        "action:grapple",
        "action:shove",
        "action:trip"
      ]
    }
  ],
  "removeAfterRoll": "if-enabled",
  "required": true,
  "selector": "athletics",
  "slug": "devise-a-strategem",
  "value": "@item.badge.value"
}
```

When the effect is applied to a token, a UI notification will appear at the top of the screen giving you 15 seconds to select a target by hovering a token a pressing T if one is not already targeted, or making the currently targeted token the marked token. If no target is selected within 15 seconds, the selection will timeout and the effect will not be applied. If you are targeting your marked token then the roll option `target:mark:<slug>` will be included in the roll data.

## Token Name

TokenName is a simple rule element that overrides a token's name. This is useful if you have a disguise effect.
```json
{
    "key": "TokenName",
    "value": "Guy Incognito"
}
```
