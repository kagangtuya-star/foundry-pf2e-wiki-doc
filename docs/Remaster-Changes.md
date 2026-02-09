<p>With the 5.9.0 release of the Pathfinder 2e Foundry system we have moved fully to the remaster rules. We want the transition to these rules to be as seamless as possible for groups. This journal is here as a reference for GMs or players who are looking for an item but can't locate it in the system compendiums, and we have future plans to expand the ability to find items quickly even if you can't remember that <em>Scorching Ray</em> is now called <em>Blazing Bolt</em>.</p>
<p>On this page we list the feat, spells, and other items that were heavily altered, merged together, or removed from the system with this release. The remaster will not be complete until the release of Player Core 2, however this release represents the bulk of the rules changes that will happen.</p>
<p>Some items have been removed from the system as they are not compatible with the new rules. Alignment ampoules, for example, are not usable in a game without alignment. However, because we know that people want to use new releases while not fully moving to the remaster rules a copy of all of the legacy content has been made available through the PF2e Legacy Content module. This module also adds alignment back to actors.</p>
<p>This is not a full changelog of the remaster changes. That changelog would be exceptionally large, as many feats and spells had minor tweaks to the descriptions. A full changelog can be built from our github repository, though be aware that the diff file produced affected several thousand files, mostly in changing stored name references.</p>
<h2>Rules changes</h2>
<p>In this release, the system implemented the following system wide rules changes:</p>
<p><strong>Removal of Alignment</strong> Alignment has been removed from the system. Alignment that had been set before on actors is still in actor data in the form of inert traits. These traits are not displayed on the sheet until a module adds the traits back. The PF2e Legacy Data module does this, and smaller modules can easily do this. You can also do this without a module by simply registering the four alignment traits in the system homebrew settings menu as actor traits.</p>
<p>Evil fiends and good celestials have had the unholy or holy trait added to them. NPC strikes that dealt evil or good damage have been converted to deal spirit damage with the unholy or holy trait.</p>
<p>Instances of alignment damage in text have been changed to spirit damage. This has been done programmatically, and may result in some errors where spirit damage does not make sense to use. If you find one of these situations, please make an issue for us to remedy it.</p>
<p><strong>Removal of Spell Schools</strong> Seven of the eight spell schools have been removed: Abjuration, conjuration, divination, enchantment, evocation, necromancy, and transmutation have been removed from spells, feats, equipment, and the like. Illusion is still a trait used in the remaster rules and has been kept.</p>
<p><strong>Removal of Spell Components</strong> Spell components (verbal, somatic, material) have been removed from all spells. If a spell had a verbal component it gained the concentrate trait. If the spell had a somatic component it gained the manipulate trait. As a general rule, all spells now require speaking. This general rule is modified by the new 'subtle' trait.</p>
<h2>Languages</h2>
<p>Several languages have been renamed.</p>
<ul>
    <li><span>Abyssal is now Chthonian.</span></li>
    <li><span>Aquan is now Thalassic.</span></li>
    <li><span>Auran is now Sussuran.</span></li>
    <li><span>Celestial is now Empyrean.</span></li>
    <li><span>Druidic is now Wildsong.</span></li>
    <li><span>Gnoll is now Kholo.</span></li>
    <li><span>Ignan is now Pyric.</span></li>
    <li><span>Infernal is now Diabolic.</span></li>
    <li><span>Sylvan is now Fey.</span></li>
    <li><span>Terran is now Petran.</span></li>
    <li><span>Undercommon is now Sakvroth.</span></li>
</ul>
<h2>Class Features</h2>
<p>The largest change to class features are the removal of all legacy witch patron themes and wizard arcane schools. These choices are still available in the PF2e Legacy Data module, and when the module is enabled they will appear alongside the remaster choices for those class features automatically.</p>
<table>
    <thead>
        <tr>
            <th>Item Name</th>
            <th>Item Type</th>
            <th>Class</th>
            <th>Status</th>
            <th>New Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Divine Spellcasting</td>
            <td>Class Feature</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Cleric Spellcasting</td>
        </tr>
        <tr>
            <td>Druid Weapon Expertise</td>
            <td>Class Feature</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Weapon Expertise</td>
        </tr>
        <tr>
            <td>Druidic Language</td>
            <td>Class Feature</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Wildsong</td>
        </tr>
        <tr>
            <td>Primal Spellcasting</td>
            <td>Class Feature</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Druid Spellcasting</td>
        </tr>
        <tr>
            <td>Wild Empathy</td>
            <td>Class Feature</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Voice of Nature</td>
        </tr>
        <tr>
            <td>Wild Order</td>
            <td>Class Feature</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Untamed Order</td>
        </tr>
        <tr>
            <td>Juggernaut</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Varies by class</td>
        </tr>
        <tr>
            <td>Attack of Opportunity</td>
            <td>Ability</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Reactive Strike</td>
        </tr>
        <tr>
            <td>Alertness</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Perception Expertise</td>
        </tr>
        <tr>
            <td>Evasion</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Varies by class</td>
        </tr>
        <tr>
            <td>Great Fortitude</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Fortitude Expertise</td>
        </tr>
        <tr>
            <td>Greater Resolve</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Varies by class</td>
        </tr>
        <tr>
            <td>Improved Evasion</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Varies by class</td>
        </tr>
        <tr>
            <td>Incredible Senses</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Perception Legend</td>
        </tr>
        <tr>
            <td>Lightning Reflexes</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Reflex Expertise</td>
        </tr>
        <tr>
            <td>Resolve</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Varies by class</td>
        </tr>
        <tr>
            <td>Vigilant Senses</td>
            <td>Class Feature</td>
            <td>Multiple</td>
            <td>Renamed</td>
            <td>Perception Mastery</td>
        </tr>
        <tr>
            <td>Iron Will</td>
            <td>Class Feature</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Will Expertise</td>
        </tr>
        <tr>
            <td>Second Skin</td>
            <td>Class Feature</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Medium Armor Mastery</td>
        </tr>
        <tr>
            <td>Trackless Step</td>
            <td>Class Feature</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Trackless Journey</td>
        </tr>
        <tr>
            <td>Weapon Mastery</td>
            <td>Class Feature</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Martial Weapon Mastery</td>
        </tr>
        <tr>
            <td>Wild Stride</td>
            <td>Class Feature</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Unimpeded Journey</td>
        </tr>
        <tr>
            <td>Slippery Mind</td>
            <td>Class Feature</td>
            <td>Rogue</td>
            <td>Renamed</td>
            <td>Agile Mind</td>
        </tr>
        <tr>
            <td>Witch Patron Themes</td>
            <td>Class Feature</td>
            <td>Witch</td>
            <td>Replaced</td>
            <td>Replaced with new patron options</td>
        </tr>
        <tr>
            <td>Arcane School</td>
            <td>Class Feature</td>
            <td>Wizard</td>
            <td>Replaced</td>
            <td>Replaced with new arcane schools</td>
        </tr>
        <tr>
            <td>Arcane Spellcasting</td>
            <td>Class Feature</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Wizard Spellcasting</td>
        </tr>
        <tr>
            <td>Metamagical Experimentation</td>
            <td>Class Feature</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Experimental Spellshaping</td>
        </tr>
        <tr>
            <td>Wizard Weapon Expertise</td>
            <td>Class Feature</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Weapon Expertise</td>
        </tr>
    </tbody>
