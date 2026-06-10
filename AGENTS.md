# Agent Guide for PF2e Automation

This guide defines how an agent should create Pathfinder 2e automation plans driven by rule elements, macros, or both. It is intended for homebrew feats, actions, items, effects, and module-assisted workflows in the Foundry VTT PF2e system.

## Goal

Given a natural-language PF2e ability, produce a practical automation package:

- Rule elements when the system can express the behavior declaratively.
- Macros when the automation requires live token, target, scene, or item state that rule elements cannot reliably read.
- A hybrid design when a macro should prepare data and a rule element should apply the actual modifier.
- Clear notes for any behavior that remains manual or version-dependent.

## Required Context

Before writing a solution, identify these facts:

- The PF2e system version if available.
- Whether the automation is placed on a feat, action, equipment item, consumable, condition, or effect.
- Whether it must be always-on, toggleable, one-use, or triggered by a module such as Automated Animations.
- Whether it depends on selected targets, target conditions, held weapons, action traits, damage types, degree of success, or measured distances.
- Whether the GM accepts a macro-assisted solution.

If any of these are unknown, make a conservative assumption and state it in the output.

## Tool Priority & Knowledge Retrieval

Use tools and references in this order to find accurate syntax, roll options, and system patterns:

1. **Context7 PF2e Knowledge Base (Primary PF2e Reference)**
   Always consult **https://context7.com/foundryvtt/pf2e** first for up-to-date Rule Element documentation, macro examples, and system data paths. Use your web search or URL fetching tools to query this database by intent, PF2e terms, or expected code shape.
   *Example queries:*
   - `site:context7.com/foundryvtt/pf2e attack bonus against frightened target FlatModifier`
   - `site:context7.com/foundryvtt/pf2e weapon hands held roll option`
   - `site:context7.com/foundryvtt/pf2e EphemeralEffect example`

2. **Context7 Foundry VTT API v14 Documentation (Core Platform Reference)**
   Consult **https://context7.com/websites/foundryvtt_api_v14** for core Foundry VTT Version 14 API classes, structures, and lifecycle methods. Ensure all generated macros, UI modules, and custom automation scripts conform strictly to v14 standards.
   *Critical V14 API Guidelines:*
   - **ApplicationV2 & DialogV2**: Always use the modern `foundry.applications.api.ApplicationV2`, `DialogV2`, and `DocumentSheetV2` instead of legacy, deprecated v1 Application classes.
   - **Active Effects V2**: Utilize updated `ActiveEffect` schemas. Note that effect durations now prefer the `seconds` property, and active effects can dynamically alter token visual/lighting properties.
   - **Scene Regions**: Older `MeasuredTemplate` usage is transitioned. Use `Region` and `RegionBehavior` documents, and leverage `canvas.regions.placeRegion` API for programmatic area templates.
   - **Scene Levels**: Implement native elevation and multi-level canvas API models (`Level` documents) rather than relying on obsolete third-party Levels modules.
   *Example queries:*
   - `site:context7.com/websites/foundryvtt_api_v14 ApplicationV2 render DialogV2`
   - `site:context7.com/websites/foundryvtt_api_v14 ActiveEffect duration seconds`
   - `site:context7.com/websites/foundryvtt_api_v14 RegionBehavior token enter`
   - `site:context7.com/websites/foundryvtt_api_v14 canvas.regions.placeRegion`

3. **Local Codebase Search (If workspace is mounted)**
   If working within a cloned PF2e system repository or module workspace, use `rg` to find exact keywords.
   *Recommended patterns:*
   - `rg -n "FlatModifier|RollOption|predicate|strike-attack-roll" src -g "!{node_modules,dist,coverage,.git}"`
   - `rg -n "target:condition|self:weapon|hands-held|item:group|item:tag" src -g "!{node_modules,dist,coverage,.git}"`

4. **Foundry and PF2e Runtime Inspection**
   When a roll option or data path is uncertain, instruct the user to verify in Foundry:
   - Make the relevant roll.
   - Right-click the chat card.
   - Choose Inspect Roll.
   - Copy the relevant roll options.
   - Inspect actor, item, and condition data from the browser console when needed.

## Decision Tree

Use a pure rule element when all required facts are available during actor or roll preparation:

- Static bonus or penalty.
- Predicate based on roll options.
- Weapon trait, group, category, or held-hand state.
- Target has a condition, trait, size, or other exposed roll option.
- Damage dice or damage type adjustments.
- Toggleable option with `RollOption`.

Use a macro when the automation needs live state not reliably exposed to rule elements:

- Read a target condition badge value.
- Count selected targets.
- Choose one target from multiple targets.
- Read scene geometry, token position, measured distance, or template placement.
- Create, update, or delete temporary effects.
- Apply different effects depending on current target data.

Use a hybrid solution when the macro can capture dynamic state and the rule element can apply the mechanical modifier:

- Macro reads live state.
- Macro writes a temporary effect, item badge, actor flag, or roll option.
- Rule element reads `@item.badge.value` or actor flags.
- Rule element handles stacking type, selector, predicates, and removal.

Prefer hybrid over direct roll manipulation because it preserves PF2e modifier stacking, roll dialogs, chat transparency, and Inspect Roll debugging.

## Common Rule Element Patterns

### Flat modifier

Use for bonuses or penalties to checks, attacks, saves, AC, DCs, damage, or HP.

```json
{
  "key": "FlatModifier",
  "selector": "strike-attack-roll",
  "type": "circumstance",
  "value": 1,
  "predicate": [
    "target:condition:frightened"
  ]
}
```

### Toggleable roll option

Use when the player must manually enable an ability.

