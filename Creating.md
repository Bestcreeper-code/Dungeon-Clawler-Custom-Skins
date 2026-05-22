# DungeonClawler Custom stuff Mod 

<br>

---
- [Mod Directory Structure Overview](#directory-structure-overview)
- [Global Config (`config.cfg`)](#global-config-configcfg)

---
<br>

# Custom stuff
- [Skins](#skins)
- [Characters](#characters)
- [Items](#items-dcitem)
- [Perks](#perks-dcperk)
- [Status Effects](#status-effects-dcstatus)
- [Pets](#pets-dcpet)
- [Liquids](#liquids-dcliquid)
---
<br>

# Syntax for common fields
- [Ability Syntax](#ability-syntax-shared-across-items-perks-pets-status-effects)
- [Ability Types & Parameters](#ability-types--parameters)
- [Value Expressions](#value-expressions-calculatedvalue)
- [Item & Perks Lists Syntax](#item-name-syntax-in-items-and-perks)
---
###### If a field is not marked as required, it is optional.
###### In most cases, only `Name` is required.

<br>
<br>
Terms:
- Object: a custom content (item, perk, pet, status effect, etc.)
<br><br><br><br><br><br><br><br> 


## Directory Structure Overview

```
BepInEx/plugins/DungeonClawlerCustomSkins/
├── config.cfg
├── Items/
│   └── MyItem/
│       ├── *.DcItem
│       └── *.png
├── Perks/
│   └── MyPerk/
│       ├── *.DcPerk
│       └── *.png
├── Characters/
│   └── MyCharacter/
│       ├── FighterPerk.DcPerk        ← exact name is required 
│       ├── *config*                  ← filename must contain "config"
│       ├── *idle.png
│       ├── *attack.png
│       ├── *buff.png
│       ├── *hit.png
│       ├── *paw.png
│       ├── *perk.png
│       ├── *backpack.png
│       └── *dam/                     
            ├── *idle.png
│           ├── *attack.png
│           ├── *buff.png
│           └── *hit.png
│       └── *cat/                     
|           ├── *idle.png
│           ├── *attack.png
│           ├── *buff.png
│           └── *hit.png
├── Liquids/
│   └── MyLiquid/
│       └── *.DcLiquid
├── StatusEffects/
│   └── MyEffect/
│       ├── *.DcStatus
│       └── *.png
├── Pets/
│   └── MyPet/
│       ├── *.DcPet
│       ├── *idle.png
│       ├── *attack.png
│       ├── *buff.png
│       └── *hit.png
└── Skins/
    └── MySkin/
        ├── *config*                  ← filename must contain "config"
        ├── *idle.png
        ├── *attack.png
        ├── *buff.png
        ├── *hit.png
        ├── *paw.png
        ├── *perk.png
        ├── *backpack.png
        ├── *dam/
        └── *cat/
```
(here * means you can put whatever before the text)

---

## Global Config (`config.cfg`)

Controls default sprite sizing for all mods.

```
SizeMult:100
```

| Field      | Type  | Description                                              |
|------------|-------|----------------------------------------------------------|
| `SizeMult` | float | **Default pixels size divisor. `100` = standard size**.<br>*(changing it isn't really a good idea and the size multiplier should isntead be changed in the different object files as this one is global to the whole mod)*|

---

## Ability Syntax (shared across Items, Perks, Pets, Status Effects)

Abilities are basically the behaviour of an object. They  are defined on **<ins>a single line<ins>** using semicolon-separated fields:

```
Trigger:OnTurnStart; Target:Self; Ability:DamageSetting(DamageAmount:5, CanCrit:true)
```

### Trigger Types 
Common values: `OnTurnStart`, `OnTurnEnd`, `OnDeath`, `OnHit`, `OnHeal`, `OnAttack`, `OnBlock`, `OnCollect`, `OnRoundStart`, `OnRoundEnd`

### Target Types 
| Key              | Description                   |
|------------------|-------------------------------|
| `Self`           | The item/perk owner (aka the player,pet or maybe enemy in some edge case)          |
| `FirstTarget`    | First enemy                   |
| `LastTarget`     | Last enemy                    |
| `AllTargets`     | All enemies                   |
| `Attacker`       | Whatever attacked the user to trigger that ability(only works with block and hit triggers)         |
| `RandomTarget`   | A random enemy                |
| `Player`         | The player (again)            |
| `PreviousTarget` | Last targeted  |
| `Everyone`       | All Entity on the field       |

### Common Ability Fields

| Field           | Type | Default | Description                        |
|-----------------|------|---------|------------------------------------|
| `TargetFriends` | bool | false   | Target allies instead of enemies   |
| `ExcludeSelf`   | bool | false   | Exclude self from targeting(for ex if you use the `Everyone` target but dont want to affect the user)        |

---

## Ability Types & Parameters examples 
##### (all number values can be replaced by a "Value" )

### `DamageSetting`
```
Ability:DamageSetting(DamageAmount:5, CanCrit:true, IgnoreArmor:false, IgnoreAttackDamageModifiers:false, IgnoreDamageModifiers:false, DontTriggerOnAttackEffects:false, BaseCritChance:5)
```

### `HealSetting`
```
Ability:HealSetting(HealAmount:5)
```

### `IncreaseBlockSetting`
```
Ability:IncreaseBlockSetting(BlockAmount:5, Multiply:false)
```

### `IncreaseStrengthSetting`
```
Ability:IncreaseStrengthSetting(StrengthAmount:5, Multiply:false)
```

### `ApplyStatusEffectSetting`
```
Ability:ApplyStatusEffectSetting(AddAmount:3, StatusEffect:Poison, Multiply:false, ReplaceExistingStacks:false, Unavoidable:false)
```

### `RemoveStatusEffectSetting`
```
Ability:RemoveStatusEffectSetting(StatusEffect:Poison, RemoveAll:false, RemoveAmount:1)
```

### `AddItemSetting`
```
Ability:AddItemSetting(Item:Dagger, ItemAmount:1, Upgraded:false, TriggerInMysteryRooms:false)
```

### `DestroyItemSetting`
```
Ability:DestroyItemSetting(PercentageOfItems:0.5, AllItems:false, TargetItem:Dagger, ShowParticleEffect:true)
```

### `SetWaterLevel`
```
Ability:SetWaterLevel(Level:5, Liquid:Water, ChangeLiquid:true)
```

### `AddCurrency`
```
Ability:AddCurrency(Amount:10)
```

### `RemoveCurrency`
```
Ability:RemoveCurrency(Amount:10)
```

### `ReplaceClaw`
```
Ability:ReplaceClaw(Claw:Normal_Claw)
```

### `AddClawSetting`
```
Ability:AddClawSetting(Claw:Normal_Claw)
```

### `ModifyBaseValue`
```
Ability:ModifyBaseValue(BaseValue:myVar, ChangeAmount:1, Overwrite:false, Multiply:false, FighterBaseValue:false)
```

### `ReplaceItemAction`
```
Ability:ReplaceItemAction(TargetItem:Dagger, ReplacementItem:Sword, ItemCount:1, Upgraded:false)
```

### `SpawnPetAction`
```
Ability:SpawnPetAction(Pet:Dam, PetAmount:1, HasStartEffect:true, StartBuff:Poison, BuffAmount:0)
```

### `SpawnEnemyAction`
```
Ability:SpawnEnemyAction(Enemy:Squalo, HasStartEffect:true, DamageAmount:Poison, EffectAmount:5)
```

### `KillAction`
```
Ability:KillAction(KillEnabled:1) 
```
###### that `KillEnabled` fields can use a set of "Value" to make a conditional kill

<!-- ### `ModifyDamageMult`
```
Ability:ModifyDamageMult(Value:2)
``` -->

---

## "Value" Expressions 

Values can be plain numbers or nested expressions (that can allow a lot of possibilities if used correctly):

```
5
Add{CombatantStat{Block}, Number{3}}
Multiply{PlayerStat{CriticalHitChance}, 2}
IfElse{GreaterThan, CombatantStat{CurrentHealth}, 10, Add{5,PlayerStat{MaxRetainedBlock}},1}
```

### Value Types

| Type                    | Args                              | Description                              |
|-------------------------|-----------------------------------|------------------------------------------|
| `Number{n}`             | float                             | Constant value(just for compatibility because you can just use a plain number)    |
| `Add{v1, v2}`           | value, value                      | v1 + v2                                  |
| `Subtract{v1, v2}`      | value, value                      | v1 - v2                                  |
| `Multiply{v1, v2}`      | value, value                      | v1 × v2                                  |
| `Divide{v1, v2}`        | value, value                      | v1 ÷ v2                                  |
| `Floor{v}`              | value                             | Round down                               |
| `Clamp{v, min, max}`    | value, value, value               | Clamp between min and max                |
| `RandomNumber{max, min}`| value, value                      | Random(self explanatory)                 |
| `IfElse{cmp, v, cv, t, f}` | comparison, value, compareVal, trueVal, falseVal | Conditional |
| `CombatantStat{stat}`   | `Block`, `Strength`, `MaxHealth`, `CurrentHealth` | Entity stat    |
| `PlayerStat{stat}`      | `MaxRetainedBlock`, `MaxRetainedDodge`, `CriticalHitChance`, `CriticalHitDamage`, `PetDamage` | Player stat |
| `ClawMachineStat{stat}` | `WaterLevel`, `NumberOfItemsThisTurn`, `CollectedFluffThisTurn` | Some useful claw machine stats for the current turn |
| `BaseValue{name}`       | string                            | Item's own base value by name            |
| `FighterBaseValue{name}`| string                            | Character's base value by name             |
| `ItemCount{itemName}`   | item name (use `_` for spaces)    | Count of item in deck                    |
| `CollectedItemCount{itemName}` | item name                  | Count collected this run                 |
| `CountOfItemsInDeck{}`  |                                   | Total items in deck                      |
| `EnemyCount{effectName}`| status effect name or empty       | Number of enemies (optionally with effect)|
| `StatusEffectCount{effectName}` | status effect name        | Stacks of status effect on self          |
| `CurrencyAmount{}`      |                                   | Current gold                             |
| `LastIncomingAbilityStats{stat}` | see below                | Last combat event stats                  |
| `AmountOfCLaws{}`       |                                   | Number of claws by default on this character(paws included)                         |
<!-- | `DamageMult{}`          |                                   | Current damage multiplier(unused value so don't really mind lol)                | -->
| `LastPickedItem{}`      |                                   | Index of last picked item                |
| `Item{itemName}`        | item internal name                         | Item's index value                       |

**Comparison types** (for `IfElse`): `LessThan`, `GreaterThan`, `EqualTo`, `NotEqualTo`, `LessThanOrEqualTo`, `GreaterThanOrEqualTo`

**LastIncomingAbilityStats stats**: `TotalIncomingDamage`, `HealthDamage`, `BlockedDamage`, `IncomingHeal`, `BlockIncrease`, `StrengthIncrease`, `CriticalHealthDamage`, `TotalOutgoingDamage`, `OutgoingCriticalHealthDamage`, `OutgoingHealthDamage`, `OutgoingHeal`, `TotalDodgedDamage`, `CoinsAdded`, `CoinsRemoved`, `CoinsCollected`

---

## Items (`.DcItem`)

**Directory:** `Items/<ItemName>/`
**File:** `*.DcItem`
**Sprite:** any `.png` in the same folder

### Full Example
```
Name:My Sword
Description:Deals 10 damage on turn start.
Rarity:Rare
Material:Iron
Cooldown:2
Sizemult:80
DontDrop:false
Fluff:false
Ethereal:false
Overwrite:true
Tags:[Offensive;Melee]
Values:[damage:10;count#:0]
UpgradeValues:[damage:15]
UpgradeDescription:Deals 15 damage instead.

Trigger:OnTurnStart; Target:Self; Ability:DamageSetting(DamageAmount:BaseValue{damage}, CanCrit:true)
```

### Field Reference

| Field                | Type              | Description                                                              |
|----------------------|-------------------|--------------------------------------------------------------------------|
| `Name`               | string            | Item name (required)                                                     |
| `Description`        | string            | Tooltip description                                                      |
| `UpgradeDescription` | string            | Description shown after upgrading                                        |
| `Rarity`             | enum              | `Normal`, `Uncommon`, `Rare`, `Epic`, `Legendary`                        |
| `Material`           | string            | Material name (e.g. `Cloth`, `Iron`, `Gold`)                             |
| `Cooldown`           | int               | Turns between activations   |
| `Sizemult`           | float             | Sprite scale override (0 = use global config)                            |
| `DontDrop`           | bool              | If true, item won't appear in reward pools                               |
| `Fluff`              | bool              | If true, item is a filler/fluff type                                     |
| `Ethereal`           | bool              | If true, item has Ethereal property                                      |
| `Overwrite`          | bool              | If false, preserves existing abilities when modifying a base game item   |
| `Tags`               | `[Tag;Tag]`       | Item tags for tokens and rewards   |
| `Values`             | `[name:val;...]`  | Starting values. Prefix with `#` for persistent, `&` for Character-scoped |
| `UpgradeValues`      | `[name:val;...]`  | Values list after upgrade (also enables upgrading) (note that these overwrite the base "Values" so if you have for ex a "amount" var in `Values` you need to also have it in `UpgradeValues` if your Abilities rely on it)                            |
<br>
---
## Item-specific Abilities

### `Replace`
`
Ability:Replace(KeepMaterial:false, ItemReplacement:Dagger, KeepUpgrade:false)
`

### `SpawnItems`
`
Ability:SpawnItems(DestroyItem:false, ItemToSpawn:Dagger, SpawnAmount:0)
`

### `Explode`
`
Ability:Explode(Radius:0, Strength:0)
`
## Item-specific Ability Fields
Items(only) also support special for effects inside the machine:


### `Conditions[LiquidName,...]` restricts the machine effect to fire only when submerged in the listed liquid(s).

```
ex:
SpecialTrigger:OnCollect; Conditions[Water,Lava,Poison_Water]; Ability:DamageSetting(DamageAmount:10)
```

---

## Perks (`.DcPerk`)

**Directory:** `Perks/<PerkName>/`
**File:** `*.DcPerk`
**Sprite:** any `.png` in the same folder

### Full Example
```
Name:Berserker sword
Description:On turn start, deal damage equal to your Strength.
Rarity:Rare
Sizemult:80
Stackamount:3
Tags:[DealDamage]
Values:[baseDmg:5]
SpecialEffects:[IncreaseMaxHp:10]

Trigger:OnTurnStart; AbilityTarget:Fighter; Target:FirstTarget; Ability:DamageSetting(Multiply{DamageAmount:CombatantStat{Strength}, BaseValue{baseDmg}})
```

### Field Reference

| Field            | Type             | Description                                                 |
|------------------|------------------|-------------------------------------------------------------|
| `Name`           | string           | Perk name (required)                                        |
| `Description`    | string           | Tooltip text                                                |
| `Rarity`         | rarity           | `Normal`, `Uncommon`, `Rare`, `Epic`, `Legendary`           |
| `Sizemult`       | float            | Sprite scale                                                |
| `Stackamount`    | int              | Max stacks if stackable                                     |
| `Tags`           | `[Tag;Tag]`      | Item tags                                                   |
| `Values`         | `[name:val;...]` | Starting values. Prefix `#` = persistent, `&` = Character    |
| `SpecialEffects` | `[Effect:val;...]`| Special perk effects (see below)                           |

### `AbilityTarget` (for Perks/Pets)
| Key       | Description                         |
|-----------|-------------------------------------|
| `Fighter` | Owned by the player Character         |
| `Pet`     | Owned by a pet                      |
| `Enemy`   | Owned by an enemy                   |

### Special Perk Effects (`SpecialEffects:[...]`)

| Effect                       | Value | Description                              |
|------------------------------|-------|------------------------------------------|
| `IncreaseMagnetism`          | float | Expand claw grab radius                  |
| `ReducePrices`               | float | Shop price reduction                     |
| `IncreaseCritChance`         | float | Flat crit chance increase                |
| `IncreaseCritDamage`         | float | Crit damage multiplier                   |
| `IncreaseMaxHp`              | float | Bonus max HP                             |
| `DecreaseEnemyHp`            | float | Enemies start with less HP (%)           |
| `ChangeItemCapacity`         | float | Inventory size change                    |
| `ChangeFluffAmount`          | float | Fluff item count modifier                |
| `RetainBlock`                | float | Block retained between rounds            |
| `ClawTimer`                  | float | Claw timer modifier                      |
| `IncreaseCurrencyGain`       | float | Gold multiplier                          |
| `MinimumWaterLevel`          | float | Minimum water level enforced             |
| `IncreasePetDamageMultiplier`| float | Pet damage multiplier                    |
| `IncreaseDamageMultiplier`   | float | Global damage multiplier                 |
| `IncreaseBlockMultiplier`    | float | Global block multiplier                  |
| `IncreaseRewardClawCount`    | float | More claw grabs in reward rooms          |
| `AutomaticallyUpgradeItems`  | float | % chance to auto-upgrade collected items |
| `IncreasePerkChoiceCount`    | float | More perk choices shown                  |
| `RetainDodge`                | float | Dodge retained between rounds            |
| `IncreasePoisonStacks`       | float | Extra poison stacks applied              |
| `IncreaseEtherealStayChance` | float | Ethereal item stay chance                |
| `FrostIncreasesDamage`       | float | Bonus damage per frost stack             |
| `ConsumeCollectedItems`      | ItemName | Auto-consume this item when collected |
| `SpecialRewardRoomClaw`      | ClawName | Use a specific claw in reward rooms   |
| `ReplaceClaw`                | ClawName | Replace all claws with this type      |
| `RerollCoupon`               | -     | Gives free reroll coupon                 |
| `ShredderCoupon`             | -     | Gives free shredder coupon               |
| `UpgradeCoupon`              | -     | Gives free upgrade coupon                |
| `AlchemistCoupon`            | -     | Gives free alchemist coupon              |

---

## Characters

**Directory:** `Characters/<CharacterName>/`

Characters use a skin folder structure (same as Skins) plus an optional `FighterPerk.DcPerk` file.
The folder name becomes the character's internal name.

### Sprite Files (matched by filename suffix)

| Pattern        | Used For                                       |
|----------------|------------------------------------------------|
| `*idle.png`    | Idle animation, map sprite, skin thumbnail     |
| `*attack.png`  | Attack animation                               |
| `*buff.png`    | Buff / perform ability animation               |
| `*hit.png`     | Hit and death animation                        |
| `*paw.png`     | Reward paw image                               |
| `*perk.png`    | Perk icon sprite                               |
| `*backpack.png`| Backpack sprite                                |
| `*dam/`        | Subfolder for Dam pet skin (same png naming)   |
| `*cat/`        | Subfolder for Cat pet skin (same png naming)   |

### `config` File (no file extension)

```
Name:Sir Plushalot
Description:A bunny.
PerkDesc:Attacks deal bonus damage per Strength stack.
BaseHp:80
SizeMult:90
Items:[Dagger:1;Small_Shield:1]
Perks:[Berserker:1]
PermEffects:[Poison:3;Strength:2]
```

| Field         | Format                    | Description                                                         |
|---------------|---------------------------|---------------------------------------------------------------------|
| `Name`        | string                   | Character name               |
| `Description` | string                   | Character description      |
| `PerkDesc`    | string                   | Character perk description   |
| `BaseHp`      | int                      | Starting max HP        |
| `SizeMult`    | float                    | Sprite scale                    |
| `Items`       | `[ItemName:count;...]`   | Starting items. Use `ItemName?Material?upgraded` for extras|
| `Perks`       | `[PerkName:count;...]`   | Starting perks             |
| `PermEffects` | `[EffectName:stacks;...]`| Status effects applied on every fight  start  |

### `FighterPerk.DcPerk`

Uses the same format as a regular `.DcPerk` file. `Name`, `Description`, `Rarity` are overridden by the `config` file's `Name` and `PerkDesc` fields. Abilities defined here become the Character's passive perk.

---

## Liquids (`.DcLiquid`)

**Directory:** `Liquids/<LiquidName>/`
**File:** `*.DcLiquid`

### Full Example
```
Name:Lava
Description:Burns items that pass through it.
HexColor:#FF4500
Density:1.5
Effect:Burn
ItemEffect:BurnEffect
ItemReplacement:BurnedItem
TargetItems[Dagger,Sword]
```

### Field Reference

| Field             | Type      | Description                                                          |
|-------------------|-----------|----------------------------------------------------------------------|
| `Name`            | string    | Liquid name (required). Matches existing liquid to override it.      |
| `Description`     | string    | Tooltip text                                                         |
| `HexColor`        | hex color | Liquid color, e.g. `#FF4500`                                         |
| `Density`         | float     | Physics density                                                      |
| `Effect`          | enum      | (e.g. `Burn`, `Freeze`)                    |
| `ItemEffect`      | string    | Name of an `ItemEffectSetting` to apply                              |
| `ItemReplacement` | string    | Item to replace collected items with                                 |
| `TargetItems`     | `[Item,Item]` | Specific items this liquid targets (mutually exclusive with `TargetMaterials`) |
| `TargetMaterials` | `[Mat,Mat]`   | Specific materials this liquid targets                           |

---

## Status Effects (`.DcStatus`)

**Directory:** `StatusEffects/<EffectName>/`
**File:** `*.DcStatus`
**Sprite:** any `.png` in the same folder

### Full Example
```
Name:Burning
Description:Takes fire damage each turn.
Type:Debuff
IsPoison:false
IsFrost:false
CanBeStacked:true
CanOnlyBeOneEffect:false
SpecialEffect:None
TextColor:#FF4500
Overwrite:true
Values:[burnDmg:3]

Trigger:OnTurnStart; Target:Self; Ability:DamageSetting(DamageAmount:BaseValue{burnDmg}, IgnoreArmor:true)
ReduceAmountOnTrigger:true; ReduceAmount:1; RemoveOnTrigger:false
```

### Field Reference

| Field                | Type   | Description                                                            |
|----------------------|--------|------------------------------------------------------------------------|
| `Name`               | string | Effect name (required). Matches existing effect to override it.        |
| `Description`        | string | Tooltip text                                                           |
| `Type`               | enum   | `Buff` or `Debuff`                                                     |
| `IsPoison`           | bool   | Counts as a poison effect                                              |
| `IsFrost`            | bool   | Counts as a frost effect                                               |
| `CanBeStacked`       | bool   | Whether stacks can accumulate                                          |
| `CanOnlyBeOneEffect` | bool   | Only one instance can exist at a time                                  |
| `SpecialEffect`      | enum   | `ESpecialStatusEffect` value                                           |
| `TextColor`          | hex    | Stack count text color                                                 |
| `Overwrite`          | bool   | If false, appends to existing abilities instead of replacing           |
| `Values`             | `[name:val;...]` | Starting base values(floating point variables)                                         |

### Ability Line Extra Fields (Status Effects only)

| Field                   | Type  | Description                                    |
|-------------------------|-------|------------------------------------------------|
| `ReduceAmountOnTrigger` | bool  | Reduce stack count when ability fires          |
| `ReduceAmount`          | value | Amount to reduce by                            |
| `RemoveOnTrigger`       | bool  | Remove effect entirely when it fires           |

---

## Pets (`.DcPet`)

**Directory:** `Pets/<PetName>/`
**File:** `*.DcPet`
**Sprites:** same pattern as Characters (`*idle.png`, `*attack.png`, `*buff.png`, `*hit.png`)

### Full Example
```
Name:Frog
Description:Fights alongside you and poisons enemies.
BaseHealth:10
TargetedByEnemies:true
SyncStrength:false
ConsumeItems:true
ItemsToBeConsumed[Poison_Fluff]
Sizemult:80
Overwrite:true
Values:[poisonAmt:2]

Trigger:OnConsume; Ability:ModifyBaseValue(ChangeAmount:1,BaseValue:poisonAmt,Overwrite:false,Multiply:false); Target:Self

Trigger:OnAttack; Ability:ApplyStatusEffectSetting(AddAmount:BaseValue{poisonAmt},StatusEffect:Poison,Unavoidable:false); Target:FirstTarget
```

### Field Reference

| Field                 | Type              | Description                                                         |
|-----------------------|-------------------|---------------------------------------------------------------------|
| `Name`                | string            | Pet name (required). Matches existing pet to override it.           |
| `Description`         | string            | Tooltip text                                                        |
| `BaseHealth`          | value expression  | Pet's starting HP                                                   |
| `TargetedByEnemies`   | bool              | Whether enemies can target this pet                                 |
| `SyncStrength`        | bool              | Pet inherits player's Strength stat                                 |
| `Hittable`            | bool              | Whether the pet can take hits                                       |
| `ConsumeItems`        | bool              | Pet eats items from the claw machine                                |
| `ItemsToBeConsumed`   | `[Item,Item]`     | Specific items consumed (mutually exclusive with `MaterialsToBeConsumed`) |
| `MaterialsToBeConsumed`| `[Mat,Mat]`      | Specific materials consumed                                         |
| `Sizemult`            | float             | Sprite scale                                                        |
| `Overwrite`           | bool              | If false, appends abilities instead of replacing                    |
| `Values`              | `[name:val;...]`  | Starting base values(floating point variables). Prefix `&` = value for the whole Character(not localto the perk), `#` = persistent between fights|

---

## Skins

**Directory:** `Skins/<SkinName>/`

Skins are applied to existing characters via the in-game Options menu. The folder structure is identical to the sprite section of Characters. The `config` file supports the same fields as the Character `config` (Name, Description, Items, Perks, etc.) but targets an existing Character instead of creating a new one.

Selecting a skin in Options applies it to whichever character is selected in the Character dropdown.

---

## Item Name Syntax in `Items:[...]` and `Perks:[...]`

<!-- ``` -->
---
Items:[ItemName:count;**ItemName?Material?upgraded**[]()***:count***]
Perks:[PerkName:count;PerkName:count]
<!-- ``` -->
---

| Part      | Description                                          |
|-----------|------------------------------------------------------|
| `ItemName/PerkName`| Internal name of the item/perk (spaces allowed)           |
| `Material`| Optional material to assign (after first `?`)        |
| `upgraded`| Optional bool, `true` = item starts upgraded (after second `?`) |
| `count`   | How many of this you start with                  |

only the count and itemname matter for perks

Example:
```
Items:[Dagger:2;Sword?Metal?true:1]
```

---

## Notes

- **Spaces in names**: Use either a space or `_` - the mod normalizes them automatically.(though using **_** is most of the time better as most spaces are removed when parsing)
- **Persistent values** (`#` prefix): These values are saved between rooms.
- **Character-scoped values** (`&` prefix): These are stored on the Character rather than the item.
- **Overwrite flag**: Defaults to `true`. Set `Overwrite:false` to extend a base-game item or effect instead of replacing it.
- changing a name will change its ID in saves and break saves referencing that item.
