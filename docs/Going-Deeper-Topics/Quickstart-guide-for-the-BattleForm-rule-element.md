The `BattleForm` rule element is the largest and most complicated rule element, this is because it is an amalgamation of many other rule elements. It works by overriding the base actor statistics, including replacing the base unarmed strike.

Let's look at a relatively straightforward example:

Example (Unicorn Form)
```json
{
	"key": "BattleForm",
	"overrides": {
		"armorClass": {
			"modifier": "19 + @actor.level"
		},
		"resistances": [{
			"type": "poison",
			"value": 5
		}],
		"senses": {
			"lowLightVision": {}
		},
		"size": "lg",
		"skills": {
			"acr": {
				"modifier": 16
			}
		},
		"speeds": {
			"land": 40
		},
		"strikes": {
			"hoof": {
				"ability": "dex",
				"category": "unarmed",
				"damage": {
					"damageType": "bludgeoning",
					"dice": 1,
					"die": "d8"
				},
				"group": "brawling",
				"modifier": 16,
				"traits": ["unarmed", "agile"]
			},
			"horn": {
				"ability": "dex",
				"category": "unarmed",
				"damage": {
					"damageType": "piercing",
					"dice": 1,
					"die": "d10",
					"modifier": 8
				},
				"group": "brawling",
				"modifier": 16,
				"traits": ["unarmed"]
			}
		},
		"tempHP": 15,
		"traits": ["fey", "beast"]
	}
}
```
There's several things to notes about this beyond the size.  First is that all the properties are under the `"overrides": {}` structure. We set things on the base actor to be overridden, taking these in the order listed we see that the armor class is overridden with a new formula.  Next is a resistance, then senses, size, skills, speed, strikes, and traits.  The rule element even allows the temp HP to be added.

The next thing to notice is that each of these overrides use a different syntax, depending on the structure of the thing they are overriding since the thing we feed the actor data should match the structure of that data.

Let's look at these pieces under `override` in more depth to see how they all work.

## Immunity, Weakness, Resistance

As seen above a resistance can be added by giving it a type and value. Note that `resistances` is followed by an array `[]` and each resistance is an object in that array. Below you can see how multiple resistances are done.

```json
"resistances": [
	{
		"type": "poison",
		"value": 5
	},
	{
		"type": "fire",
		"value": 5,
		"except": "magical"
	}]
```
Included in this example is an exception to the fire resistance which will show up onn the sheet. Immunity and weakness can be done similarly, with the biggest difference being that immunity does not accept a value.


## Senses
The senses override can grant multiple senses at once, and even specify the acuity and range of the sense
```json
"senses": {
			"lowLightVision": {},
			"scent": {
				"acuity": "imprecise",
				"range": 30
			}
		},
```
Note that this is not an array (simple list) of senses, each one has an object assigned to it. You can see here that `lowLightVision` still needs the trailing braces passing an empty object. Scent and most other senses require an acuity and range to be specified. Low light and darkvision have default `precise` acuity and unlimited range.

## Size
Size is as straightforward, you can override the size of a creature. The sizes in the system are `tiny`, `sm`, `med`, `lg`, `huge`, and `grg`.
```json
"size": "grg",
```

## Skills
Skills can be overridden, most forms will override Athletics but some will grant Acrobatics overrides as well. Skills are pass in an object, and each skill itself is an object with a `modifier`.

```json
"skills": {
	"athletics": {
		"modifier": 18
	},
	"acrobatics": {
		"modifier": 18
	}
},
```

## Speeds
The speeds are passed as an object, with each speed type being separated by a comma like so.
```json
"speeds": {
	"land": 40,
	"fly": 100
},
```

## Traits
Traits are passed as an array of the traits.
```json
"traits": ["undead", "vampire"]
```

## Use Own Unarmed Value (if higher)
You can also set a boolean flag for the form allowing you to use your own unarmed attack proficiency if it is higher. This is set per strike, inside the strike object. If left out it defaults to true.

```json
"ownIfHigher": false,
```

## Keep existing unarmed strikes
If your battleform lets you keep your existing unarmed strikes then this property can be used. If left out it defaults to false.

```json
"ownUnarmed": true,
```

## Other Options
There are other options that can be passed to the BattleForm.  These are not functional now, but in the future could be used for automation. They all default to false.  Note that in pf2e these are only true if the spell explicitly says youy can speak or have hands, not if the creature logically could speak.
```json
"hasHands":true,
"canCast": false,
"canSpeak": false,
```

## Strikes
Strikes are the most complicated part of the above example, but each line should be straightforward to understand what it is doing. This is similar to the strike rule element, in fact all of the above pieces should look similar in structure to their respective REs. This is because the BattleForm RE uses the underlying code of all of those to implement itself in a single package.

