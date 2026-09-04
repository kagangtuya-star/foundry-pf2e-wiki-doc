Pathfinder, like virtually all tabletop roleplaying games, was not written to be implemented as a computer program. At a live table, when there are ambiguities, apparent contradictions, and similar issues in the rules, they are not typically so serious that a gamemaster cannot make rulings ad hoc to resolve whatever has caused a pause in the flow of play. With a VTT implementation, there is sometimes greater tension when it comes to automating these rules, and occasionally the developers feel it necessary to bake in RAI judgement calls.

This page attempts to document areas in the Second Edition rules that made us feel compelled to make such calls, as well as common questions among users of the system that we believe have clear RAW answers.

---

### Why do some PC sheets show separate "fist" and "unarmed attack" entries? Are these not the same?

Fists and unspecific unarmed attacks may share a common stat block, but they are not identical (_Player Core_ pg. 275).

> The Unarmed Attacks table (page 277) lists the statistics for an unarmed attack with a fist, though you'll usually use the same statistics for attacks made with any other parts of your body.

Notably, attacking with a fist has additional restrictions compared to, say, an imagined hip check or headbutt (_ibid._, pg. 283).

> An unarmed attack can't be Disarmed. It also doesn't take up a hand, though a fist or other grasping appendage generally works like a free-hand weapon.

Some feats, class features, etc. grant abilities specifically to fists (_Player Core 2_, pg. 116).

> The damage die for your fist increases to 1d6 instead of 1d4. You don't take the normal –2 circumstance penalty when making a lethal attack with your fist or any other unarmed attacks.

The Pathfinder Second Edition system in Foundry adopts language from _Player Core_ (pg. 276) to make this distinction (emphasis ours).

> The tables on pages 277–281 list the statistics for various melee and ranged weapons that you can purchase, as well as the statistics for striking with a **fist** (or another **basic unarmed attack**).

---

### Why can't the Medic archetype's Treat Condition feat be added to a Free Archetype feat slot?

Free Archetype provides additional class feats (_GM Core_ pg. 84):

> The only difference between a normal character and a free-archetype character is that the character receives an extra class feat at 2nd level and every even level thereafter that they can use only for archetype feats.

Most archetype feats are also class feats (_Player Core_ pg. 215):

> Most archetype feats are taken in place of class feats, and so these are called archetype class feats.

Some archetype feats, like Treat Condition, are instead skill feats (_ibid._):

> Some archetype feats in other books have the skill trait, allowing you to take them in place of a skill feat rather than a class feat. A skill feat still counts to satisfy the requirement of the dedication.

If a GM wishes to allow an archetype skill feat to be taken instead of an archetype class feat, this can be done in Foundry by changing the category of the feat to "Class Feat".