</table>

<h2>Feats</h2>
<table>
    <thead>
        <tr>
            <th>Item Name</th>
            <th>Item Type</th>
            <th>Main Trait</th>
            <th>Status</th>
            <th>New Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Abundant Step</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Merged</td>
            <td>Advanced Qi Spells</td>
          </tr>
        <tr>
            <td>Aasimar's Mercy</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Celestial Mercy</td>
        </tr>
        <tr>
            <td>Alchemical Savant</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Renamed</td>
            <td>Alchemical Assessment</td>
          </tr>
        <tr>
            <td>Advanced School Spell</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Altered mechanics</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Align Armament</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Sanctify Armament</td>
        </tr>
        <tr>
            <td>Align Ki</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Align Qi</td>
          </tr>
        <tr>
            <td>Angelic Magic</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Celestial Magic</td>
        </tr>
        <tr>
            <td>Animal Feature</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Advanced Warden</td>
        </tr>
        <tr>
            <td>Archon Magic</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Merged</td>
            <td>Celestial Magic</td>
        </tr>
        <tr>
            <td>Arrow Snatching</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Projectile Snatching</td>
          </tr>
        <tr>
            <td>Aura of Preservation</td>
            <td>Feat</td>
            <td>Champion</td>
            <td>Renamed</td>
            <td>Aura of Determination</td>
          </tr>
        <tr>
            <td>Azata Magic</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Merged</td>
            <td>Celestial Magic</td>
        </tr>
        <tr>
            <td>Bespell Weapon</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Bespell Strikes</td>
        </tr>
        <tr>
            <td>Bloodline Wellspring</td>
            <td>Feat</td>
            <td>Sorcerer</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Bone Rider</td>
            <td>Feat</td>
            <td>Lizardfolk</td>
            <td>Renamed</td>
            <td>Fossil Rider</td>
        </tr>
        <tr>
            <td>Buckler Expertise</td>
            <td>Feat</td>
            <td>Swashbuckler</td>
            <td>Renamed</td>
            <td>Elegant Buckler</td>
          </tr>
        <tr>
            <td>Burrow Elocutionist</td>
            <td>Feat</td>
            <td>Gnome</td>
            <td>Renamed</td>
            <td>Animal Elocutionist</td>
        </tr>
        <tr>
            <td>Call Bonded Item</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Call Wizardly Tools</td>
        </tr>
        <tr>
            <td>Cauldron</td>
            <td>Feat</td>
            <td>Witch</td>
            <td>Altered mechanics</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Celestial Eyes</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Nephilim Eyes</td>
        </tr>
        <tr>
            <td>Celestial Lore</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Nephilim Lore</td>
        </tr>
        <tr>
            <td>Celestial Resistance</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Nephilim Resistance</td>
        </tr>
        <tr>
            <td>Celestial Wings</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Divine Wings</td>
        </tr>
        <tr>
            <td>Celestial Word</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Divine Declaration</td>
        </tr>
        <tr>
            <td>Channeled Succor</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Restorative Channel</td>
        </tr>
        <tr>
            <td>Combat Reflexes</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Tactical Reflexes</td>
        </tr>
        <tr>
            <td>Conflux Wellspring</td>
            <td>Feat</td>
            <td>Magus</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Crossbow Ace</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Altered mechanics</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Daemon Magic</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Renamed</td>
            <td>Fiendish Magic</td>
        </tr>
        <tr>
            <td>Deepest Wellspring</td>
            <td>Feat</td>
            <td>Psychic</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Deflect Arrow</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Deflect Projectile</td>
          </tr>
        <tr>
            <td>Demon Magic</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Fiendish Magic</td>
        </tr>
        <tr>
            <td>Devil Magic</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Fiendish Magic</td>
        </tr>
        <tr>
            <td>Divine Ally</td>
            <td>Feat</td>
            <td>Champion Dedication</td>
            <td>Renamed, Altered</td>
            <td>Devout Blessing</td>
          </tr>
        <tr>
            <td>Disrupt Ki</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Disrupt Qi</td>
          </tr>
        <tr>
            <td>Diviner Sense</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Keen Magical Detection</td>
        </tr>
        <tr>
            <td>Domain Wellspring</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Dragging Strike</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Sleek Reposition</td>
        </tr>
        <tr>
            <td>Dueling Parry (Swashbuckler)</td>
            <td>Feat</td>
            <td>Swashbuckler</td>
            <td>Renamed</td>
            <td>Extravagant Parry</td>
          </tr>
        <tr>
            <td>Dwarven Weapon Cunning</td>
            <td>Feat</td>
            <td>Dwarf</td>
            <td>Merged</td>
            <td>Dwarven Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Dwarven Weapon Expertise</td>
            <td>Feat</td>
            <td>Dwarf</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Eldritch Nails</td>
            <td>Feat</td>
            <td>Witch</td>
            <td>Renamed</td>
            <td>Witch's Armaments</td>
        </tr>
        <tr>
            <td>Empty Body, Ki Form</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Grandmaster Qi Spells</td>
          </tr>
        <tr>
            <td>Elven Weapon Elegance</td>
            <td>Feat</td>
            <td>Elf</td>
            <td>Merged</td>
            <td>Elven Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Elven Weapon Expertise</td>
            <td>Feat</td>
            <td>Elf</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Empyreal Blessing</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Extraplanar Supplication</td>
        </tr>
        <tr>
            <td>Elastic Mutagen</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Merged</td>
            <td>Mutant Physique</td>
        <tr>
            <tr>
                <td>Enchanting Arrow</td>
                <td>Feat</td>
                <td>Eldritch Archer</td>
                <td>Renamed</td>
                <td>Enchanting Shot</td>
              </tr>
        <tr>
            <td>Ephemeral Tracking</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Masterful Warden</td>
        </tr>
        <tr>
            <td>Eschew Materials</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Expert Alchemy</td>
            <td>Feat</td>
            <td>Alchemist Dedication</td>
            <td>Removed</td>
            <td>—</td>
          </tr>
          <tr>
            <td>Expert Disassembler</td>
            <td>Feat</td>
            <td>Scrounger Dedication</td>
            <td>Renamed</td>
            <td>Expert Disassembly</td>
          </tr>
          <tr>
            <td>Expert Herbalism</td>
            <td>Feat</td>
            <td>Herbalist Dedication</td>
            <td>Removed</td>
            <td>—</td>
          </tr>
          <tr>
            <td>Expert Poisoner</td>
            <td>Feat</td>
            <td>Poisoner Dedication</td>
            <td>Removed</td>
            <td>—</td>
          </tr>
        <tr>
            <td>Extend Armament Alignment</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Lasting Armament</td>
        </tr>
        <tr>
            <td>Eyes of the Night</td>
            <td>Feat</td>
            <td>Dhampir</td>
            <td>Renamed</td>
            <td>Eyes of Night</td>
          </tr>
        <tr>
            <td>Favored Enemy</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Favored Prey</td>
        </tr>
        <tr>
            <td>Feral Mutagen</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Merged</td>
            <td>Mutant Physique</td>
        <tr>
            <td>Fiend's Door</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Renamed</td>
            <td>Slip Sideways</td>
        </tr>
        <tr>
            <td>Fiendish Eyes</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Nephilim Eyes</td>
        </tr>
        <tr>
            <td>Fiendish Lore</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Nephilim Lore</td>
        </tr>
        <tr>
            <td>Fiendish Resistance</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Nephilim Resistance</td>
        </tr>
        <tr>
            <td>Fiendish Wings</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Divine Wings</td>
        </tr>
        <tr>
            <td>Fiendish Word</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Divine Declaration</td>
        </tr>
        <tr>
            <td>Form of the Fiend</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Renamed</td>
            <td>Bestial Manifestation</td>
        </tr>
        <tr>
            <td>Genius Mutagen</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Merged</td>
            <td>Mutant Innervation</td>
          </tr>
        <tr>
            <td>Glib Mutagen</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Merged</td>
            <td>Mutant Innervation</td>
          </tr>
        <tr>
            <td>Gnome Weapon Expertise</td>
            <td>Feat</td>
            <td>Gnome</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Gnome Weapon Innovator</td>
            <td>Feat</td>
            <td>Gnome</td>
            <td>Merged</td>
            <td>Gnome Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Gnoll Lore</td>
            <td>Feat</td>
            <td>Gnoll</td>
            <td>Renamed</td>
            <td>Kholo Lore</td>
        </tr>
        <tr>
            <td>Gnoll Weapon Familiarity</td>
            <td>Feat</td>
            <td>Gnoll</td>
            <td>Renamed</td>
            <td>Kholo Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Goblin Weapon Expertise</td>
            <td>Feat</td>
            <td>Goblin</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Goblin Weapon Frenzy</td>
            <td>Feat</td>
            <td>Goblin</td>
            <td>Merged</td>
            <td>Goblin Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Gravity Weapon</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Initiate Warden</td>
        </tr>
        <tr>
            <td>Green Empathy</td>
            <td>Feat</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Plant Empathy</td>
        </tr>
        <tr>
            <td>Grippli Glide</td>
            <td>Feat</td>
            <td>Grippli</td>
            <td>Renamed</td>
            <td>Tripkee Glide</td>
          </tr>
          <tr>
            <td>Grippli Lore</td>
            <td>Feat</td>
            <td>Grippli</td>
            <td>Renamed</td>
            <td>Trpikee Lore</td>
          </tr>
          <tr>
            <td>Grippli Weapon Familiarity</td>
            <td>Feat</td>
            <td>Grippli</td>
            <td>Renamed</td>
            <td>Tripkee Weapon Familiarity</td>
          </tr>
        <tr>
            <td>Halfling Weapon Expertise</td>
            <td>Feat</td>
            <td>Halfling</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Halfling Weapon Trickster</td>
            <td>Feat</td>
            <td>Halfling</td>
            <td>Merged</td>
            <td>Halfling Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Hand of the Apprentice</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Hatchling Flight</td>
            <td>Feat</td>
            <td>Kobold</td>
            <td>Rename</td>
            <td>Winglet Flight</td>
          </tr>
          <tr>
            <td>Healing Touch</td>
            <td>Feat</td>
            <td>Champion Dedication</td>
            <td>Renamed, Altered</td>
            <td>Devout Magic</td>
          </tr>
        <tr>
            <td>Hex Wellspring</td>
            <td>Feat</td>
            <td>Witch</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Hold Mark</td>
            <td>Feat</td>
            <td>Orc</td>
            <td>Altered mechanics</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Holy Castigation</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Divine Castigation</td>
        </tr>
        <tr>
            <td>Hunter's Luck</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Merged</td>
            <td>Advanced Warden</td>
        </tr>
        <tr>
            <td>Hunter's Vision</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Peerless Warden</td>
        </tr>
        <tr>
            <td>Improved Knockdown</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Crashing Slam</td>
        </tr>
        <tr>
            <td>Inspirational Performance</td>
            <td>Feat</td>
            <td>Archetype Bard</td>
            <td>Renamed</td>
            <td>Anthemic Performance</td>
        </tr>
        <tr>
            <td>Inspire Competence</td>
            <td>Feat</td>
            <td>Bard</td>
            <td>Renamed</td>
            <td>Uplifting Overture</td>
        </tr>
        <tr>
            <td>Inspire Defense</td>
            <td>Feat</td>
            <td>Bard</td>
            <td>Renamed</td>
            <td>Rallying Anthem</td>
        </tr>
        <tr>
            <td>Inspire Heroics</td>
            <td>Feat</td>
            <td>Bard</td>
            <td>Renamed</td>
            <td>Fortissimo Composition</td>
        </tr>
        <tr>
            <td>Invincible Mutagen</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Merged</td>
            <td>Mutant Physique</td>
        <tr>
        <tr>
            <td>Knockdown</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Slam Down</td>
        </tr>
        <tr>
            <td>Ki Blast</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Advanced Qi Spells</td>
          </tr>
          <tr>
            <td>Ki Center</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Qi Center</td>
          </tr>
          <tr>
            <td>Ki Rush</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Qi Spells</td>
          </tr>
          <tr>
            <td>Ki Rush</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Merged</td>
            <td>Qi Spells</td>
          </tr>
        <tr>
            <td>Leech-Clipper</td>
            <td>Feat</td>
            <td>Hobgoblin</td>
            <td>Renamed</td>
            <td>Leech-Clip</td>
          </tr>
        <tr>
            <td>Link Wellspring</td>
            <td>Feat</td>
            <td>Summoner</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Living Hair</td>
            <td>Feat</td>
            <td>Witch</td>
            <td>Merged</td>
            <td>Witch's Armaments</td>
        </tr>
        <tr>
            <td>Magic Arrow</td>
            <td>Feat</td>
            <td>Eldritch Archer</td>
            <td>Renamed</td>
            <td>Magic Ammunition</td>
          </tr>
        <tr>
            <td>Malicious Bane</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Extraplanar Supplication</td>
        </tr>
        <tr>
            <td>Master Alchemy</td>
            <td>Feat</td>
            <td>Alchemist Dedication</td>
            <td>Removed</td>
            <td>—</td>
          </tr>
        <tr>
            <td>Meditative Wellspring</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Medusa's Wrath</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Merged</td>
            <td>Master Qi Spells</td>
          </tr>
        <tr>
            <td>Metamagic Channel</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Spellshape Channel</td>
        </tr>
        <tr>
            <td>Mindblank Mutagen</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Renamed</td>
            <td>Mutant Innervation</td>
          </tr>
        <tr>
            <td>Necrotic Infusion</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Merged</td>
            <td>Divine Infusion</td>
        </tr>
        <tr>
            <td>Nimble Dodge</td>
            <td>Feat</td>
            <td>Swashbuckler</td>
            <td>Renamed</td>
            <td>Flashy Dodge</td>
          </tr>
          <tr>
            <td>Nimble Roll</td>
            <td>Feat</td>
            <td>Swashbuckler</td>
            <td>Renamed</td>
            <td>Flashy Roll</td>
          </tr>
        <tr>
            <td>Nocturnal Grippli</td>
            <td>Feat</td>
            <td>Grippli</td>
            <td>Renamed</td>
            <td>Nocturnal Tripkee</td>
          </tr>
          <tr>
            <td>Nocturnal Sense</td>
            <td>Feat</td>
            <td>Barbarian</td>
            <td>Renamed</td>
            <td>Nocturnal Senses</td>
          </tr>
        <tr>
            <td>Opportunist</td>
            <td>Feat</td>
            <td>Archetype Fighter</td>
            <td>Renamed</td>
            <td>Reactive Striker</td>
        </tr>
        <tr>
            <td>Orc Weapon Carnage</td>
            <td>Feat</td>
            <td>Orc</td>
            <td>Merged</td>
            <td>Orc Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Orc Weapon Expertise</td>
            <td>Feat</td>
            <td>Orc</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Pirate Weapon Training</td>
            <td>Feat</td>
            <td>Pirate Dedication</td>
            <td>Renamed, Altered</td>
            <td>Pirate Combat Training</td>
        </tr>
        <tr>
            <td>Point-Blank Shot</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Point Blank Stance</td>
        </tr>
        <tr>
            <td>Power Attack</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Vicious Swing</td>
        </tr>
        <tr>
            <td>Precious Arrow</td>
            <td>Feat</td>
            <td>Eldritch Archer</td>
            <td>Renamed</td>
            <td>Precious Ammunition</td>
          </tr>
        <tr>
            <td>Primal Wellspring</td>
            <td>Feat</td>
            <td>Druid</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Quick Alchemy</td>
            <td>Feat</td>
            <td>Alchemist Dedication</td>
            <td>Renamed, Altered</td>
            <td>Advanced Alchemy</td>
          </tr>
        <tr>
            <td>Quivering Palm</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Merged</td>
            <td>Master Qi Spells</td>
          </tr>
          <tr>
            <td>Radiant Blade Master</td>
            <td>Feat</td>
            <td>Champion</td>
            <td>Renamed</td>
            <td>Armament Paragon</td>
          </tr>
          <tr>
            <td>Radiant Blade Spirit</td>
            <td>Feat</td>
            <td>Champion</td>
            <td>Renamed</td>
            <td>Radiant Armament</td>
          <tr>
            <td>Radiant Infusion</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Divine Infusion</td>
        </tr>
        <tr>
            <td>Ranged Reprisal</td>
            <td>Feat</td>
            <td>Champion</td>
            <td>Renamed</td>
            <td>Nimble Reprisal</td>
          </tr>
        <tr>
            <td>Razor Claws</td>
            <td>Feat</td>
            <td>Lizardfolk</td>
            <td>Merged</td>
            <td>Iruxi Armaments</td>
        </tr>
        <tr>
            <td>Ranger's Bramble</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Merged</td>
            <td>Masterful Warden</td>
        </tr>
        <tr>
            <td>Ratfolk Growth</td>
            <td>Feat</td>
            <td>Ratfolk</td>
            <td>Renamed</td>
            <td>Greater Than the Sum</td>
        </tr>
        <tr>
            <td>Red Herring</td>
            <td>Feat</td>
            <td>Investigator</td>
            <td>Renamed</td>
            <td>Eliminate Red Herrings</td>
          </tr>
        <tr>
            <td>Reckless Abandon (Barbarian)</td>
            <td>Feat</td>
            <td>Barbarian</td>
            <td>Renamed</td>
            <td>Desperate Wrath</td>
          </tr>
        <tr>
            <td>Relentless Wings</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Eternal Wings (Nephilim)</td>
        </tr>
        <tr>
            <td>Safeguarded Spell</td>
            <td>Feat</td>
            <td>Sorcerer</td>
            <td>Renamed</td>
            <td>Safeguard Spell</td>
          </tr>
        <tr>
            <td>Sacred Ki</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Sharp Fangs</td>
            <td>Feat</td>
            <td>Lizardfolk</td>
            <td>Merged</td>
            <td>Iruxi Armaments</td>
        </tr>
        <tr>
            <td>Shared Luck (Catfolk)</td>
            <td>Feat</td>
            <td>Catfolk</td>
            <td>Renamed</td>
            <td>Luck of the Clowder</td>
          </tr>
        <tr>
            <td>Scroll Savant</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Scroll Adept</td>
        </tr>
        <tr>
            <td>Second Ally</td>
            <td>Feat</td>
            <td>Champion</td>
            <td>Renamed</td>
            <td>Second Blessing</td>
          </tr>
        <tr>
            <td>Second Chance Spell</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Second Thoughts</td>
        </tr>
        <tr>
            <td>Seeker Arrow</td>
            <td>Feat</td>
            <td>Eldritch Archer</td>
            <td>Renamed</td>
            <td>Homing Shot</td>
        </tr>
        <tr>
            <td>Silent Spell</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Merged</td>
            <td>Conceal Spell</td>
        </tr>
        <tr>
            <td>Smite Evil</td>
            <td>Feat</td>
            <td>Champion</td>
            <td>Merged</td>
            <td>Smite</td>
          </tr>
          <tr>
            <td>Smite Good</td>
            <td>Feat</td>
            <td>Champion</td>
            <td>Merged</td>
            <td>Smite</td>
          </tr>
        <tr>
            <td>Sneak Savant</td>
            <td>Feat</td>
            <td>Rogue</td>
            <td>Renamed</td>
            <td>Sneak Adept</td>
        </tr>
        <tr>
            <td>Soothing Mist</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Merged</td>
            <td>Advanced Warden</td>
        </tr>
        <tr>
            <td>Spellbook Prodigy</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Altered mechanics</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Spell Penetration</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Renamed</td>
            <td>Irresistible Magic</td>
        </tr>
        <tr>
            <td>Spirit Strikes</td>
            <td>Feat</td>
            <td>Duskwalker</td>
            <td>Rename</td>
            <td>Quietus Strikes</td>
          </tr>
        <tr>
            <td>Spring Attack</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Dashing Strike</td>
        </tr>
        <tr>
            <td>Stance Savant</td>
            <td>Feat</td>
            <td>Fighter</td>
            <td>Renamed</td>
            <td>Opening Stance</td>
        </tr>
        <tr>
            <td>Stance Savant</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Reflexive Stance</td>
          </tr>
        <tr>
            <td>Stonecunning</td>
            <td>Feat</td>
            <td>Dwarf</td>
            <td>Renamed</td>
            <td>Stonemason's Eye</td>
        </tr>
        <tr>
            <td>Stunning Fist</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Stunning Blows</td>
          </tr>
          <tr>
            <td>Subtle Delivery</td>
            <td>Feat</td>
            <td>Alchemist</td>
            <td>Renamed</td>
            <td>Blowgun Poisoner</td>
          </tr>
          <tr>
            <td>Summon Celestial Kin</td>
            <td>Feat</td>
            <td>Aasimar</td>
            <td>Renamed</td>
            <td>Summon Nephilim Kin</td>
        </tr>
        <tr>
            <td>Summon Fiendish Kin</td>
            <td>Feat</td>
            <td>Tiefling</td>
            <td>Merged</td>
            <td>Summon Nephilim Kin</td>
        </tr>
        <tr>
            <td>Tail Whip</td>
            <td>Feat</td>
            <td>Lizardfolk</td>
            <td>Merged</td>
            <td>Iruxi Armaments</td>
        </tr>
        <tr>
            <td>Temporary Potions</td>
            <td>Feat</td>
            <td>Witch</td>
            <td>Merged</td>
            <td>Cauldron</td>
        </tr>
        <tr>
            <td>Terrain Transposition</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Merged</td>
            <td>Peerless Warden</td>
        </tr>
        <tr>
            <td>Thousand Faces</td>
            <td>Feat</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Anthropomorphic Shape</td>
        </tr>
        <tr>
            <td>Turn Undead</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Renamed</td>
            <td>Panic the Dead</td>
        </tr>
        <tr>
            <td>Unconventional Expertise</td>
            <td>Feat</td>
            <td>Human</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Universal Versatility</td>
            <td>Feat</td>
            <td>Wizard</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Vanth's Weapon Familiarity</td>
            <td>Feat</td>
            <td>Duskwalker</td>
            <td>Renamed</td>
            <td>Duskwalker Weapon Familiarity</td>
        </tr>
        <tr>
            <td>Vengeful Hatred</td>
            <td>Feat</td>
            <td>Dwarf</td>
            <td>Renamed</td>
            <td>Mountain Strategy</td>
        </tr>
        <tr>
            <td>Vigorous Inspiration</td>
            <td>Feat</td>
            <td>Bard</td>
            <td>Renamed</td>
            <td>Vigorous Anthem</td>
        </tr>
        <tr>
            <td>Vile Desecration</td>
            <td>Feat</td>
            <td>Cleric</td>
            <td>Merged</td>
            <td>Divine Castigation</td>
        </tr>
        <tr>
            <td>Warden's Wellspring</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Warding Rune</td>
            <td>Feat</td>
            <td>Archetype</td>
            <td>Removed</td>
            <td>—</td>
        </tr>
        <tr>
            <td>Wholeness of Body</td>
            <td>Feat</td>
            <td>Monk</td>
            <td>Renamed</td>
            <td>Harmonize Self</td>
          </tr>
        <tr>
            <td>Wild Empathy</td>
            <td>Feat</td>
            <td>Ranger</td>
            <td>Renamed</td>
            <td>Animal Empathy (Ranger)</td>
        </tr>
        <tr>
            <td>Wild Shape</td>
            <td>Feat</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Untamed Form</td>
        </tr>
        <tr>
            <td>Woodland Stride</td>
            <td>Feat</td>
            <td>Druid</td>
            <td>Renamed</td>
            <td>Forest Passage</td>
        </tr>
    </tbody>