```json
"strikes": {
	"horn": {
		"category": "unarmed",
		"damage": {
			"damageType": "piercing",
			"dice": 1,
			"die": "d8"
		},
		"group": "brawling",
		"label": "PF2E.Battleform.Attack.Horn",
		"traits": ["unarmed", "deadly-d10"]
	},
	"jaws": {
		"baseType": "jaws",
		"category": "unarmed",
		"damage": {
			"damageType": "piercing",
			"dice": 2,
			"die": "d8"
		},
		"group": "brawling",
		"label": "PF2E.Weapon.Base.jaws",
		"traits": ["unarmed"]
	},
	"acidSpit": {
		"category": "martial",
                "ability": "dex",
		"damage": {
			"damageType": "acid",
			"dice": 3,
			"die": "d6"
		},
		"group": "bomb",
		"label": "Acid Spit",
		"traits": ["unarmed","range-30"]
	}
},
```
Some forms will include additional effects such as an angel form with a flaming longsword may need a second DamageDice RE to add the d6 of fire damage and a Note RE to add a persistent damage note to critical hits. These can reference the strikes made by the BattleForm RE the same way they can reference any strike, by name in the selector. In the above example these can be referenced in the selector of another RE by `horn-damage` or `jaws-attack`, etc.

Also note that the label can be set using a translation, by default it will use the name given under strikes but by calling `PF2E.Weapon.Base.jaws` the text is pulled from a language file and is automatically translated.  This does not change the underlying name of the strike, so the above REs referencing `claw-damage` would still work.

If your strike is a `claw`, `jaws`, `fist`, or a physical weapon like `longsword` you will want `baseType` set.  This is because those are base weapons in the system.  Only those 3 unarmed strikes are baseTypes for this.

The ability score used for the attack can be manually overridden.  This won't change the damage, but it will change whether the attack is affected by enfeebled, clumsy, etc as well as changing the breakdown of the attack.  strength is the default and does not need to be changed.

Something not shown here is that a custom image can be supplied, however if the RE is not told to use an image it will try to find a matching image (match by name) from the system's image folder. Before specifying an image see if the system can find one without needing to do so.

# Bracketing Data

Many form spells can be heightened, this heightening is handled by brackets. *Outside* the override object we can place a value followed by our brackets for levels. Inside the values of each bracket we can add the same type of structure to modify the base strike. Note that any property not included in the brackets falls back to the base override data.

```json
{
	"key": "BattleForm",
	"label": "Animal Form (Bear)",
	"overrides": {
		"senses": {
			"lowLightVision": {},
			"scent": {
				"acuity": "imprecise",
				"range": 30
			}
		},
		"speeds": {
			"land": 30
		},
		"strikes": {
			"claw": {
				"baseType": "claw",
				"category": "unarmed",
				"damage": {
					"damageType": "slashing",
					"dice": 1,
					"die": "d8"
				},
				"group": "brawling",
				"label": "PF2E.Weapon.Base.claw",
				"traits": ["unarmed", "agile"]
			},
			"jaws": {
				"baseType": "jaws",
				"category": "unarmed",
				"damage": {
					"damageType": "piercing",
					"dice": 2,
					"die": "d8"
				},
				"group": "brawling",
				"label": "PF2E.Weapon.Base.jaws",
				"traits": ["unarmed"]
			}
		},
		"traits": ["animal"]
	},
	"value": {
		"brackets": [{
			"end": 2,
			"start": 2,
			"value": {
				"armorClass": {
					"modifier": "16 + @details.level.value"
				},
				"size": "med",
				"skills": {
					"ath": {
						"modifier": 9
					}
				},
				"strikes": {
					"claw": {
						"damage": {
							"modifier": 1
						},
						"modifier": 9
					},
					"jaws": {
						"damage": {
							"modifier": 1
						},
						"modifier": 9
					}
				},
				"tempHP": 5
			}
		}, {
			"end": 3,
			"start": 3,
			"value": {
				"armorClass": {
					"modifier": "17 + @details.level.value"
				},
				"skills": {
					"ath": {
						"modifier": 14
					}
				},
				"strikes": {
					"claw": {
						"damage": {
							"modifier": 5
						},
						"modifier": 14
					},
					"jaws": {
						"damage": {
							"modifier": 5
						},
						"modifier": 14
					}
				},
				"tempHP": 10
			}
		}, {
			"end": 4,
			"start": 4,
			"value": {
				"armorClass": {
					"modifier": "18 + @details.level.value"
				},
				"size": "lg",
				"skills": {
					"ath": {
						"modifier": 16
					}
				},
				"strikes": {
					"claw": {
						"damage": {
							"modifier": 9
						},
						"modifier": 16,
						"traits": ["unarmed", "agile", "reach"]
					},
					"jaws": {
						"damage": {
							"modifier": 9
						},
						"modifier": 16,
						"traits": ["unarmed", "reach"]
					}
				},
				"tempHP": 15
			}
		}, {
			"start": 5,
			"value": {
				"armorClass": {
					"modifier": "18 + @details.level.value"
				},
				"size": "huge",
				"skills": {
					"ath": {
						"modifier": 20
					}
				},
				"strikes": {
					"claw": {
						"damage": {
							"dice": 2,
							"modifier": 7
						},
						"modifier": 18,
						"traits": ["unarmed", "agile", "reach-15"]
					},
					"jaws": {
						"damage": {
							"dice": 4,
							"modifier": 7
						},
						"modifier": 18,
						"traits": ["unarmed", "reach-15"]
					}
				},
				"tempHP": 20
			}
		}],
		"field": "item|data.level.value"
	}
}
```