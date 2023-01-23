# Unreal Melee Combat System

A soulslike melee combat system on unreal by blueprint.

engine version: 5.1.0



Artistic Assets

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
| <nobr>Perform Death</nobr>        | Play death montage and change character state to Death.      |
| <nobr>Get Speed Mode</nobr>       | Get character speed mode (Walking, Jogging, Sprinting).      |
| <nobr>Get Desire Rotation</nobr>  | Get character desire rotation (forward or current input direction). |
| <nobr>Set Moveable</nobr>         | Set character moveable.                                      |
| <nobr>Can Recieve Damage?</nobr>  | Character can receive damage or not.                         |
| <nobr>Reset Combat</nobr>         | Reset character attack count and attack state.               |
| <nobr>Continue Attack</nobr>      | Continue play combo attack montage.                          |
| <nobr>Consume Item</nobr>         | Consume Item.                                                |
| <nobr>Set Invincibility</nobr>    | Set character invincibility or not.                          |
| Move To                           | Let character move to specify location in the specified time. |
| Jump To                           | Let character jump to specify location in the specified time. |

### GameplayTag_BPI

Gameplay tag stuff.

| Functions                               | Description                                                  |
| --------------------------------------- | ------------------------------------------------------------ |
| <nobr>Get Owned Gameplay Tags</nobr>    | Get asset's gameplay tag.                                    |
| <nobr>Has Matching Gameplay Tags</nobr> | Check if the asset has gameplay tags that matches against any of the specified tags. |

### Interactable_BPI

| Functions | Description                                  |
| --------- | -------------------------------------------- |
| Interact  | Interact with interactive objects by caller. |

### Targeting_BPI

| Functions                     | Description                                       |
| ----------------------------- | ------------------------------------------------- |
| <nobr>Can Be Targeted?</nobr> | Check character can be targeted or not.           |
| <nobr>Enable Lock On</nobr>   | **Enable** / **Disable** lock on perform.         |
| <nobr>On Targeted</nobr>      | Character on **Targeted** / **Untargeted** event. |

### Weapon_BPI

| Functions                    | Description                   |
| ---------------------------- | ----------------------------- |
| <nobr>On Owner Landed</nobr> | Weapon owner on landed event. |

## Actor Components

### BP_CollisionComponent

![BP_CollisionComponent](/Users/georgehuan/GitRepo/Unreal-Melee-Combat-System/MeleeCombatSystem/Saved/Screenshots/MacEditor/BPGraphScreenshots/BP_CollisionComponent.png)

*Collision Component* provides attack collision detection capacity for weapon actor or character.

#### Event

| Event                                     | Description                                                  |
| ----------------------------------------- | ------------------------------------------------------------ |
| <nobr>Set Collision Mesh Component</nobr> | Set static mesh or skeleton mesh as collision socket source. |
| Activate Collision                        | Clear already hit actors, and enable multi sphere trace.     |
| Deactivate Coliision                      | Disable multi sphere trace.                                  |
| Collision Trace                           | Start multi sphere trace, call `On Hit` delegate for qualified actors. |

#### Dispatchers

| Dispatchers         | Input      |
| ------------------- | ---------- |
| <nobr>On Hit</nobr> | Hit Result |

#### Public Variables

| Variables                         | Type                                | Description                              |
| --------------------------------- | ----------------------------------- | ---------------------------------------- |
| Start Socket Name                 | Name                                | Multi sphere trace *Start* location.     |
| End Socket Name                   | Name                                | Multi sphere trace *End* location.       |
| Trace Radius                      | Float                               | Multi sphere trace *Radius*.             |
| Collision Object Type             | EObject Type (array)                | Multi sphere trace *Object Type*.        |
| Actors to Ignore                  | Actor (array)                       | Multi sphere trace *Actors to Ignore*.   |
| Draw Debug Type                   | Draw Debug Type                     | Multi sphere trace *Draw Debug Type*.    |
| <nobr>Ignore Gameplay Tags</nobr> | <nobr>Gameplay Tag Container</nobr> | Multi sphere trace ignore gameplay tags. |

#### Functions

| Functions              | Description                                     |
| ---------------------- | ----------------------------------------------- |
| Is Collision Enabled?  | Check collision is enabled or not.              |
| Get Already Hit Actors | Get multi sphere trace hit actors in collision. |
| Get Last Hit Result    | Get last multi sphere trace hit result.         |
| Get Ignore Actors      | Get ignore actors list.                         |
| Add Ignore Actors      | Add actors to collision ignore list.            |
| Remove Ignore Actors   | Remove actors form ignore list.                 |

### BP_CombatComponent

*Combat Component* is used to manage the character current main weapon, shield weapons and props.

#### Public Variables

| Variables    | Type    | Description                                              |
| ------------ | ------- | -------------------------------------------------------- |
| Attack Count | Integer | Continuous attack counter, as combo montage array index. |

#### Functions

| Functions            | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| Set Main Weapon      | Set weapon as character current equipped main weapon.        |
| Get Main Weapon      | Get character current equipped main weapon.                  |
| Set Shield Weapon    | Set shield weapon character current equipped shield weapon.  |
| Get Shield Weapon    | Get character current equipped shield weapon.                |
| Set Item             | Set character current equipped item.                         |
| Get Item             | Get character current equipped item.                         |
| Consume Item         | Consume character current equipped item.                     |
| Set Combat Enable    | Toggle character **Common** / **Battle** state.              |
| Is Combat Enabled?   | Check character is in battle or not.                         |
| Reset Attack         | Reset `Attack Count` to `0`, reset `Attack Saved` and `Wait for Attack` to `false`. |
| Set Blocking Enable  | Toggle character **Defensive** / **Offensive** state.        |
| Is Blocking Enabled? | Check character is blocking or not.                          |

#### Dispatchers

| Dispatchers                   | Input             |
| ----------------------------- | ----------------- |
| <nobr>On Toggle Combat</nobr> | Boolean           |
| On Toggle Blocking            | Boolean           |
| On Item Changed               | BP_ConsumableItem |

### BP_EquipmentComponent

*Equipment Component* is character's equipment manager. 

#### Public Variables

| Variables           | Type                     | Description                                        |
| ------------------- | ------------------------ | -------------------------------------------------- |
| Starting Equipments | BP_BaseEquipment (array) | Initial equipments configuration of the character. |

#### Functions

| Functions              | Description |
| ---------------------- | ----------- |
| Initialize Equipment   |             |
| Get Current Equipments |             |

### BP_StateManagerComponent



### BP_StatsComponent



### BP_TargetingComponent



## Character Actor

![BP_CombatCharacter_Base_RefView](/Users/georgehuan/Desktop/BP_CombatCharacter_Base_RefView.png)

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