```json
{
  "key": "RollOption",
  "domain": "all",
  "option": "example-toggle",
  "toggleable": true,
  "label": "Example Toggle"
}
```

Then predicate on it:

```json
{
  "key": "FlatModifier",
  "selector": "strike-attack-roll",
  "type": "circumstance",
  "value": 1,
  "predicate": [
    "example-toggle"
  ]
}
```

### Temporary effect badge value

Use when a macro needs to store a dynamic number and the rule element should consume it.

```json
{
  "key": "FlatModifier",
  "selector": "strike-attack-roll",
  "type": "circumstance",
  "value": "@item.badge.value",
  "removeAfterRoll": "if-enabled",
  "predicate": [
    "target:condition:frightened"
  ]
}
```

### Weapon predicates

Use roll options from Inspect Roll whenever possible. Common examples:

```json
[
  "self:weapon:hands-held:1",
  {
    "or": [
      "item:group:firearm",
      "item:tag:crossbow"
    ]
  }
]
```

Validate exact roll options on the target Strike because weapon roll options can vary by PF2e version and item data.

### Inline action statistic override

Use for actions where the PF2e action macro supports a different statistic.

```html
[[/act demoralize statistic=deception]]{Demoralize}
```

This is often better than trying to model a skill substitution with `SubstituteRoll`, because `SubstituteRoll` changes the d20 result rather than the statistic used for a check.

## Macro Design Rules

Macros should be small, explicit, and safe.

- Resolve the acting actor from AA arguments if known, then from `actor`, `token.actor`, controlled token, or `game.user.character`.
- Require exactly one target when target-specific automation is needed.
- Fail with `ui.notifications.warn` instead of throwing unhandled errors.
- Use stable slugs for temporary effects so repeated use updates the old effect instead of creating duplicates.
- Delete stale effects when prerequisites are not met.
- Put mechanical modifiers in rule elements, not direct roll hooks, unless there is no alternative.
- Keep version-sensitive data reads in helper functions.

### Macro skeleton for target-derived effect

```js
const EFFECT_SLUG = "example-target-derived-bonus";
const EFFECT_NAME = "Example Target-Derived Bonus";

const macroActor =
  (typeof actor !== "undefined" ? actor : null) ??
  (typeof token !== "undefined" ? token?.actor : null) ??
  canvas.tokens.controlled[0]?.actor ??
  game.user.character;

if (!macroActor) {
  ui.notifications.warn("No actor found.");
  return;
}

const targets = Array.from(game.user.targets);
if (targets.length !== 1) {
  ui.notifications.warn("Select exactly one target.");
  return;
}

const targetActor = targets[0].actor;
if (!targetActor) {
  ui.notifications.warn("Target has no actor.");
  return;
}

function getConditionValue(actor, slug) {
  const condition = actor.itemTypes.condition.find((item) => {
    return item.slug === slug || item.system?.slug === slug;
  });

  if (!condition) return 0;

  const candidates = [
    condition.system?.value?.value,
    condition.system?.badge?.value,
    condition.badge?.value,
    condition.system?.value
  ];

  const value = candidates.find((candidate) => Number.isFinite(Number(candidate)));
  return Number(value ?? 1);
}
```

## Output Format

For each automation request, return these sections.

### Summary

State what is fully automated, what is manually triggered, and what remains manual.

### Assumptions

List PF2e version assumptions, placement assumptions, and module assumptions.

### Rule Elements

Provide complete JSON blocks. Include where each block should live.

### Macro

Provide complete JavaScript. Include how AA or another trigger should call it.

### Usage

Give short player-facing steps.

### Validation

Give exact Foundry checks:

- Add the item/effect to a test actor.
- Target a creature with and without the relevant condition.
- Make the relevant roll.
- Inspect Roll and confirm the modifier, selector, predicate, and roll options.
- Confirm cleanup behavior if `removeAfterRoll` is used.

### Limitations

State any version-sensitive fields, unverified roll options, or behavior that requires manual toggling.

## Validation Checklist

Before finalizing a solution, verify:

- The selector matches the intended roll.
- The modifier type matches Pathfinder stacking rules.
- The predicate uses roll options visible in Inspect Roll.
- The rule does not rely on undocumented target data paths unless explicitly marked experimental.
- Temporary effects have stable slugs.
- Repeated macro use updates or replaces previous effects.
- The macro handles no actor, no target, multiple targets, and missing condition.
- The effect is removed or expires according to the ability text.
- The output tells the user what to test in Foundry.

## Example Hybrid Package

Ability text:

```text
When you Strike a frightened target with a one-handed firearm or one-handed crossbow, gain a circumstance bonus to the attack roll equal to the target's frightened value.
```

Rule element inside the macro-created effect:

```json
{
  "key": "FlatModifier",
  "selector": "strike-attack-roll",
  "type": "circumstance",
  "value": "@item.badge.value",
  "removeAfterRoll": "if-enabled",
  "predicate": [
    "target:condition:frightened",
    "self:weapon:hands-held:1",
    {
      "or": [
        "item:group:firearm",
        "item:tag:crossbow"
      ]
    }
  ]
}
```

Macro behavior:

- Read selected target's `frightened` condition value.
- Create or update a temporary effect on the actor.
- Set the effect badge to the target's frightened value.
- Remove the effect if the target is not frightened.

## Anti-Patterns

Avoid these unless there is a strong reason:

- Using `SubstituteRoll` to model a skill substitution.
- Applying direct bonuses in macro roll hooks when `FlatModifier` can handle stacking.
- Creating duplicate temporary effects on every click.
- Depending on hidden data paths without an Inspect Roll or console verification step.
- Using actor flags when an item badge is enough.
- Encoding target-specific state in a permanent feat item.