</table>

<h2>Spells</h2>

<table>
    <thead>
        <tr>
            <th>Spell Name</th>
            <th>Status</th>
            <th>New Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Abundant Step</td>
            <td>Renamed</td>
            <td>Shrink the Span</td>
          </tr>
          <tr>
            <td>Abyssal Wrath</td>
            <td>Renamed</td>
            <td>Chthonian Wrath</td>
          </tr>
          <tr>
            <td>Animate Dead</td>
            <td>Renamed</td>
            <td>summon undead</td>
          </tr>
          <tr>
            <td>Augment Summoning</td>
            <td>Renamed</td>
            <td>Fortify Summoning</td>
          </tr>
          <tr>
            <td>Baleful Polymorph</td>
            <td>Renamed</td>
            <td>cursed metamorphosis</td>
          </tr>
          <tr>
            <td>Barkskin</td>
            <td>Renamed</td>
            <td>oaken resilience</td>
          </tr>
          <tr>
            <td>Bind Soul</td>
            <td>Renamed</td>
            <td>seize soul</td>
          </tr>
          <tr>
            <td>Blind Ambition</td>
            <td>Renamed</td>
            <td>Ignite Ambition</td>
          </tr>
          <tr>
            <td>Blink</td>
            <td>Renamed</td>
            <td>Flicker</td>
          </tr>
          <tr>
            <td>Burning Hands</td>
            <td>Renamed</td>
            <td>breathe fire</td>
          </tr>
          <tr>
            <td>Calm Emotions</td>
            <td>Renamed</td>
            <td>calm</td>
          </tr>
          <tr>
            <td>Charming Words</td>
            <td>Renamed</td>
            <td>Charming Push</td>
          </tr>
          <tr>
            <td>Chill Touch</td>
            <td>Renamed</td>
            <td>Void Warp</td>
          </tr>
          <tr>
            <td>Cloudkill</td>
            <td>Renamed</td>
            <td>toxic cloud</td>
          </tr>
          <tr>
            <td>Color Spray</td>
            <td>Renamed</td>
            <td>dizzying colors</td>
          </tr>
          <tr>
            <td>Commune with Nature</td>
            <td>Merged</td>
            <td>Commune</td>
          </tr>
          <tr>
            <td>Comprehend Language</td>
            <td>Renamed</td>
            <td>translate</td>
          </tr>
          <tr>
            <td>Continual Flame</td>
            <td>Renamed</td>
            <td>everlight</td>
          </tr>
          <tr>
            <td>Crushing Despair</td>
            <td>Renamed</td>
            <td>wave of despair</td>
          </tr>
          <tr>
            <td>Dancing Lights</td>
            <td>Merged</td>
            <td>Light</td>
          </tr>
          <tr>
            <td>Dimension Door</td>
            <td>Renamed</td>
            <td>translocate</td>
          </tr>
          <tr>
            <td>Dimensional Anchor</td>
            <td>Renamed</td>
            <td>planar seal</td>
          </tr>
          <tr>
            <td>Dimensional Lock</td>
            <td>Renamed</td>
            <td>Planar Tether</td>
          </tr>
          <tr>
            <td>Discern Location</td>
            <td>Renamed</td>
            <td>pinpoint</td>
          </tr>
          <tr>
            <td>Disrupt Undead</td>
            <td>Renamed</td>
            <td>Vitality Lash</td>
          </tr>
          <tr>
            <td>Disrupting Weapons</td>
            <td>Renamed</td>
            <td>Infuse Vitality</td>
          </tr>
          <tr>
            <td>Dragon Claws</td>
            <td>Renamed</td>
            <td>Flurry of Claws</td>
          </tr>
          <tr>
            <td>Empty Body</td>
            <td>Renamed</td>
            <td>Embrace Nothingness</td>
          </tr>
          <tr>
            <td>Endure Elements</td>
            <td>Renamed</td>
            <td>environmental endurance</td>
          </tr>
          <tr>
            <td>Entangle</td>
            <td>Renamed</td>
            <td>entangling flora</td>
          </tr>
          <tr>
            <td>False Life</td>
            <td>Renamed</td>
            <td>false vitality</td>
          </tr>
          <tr>
            <td>Feather Fall</td>
            <td>Renamed</td>
            <td>gentle landing</td>
          </tr>
          <tr>
            <td>Feeblemind</td>
            <td>Renamed</td>
            <td>Never Mind</td>
          </tr>
          <tr>
            <td>Finger of Death</td>
            <td>Renamed</td>
            <td>execute</td>
          </tr>
          <tr>
            <td>Flaming Sphere</td>
            <td>Renamed</td>
            <td>floating flame</td>
          </tr>
          <tr>
            <td>Flesh To Stone</td>
            <td>Renamed</td>
            <td>petrify</td>
          </tr>
          <tr>
            <td>Floating Disk</td>
            <td>Renamed</td>
            <td>Carryall</td>
          </tr>
          <tr>
            <td>Force Cage</td>
            <td>Renamed</td>
            <td>Lifewood Cage</td>
          </tr>
          <tr>
            <td>Freedom of Movement</td>
            <td>Renamed</td>
            <td>unfettered movement</td>
          </tr>
          <tr>
            <td>Gaseous Form</td>
            <td>Renamed</td>
            <td>vapor form</td>
          </tr>
          <tr>
            <td>Gentle Repose</td>
            <td>Renamed</td>
            <td>peaceful rest</td>
          </tr>
          <tr>
            <td>Glibness</td>
            <td>Renamed</td>
            <td>honeyed words</td>
          </tr>
          <tr>
            <td>Glitterdust</td>
            <td>Merged</td>
            <td>revealing light</td>
          </tr>
          <tr>
            <td>Globe of Invulnerability</td>
            <td>Renamed</td>
            <td>Dispelling Globe</td>
          </tr>
          <tr>
            <td>Glutton's Jaw</td>
            <td>Renamed</td>
            <td>Glutton's Jaws</td>
          </tr>
          <tr>
            <td>Hallucinatory Terrain</td>
            <td>Renamed</td>
            <td>mirage</td>
          </tr>
          <tr>
            <td>Heroes' Feast</td>
            <td>Renamed</td>
            <td>Fortifying Brew</td>
          </tr>
          <tr>
            <td>Hideous Laughter</td>
            <td>Renamed</td>
            <td>laughing fit</td>
          </tr>
          <tr>
            <td>Horrid Wilting</td>
            <td>Renamed</td>
            <td>desiccate</td>
          </tr>
          <tr>
            <td>Hypnotic Pattern</td>
            <td>Renamed</td>
            <td>hypnotize</td>
          </tr>
          <tr>
            <td>Inspire Competence</td>
            <td>Renamed</td>
            <td>uplifting overture</td>
          </tr>
          <tr>
            <td>Inspire Courage</td>
            <td>Renamed</td>
            <td>courageous anthem</td>
          </tr>
          <tr>
            <td>Inspire Defense</td>
            <td>Renamed</td>
            <td>rallying anthem</td>
          </tr>
          <tr>
            <td>Inspire Heroics</td>
            <td>Renamed</td>
            <td>fortissimo composition</td>
          </tr>
          <tr>
            <td>Invisibility Sphere</td>
            <td>Renamed</td>
            <td>Shared Invisibility</td>
          </tr>
          <tr>
            <td>Ki Blast</td>
            <td>Renamed</td>
            <td>Qi Blast</td>
          </tr>
          <tr>
            <td>Ki Form</td>
            <td>Renamed</td>
            <td>Qi Form</td>
          </tr>
          <tr>
            <td>Ki Rush</td>
            <td>Renamed</td>
            <td>Qi Rush</td>
          </tr>
          <tr>
            <td>Ki Strike</td>
            <td>Renamed</td>
            <td>Inner Upheaval</td>
          </tr>
          <tr>
            <td>Know Direction</td>
            <td>Renamed</td>
            <td>know the way</td>
          </tr>
          <tr>
            <td>Legend Lore</td>
            <td>Renamed</td>
            <td>collective memories</td>
          </tr>
          <tr>
            <td>Longstrider</td>
            <td>Renamed</td>
            <td>tailwind</td>
          </tr>
          <tr>
            <td>Mage Armor</td>
            <td>Renamed</td>
            <td>mystic armor</td>
          </tr>
          <tr>
            <td>Mage Hand</td>
            <td>Renamed</td>
            <td>telekinetic hand</td>
          </tr>
          <tr>
            <td>Magic Aura</td>
            <td>Renamed</td>
            <td>disguise magic</td>
          </tr>
          <tr>
            <td>Magic Fang</td>
            <td>Renamed</td>
            <td>runic body</td>
          </tr>
          <tr>
            <td>Magic Missile</td>
            <td>Renamed</td>
            <td>force barrage</td>
          </tr>
          <tr>
            <td>Magic Mouth</td>
            <td>Renamed</td>
            <td>embed message</td>
          </tr>
          <tr>
            <td>Magic Weapon</td>
            <td>Renamed</td>
            <td>runic weapon</td>
          </tr>
          <tr>
            <td>Magnificent Mansion</td>
            <td>Renamed</td>
            <td>planar palace</td>
          </tr>
          <tr>
            <td>Maze</td>
            <td>Renamed</td>
            <td>quandary</td>
          </tr>
          <tr>
            <td>Meld into Stone</td>
            <td>Renamed</td>
            <td>one with stone</td>
          </tr>
          <tr>
            <td>Meteor Swarm</td>
            <td>Renamed</td>
            <td>falling stars</td>
          </tr>
          <tr>
            <td>Mind Blank</td>
            <td>Renamed</td>
            <td>Hidden Mind</td>
          </tr>
          <tr>
            <td>Misdirection</td>
            <td>Renamed</td>
            <td>disguise magic</td>
          </tr>
          <tr>
            <td>Modify Memory</td>
            <td>Renamed</td>
            <td>rewrite memory</td>
          </tr>
          <tr>
            <td>Neutralize Poison</td>
            <td>Renamed</td>
            <td>cleanse affliction</td>
          </tr>
          <tr>
            <td>Nondetection</td>
            <td>Renamed</td>
            <td>veil of privacy</td>
          </tr>
          <tr>
            <td>Obscuring Mist</td>
            <td>Renamed</td>
            <td>mist</td>
          </tr>
          <tr>
            <td>Pass Without Trace</td>
            <td>Renamed</td>
            <td>Vanishing Tracks</td>
          </tr>
          <tr>
            <td>Passwall</td>
            <td>Renamed</td>
            <td>Magic Passage</td>
          </tr>
          <tr>
            <td>Phantom Mount</td>
            <td>Renamed</td>
            <td>Marvelous mount</td>
          </tr>
          <tr>
            <td>Planar Binding</td>
            <td>Renamed</td>
            <td>planar servitor</td>
          </tr>
          <tr>
            <td>Plane Shift</td>
            <td>Renamed</td>
            <td>interplanar teleport</td>
          </tr>
          <tr>
            <td>Positive Luminance</td>
            <td>Renamed</td>
            <td>Vital Luminance</td>
          </tr>
          <tr>
            <td>Private Sanctum</td>
            <td>Renamed</td>
            <td>peaceful bubble</td>
          </tr>
          <tr>
            <td>Prying Eye</td>
            <td>Renamed</td>
            <td>scouting eye</td>
          </tr>
          <tr>
            <td>Pulse of The City</td>
            <td>Renamed</td>
            <td>Pulse of Civilization</td>
          </tr>
          <tr>
            <td>Purify Food And Drink</td>
            <td>Renamed</td>
            <td>cleanse cuisine</td>
          </tr>
          <tr>
            <td>Quivering Palm</td>
            <td>Renamed</td>
            <td>Touch of Death</td>
          </tr>
          <tr>
            <td>Ray of Enfeeblement</td>
            <td>Renamed</td>
            <td>Enfeeble</td>
          </tr>
          <tr>
            <td>Remove Curse</td>
            <td>Renamed</td>
            <td>cleanse affliction</td>
          </tr>
          <tr>
            <td>Remove Disease</td>
            <td>Renamed</td>
            <td>cleanse affliction</td>
          </tr>
          <tr>
            <td>Remove Fear</td>
            <td>Renamed</td>
            <td>clear mind</td>
          </tr>
          <tr>
            <td>Remove Paralysis</td>
            <td>Renamed</td>
            <td>sure footing</td>
          </tr>
          <tr>
            <td>Restore Senses</td>
            <td>Renamed</td>
            <td>Sound Body</td>
          </tr>
          <tr>
            <td>Righteous Might</td>
            <td>Renamed</td>
            <td>Sacred Form</td>
          </tr>
          <tr>
            <td>Sanctified Ground</td>
            <td>Renamed</td>
            <td>Anointed Ground</td>
          </tr>
          <tr>
            <td>Scintillating Pattern</td>
            <td>Renamed</td>
            <td>Confusing Colors</td>
          </tr>
          <tr>
            <td>Scorching Ray</td>
            <td>Renamed</td>
            <td>Blazing Bolt</td>
          </tr>
          <tr>
            <td>Searing Light</td>
            <td>Renamed</td>
            <td>holy light</td>
          </tr>
          <tr>
            <td>See Invisibility</td>
            <td>Renamed</td>
            <td>see the unseen</td>
          </tr>
          <tr>
            <td>Shadow Walk</td>
            <td>Renamed</td>
            <td>umbral journey</td>
          </tr>
          <tr>
            <td>Shapechange</td>
            <td>Renamed</td>
            <td>Metamorphosis</td>
          </tr>
          <tr>
            <td>Shield Other</td>
            <td>Renamed</td>
            <td>share life</td>
          </tr>
          <tr>
            <td>Simulacrum</td>
            <td>Renamed</td>
            <td>Shadow Double</td>
          </tr>
          <tr>
            <td>Sound Burst</td>
            <td>Renamed</td>
            <td>Noise Blast</td>
          </tr>
          <tr>
            <td>Spectral Hand</td>
            <td>Renamed</td>
            <td>Ghostly Carrier</td>
          </tr>
          <tr>
            <td>Spider Climb</td>
            <td>Renamed</td>
            <td>Gecko Grip</td>
          </tr>
          <tr>
            <td>Splash of Art</td>
            <td>Renamed</td>
            <td>Creative Splash</td>
          </tr>
          <tr>
            <td>Stone Tell</td>
            <td>Renamed</td>
            <td>Speak with Stones</td>
          </tr>
          <tr>
            <td>Stoneskin</td>
            <td>Renamed</td>
            <td>Mountain Resilience</td>
          </tr>
          <tr>
            <td>Tanglefoot</td>
            <td>Renamed</td>
            <td>Tangle Vine</td>
          </tr>
          <tr>
            <td>Time Stop</td>
            <td>Renamed</td>
            <td>Freeze Time</td>
          </tr>
          <tr>
            <td>Tongues</td>
            <td>Renamed</td>
            <td>truespeech</td>
          </tr>
          <tr>
            <td>Touch of Idiocy</td>
            <td>Renamed</td>
            <td>stupefy</td>
          </tr>
          <tr>
            <td>Tree Shape</td>
            <td>Renamed</td>
            <td>one with plants</td>
          </tr>
          <tr>
            <td>Tree Stride</td>
            <td>Renamed</td>
            <td>nature's pathway</td>
          </tr>
          <tr>
            <td>True Strike</td>
            <td>Renamed</td>
            <td>sure strike</td>
          </tr>
          <tr>
            <td>Trueseeing</td>
            <td>Renamed</td>
            <td>truesight</td>
          </tr>
          <tr>
            <td>Unseen Custodians</td>
            <td>Renamed</td>
            <td>Phantasmal Custodians</td>
          </tr>
          <tr>
            <td>Unseen Servant</td>
            <td>Renamed</td>
            <td>phantasmal minion</td>
          </tr>
          <tr>
            <td>Vampiric Touch</td>
            <td>Renamed</td>
            <td>vampiric feast</td>
          </tr>
          <tr>
            <td>Veil</td>
            <td>Merged</td>
            <td>Illusory Disguise</td>
          </tr>
          <tr>
            <td>Vigilant Eye</td>
            <td>Renamed</td>
            <td>Rune of Observation</td>
          </tr>
          <tr>
            <td>Wail of the Banshee</td>
            <td>Renamed</td>
            <td>Wails of the Damned</td>
          </tr>
          <tr>
            <td>Wholeness of Body</td>
            <td>Renamed</td>
            <td>Harmonize Self</td>
          </tr>
          <tr>
            <td>Wind Walk</td>
            <td>Renamed</td>
            <td>Migration</td>
          </tr>
          <tr>
            <td>Word of Recall</td>
            <td>Renamed</td>
            <td>Gathering Call</td>
          </tr>
          <tr>
            <td>Zone of Truth</td>
            <td>Renamed</td>
            <td>Ring of Truth</td>
          </tr>
    </tbody>
