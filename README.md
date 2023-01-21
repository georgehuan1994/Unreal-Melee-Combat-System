# Unreal Melee Combat System

A soulslike melee combat system on unreal by blueprint.

engine version: 5.1.0

## Components

### BP_CombatComponent



### BP_CollisionComponent



### BP_EquipmentComponent



### BP_StateManagerComponent



### BP_StatsComponent



### BP_TargetingComponent



## Interfaces

### AnimInstance_BPI

Implemented in the animation blueprint, used to update animation state machine.

| Functions                          | Description                                                  |
| ---------------------------------- | ------------------------------------------------------------ |
| <nobr>Update Combat Type</nobr>    | Change character locomotion animations by different weapon type (eg: No Weapon, Light Sword, Shield, Twin Sword, Great Sword...) |
| <nobr>Update Combat Enable</nobr>  | Change character **Common** / **Battle** locomotion animations. |
| <nobr>Update Blocking State</nobr> | Change character **Defensive** / **Offensive** posture locomotion animations. |

### Combat_BPI

Implemented in the combat character actor blueprint.

| Functions                         | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| <nobr>Perform Attack</nobr>       | Play the attack montage (Light Attack, Heavy Attack, Charged Attack, Falling Attack, Sprinting Attack, Special Attack...) depend on current equipped weapon, and change character state to Attack. |
| <nobr>Perform Action</nobr>       | Play the action montage (Enter Combat, Exit Combat, Dodge, Block, Slide, Die...), and change character state. |
| <nobr>Perform Use Item</nobr>     | Play the item usage montage, depend on current equipped item. |
| <nobr>Perform Hit Reaction</nobr> | Play the hit reaction montage, depend on damage direction (Front, Back, Left, Right), damage type (Melee, Knockdown, Dot...) and character blocking state. |
| Perform Death                     | Play death montage and change character state to Death.      |
|                                   |                                                              |



## Character Actor

![截屏2023-01-20 00.05.26](/Users/georgehuan/Desktop/截屏2023-01-20 00.05.26.png)

### BP_CombatCharacter_Base

All the characters are inherited from `BP_CombatCharacter_Base`, which including common logic of **Player** and **AI**.

#### Event

| Event           | Description                                |
| --------------- | ------------------------------------------ |
| Event BeginPlay | Initialize character stats and equipments. |
| Event Tick      | Get delta seconds.                         |

#### Interfaces



## Weapon Actor



## Animation Notifies