</table>

<h2>Equipment</h2>
<p>Most equipment changes are direct renames. The only set of removed items were alignment ampoules, as they are not compatible with the removal of alignment from the game.</p>
<table>
    <thead>
        <tr>
            <th>Old Name</th>
            <th>New Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Aeon Stone (Clear Spindle)</td>
            <td>Aeon Stone (Nourishing)</td>
          </tr>
          <tr>
            <td>Aeon Stone (Dull Grey)</td>
            <td>Aeon Stone (consumed)</td>
          </tr>
          <tr>
            <td>Aeon Stone (Orange Prism)</td>
            <td>Aeon Stone (Amplifying)</td>
          </tr>
          <tr>
            <td>Aeon Stone (Tourmaline Sphere)</td>
            <td>Aeon Stone (Delaying)</td>
          </tr>
          <tr>
            <td>Bag of Holding</td>
            <td>Spacious Pouch</td>
          </tr>
          <tr>
            <td>Barkskin Potion</td>
            <td>Oak Potion</td>
          </tr>
          <tr>
            <td>Boots of Speed</td>
            <td>Propulsive Boots</td>
          </tr>
          <tr>
            <td>Bracers of Armor</td>
            <td>bands of force</td>
          </tr>
          <tr>
            <td>Breastplate of Command</td>
            <td>Warleader's Bulwark</td>
          </tr>
          <tr>
            <td>Broom of Flying</td>
            <td>Flying Broomstick</td>
          </tr>
          <tr>
            <td>Celestial Armor</td>
            <td>Holy Chain</td>
          </tr>
        <tr>
          <td>Dancing Rune</td>
          <td>Animated Rune</td>
      </tr>
          <tr>
            <td>Dagger of Venom</td>
            <td>Serpent Dagger</td>
          </tr>
          <tr>
            <td>Demon Armor</td>
            <td>Unholy Plate</td>
          </tr>
          <tr>
            <td>Dragon's Breath potion (various)</td>
            <td>Energy Breath Potion (various)</td>
          </tr>
          <tr>
            <td>Druid's Vestments</td>
            <td>Living Mantle</td>
          </tr>
          <tr>
            <td>Everburning Torch</td>
            <td>Everlight Crystal</td>
          </tr>
          <tr>
            <td>Eyes of the Eagle</td>
            <td>Eyes of the Cat</td>
          </tr>
          <tr>
            <td>Feather Token (Chest, Ladder, Swan Boat)</td>
            <td>Marvelous Miniatures (Chest, Ladder, Boat)</td>
          </tr>
          <tr>
            <td>Flame Tongue</td>
            <td>Searing Blade</td>
          </tr>
          <tr>
            <td>Gloves of Storing</td>
            <td>Retrieval Belt</td>
          </tr>
          <tr>
            <td>Hat of Disguise</td>
            <td>Masquerade Scarf</td>
          </tr>
          <tr>
            <td>Hat of the Magi</td>
            <td>Mage's Hat</td>
        </tr>
          <tr>
            <td>Holy Avenger</td>
            <td>Chalice of Justice</td>
          </tr>
          <tr>
            <td>Horn of Fog</td>
            <td>Cloud Pouch</td>
          </tr>
          <tr>
            <td>Horseshoes of Speed</td>
            <td>Alacritous Horseshoes</td>
          </tr>
          <tr>
            <td>Javelin of Lightning</td>
            <td>Trident of Lightning</td>
          </tr>
          <tr>
            <td>Lich Dust</td>
            <td>Enervating Powder</td>
          </tr>
          <tr>
            <td>Malyass Root Paste</td>
            <td>Tangle Root Toxin</td>
          </tr>
          <tr>
            <td>Mask of the Banshee</td>
            <td>Guise of the Smirking Devil</td>
          </tr>
          <tr>
            <td>Oil of Object Animation</td>
            <td>Oil of Dynamism</td>
          </tr>
          <tr>
            <td>Owlbear Claw</td>
            <td>Predator's Claw</td>
          </tr>
          <tr>
            <td>Purple Worm Venom</td>
            <td>Cave Worm Venom</td>
          </tr>
          <tr>
            <td>Rhino Hide</td>
            <td>Onslaught Hide</td>
          </tr>
          <tr>
            <td>Ring of Energy Resistance</td>
            <td>Charm of Resistance</td>
        </tr>
          <tr>
            <td>Rungu</td>
            <td>Cruuk</td>
          </tr>
          <tr>
            <td>Salamander Elixir</td>
            <td>Cooling Elixir</td>
          </tr>
          <tr>
            <td>Salve of Slipperiness</td>
            <td>Tricky Liniment</td>
          </tr>
          <tr>
            <td>Shadow Essence</td>
            <td>Nethershade</td>
          </tr>
          <tr>
            <td>Silversheen</td>
            <td>Silver Salve</td>
          </tr>
          <tr>
            <td>Sinew-Shock Serum</td>
            <td>Surging Serum</td>
          </tr>
          <tr>
            <td>Smokestick</td>
            <td>Smoke Ball</td>
          </tr>
          <tr>
            <td>Sovereign Glue</td>
            <td>Everlasting Adhesive</td>
          </tr>
          <tr>
            <td>Spell-Storing Rune</td>
            <td>Spell Reservoir</td>
        </tr>
          <tr>
            <td>Sunrod</td>
            <td>Glow Rod</td>
          </tr>
          <tr>
            <td>Tanglefoot bag</td>
            <td>Glue Bomb</td>
          </tr>
          <tr>
            <td>Thunderstone</td>
            <td>Blasting Stone</td>
          </tr>
          <tr>
            <td>Tindertwig</td>
            <td>Matchstick</td>
          </tr>
          <tr>
            <td>Universal Solvent</td>
            <td>Absolute Solvent</td>
          </tr>
          <tr>
            <td>Wand of Manifold Missiles</td>
            <td>Wand of Shardstorm</td>
          </tr>
          <tr>
            <td>Wand of Slaying</td>
            <td>Wand of Slaughter</td>
          </tr>
          <tr>
            <td>Winter Wolf Elixir</td>
            <td>Witchwarg Elixir</td>
          </tr>
    </tbody>
</table>