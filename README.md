# Unreal Melee Combat System



A soulslike melee combat system on unreal by blueprint.

engine version: 5.1.0

## Interfaces

![Interfaces](https://gorh.cn/unreal-melee-combat-system/screenshots/Interfaces.png)

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
| Move To                           | Let character move to specify location in the input time.    |
| Jump To                           | Let character jump to specify location in the input time.    |

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

![ActorComponents](https://gorh.cn/unreal-melee-combat-system/screenshots/ActorComponents.png)

### BP_CollisionComponent

*Collision Component* provides attack collision detection capacity for weapon actor or character.

![BP_CollisionComponent](https://gorh.cn/unreal-melee-combat-system/screenshots/BP_CollisionComponent.png)

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

| Functions              | Description                                   |
| ---------------------- | --------------------------------------------- |
| Initialize Equipment   | Initialize starting equipments for character. |
| Get Current Equipments | Get character current equipment list.         |

### BP_StateManagerComponent

*State Manager Component* is used to manage the owner's states and action.

#### Functions

| Functions                       | Description                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| Set Current State               | Set owner current state gameplay tag.                        |
| Get Current State               | Get owner current state gameplay tag.                        |
| Reset State                     | Reset owner current state gameplay tag.                      |
| Is Current State Equal To Any?  | Check owner current state is equal to any specific gameplay tag or not. |
| Set Current Action              | Set owner current action gameplay tag.                       |
| Get Current Action              | Get owner current action gameplay tag.                       |
| Is Current Action Equal To Any? | Check owner current action is equal to any specific gameplay tag or not. |

#### Dispatchers

| Dispatchers                            | Input        |
| -------------------------------------- | ------------ |
| <nobr>On Character State Begin</nobr>  | Gameplay Tag |
| <nobr>On Character State End</nobr>    | Gameplay Tag |
| <nobr>On Character Action Begin</nobr> | Gameplay Tag |
| <nobr>On Character Action End</nobr>   | Gameplay Tag |

### BP_StatsComponent

*Stats Component* is used to manage owner's stats value.

#### Event

| Event                    | Description                      |
| ------------------------ | -------------------------------- |
| <nobr>Start Regen</nobr> | Start regen specific stat value. |
| <nobr>Stop Regen</nobr>  | Stop regen specific stat value.  |

#### Public Variables

| Variables                       | Type                                             | Description                               |
| ------------------------------- | ------------------------------------------------ | ----------------------------------------- |
| <nobr>Base Stats</nobr>         | <nobr>E_StatsType -> F_BaseStat (mapping)</nobr> | Owner base stats and value configuration. |
| <nobr>Stamina Regen Rate</nobr> | Float                                            | Stamina regen rate, per 0.1 second.       |
| Regen Delay                     | Float                                            | Stamina regen delay time, second.         |

#### Functions

| Functions                              | Description                                                  |
| -------------------------------------- | ------------------------------------------------------------ |
| Initialize Stats                       | Initialize owner stats by `Base Stats`.                      |
| Set Base Stat Value                    | Set specific stat base value.                                |
| Get Base Stat Value                    | Get specific stat base value.                                |
| Set Max Stat Value                     | Set specific stat max value.                                 |
| Get Max Stat Value                     | Get specific stat max value.                                 |
| Set Current Stat Value                 | Set specific stat current value.                             |
| Get Current Stat Value                 | Get specific stat current value.                             |
| <nobr>Modify Current Stat Value</nobr> | Modify specific stat current value by delta value.           |
| Take Damage                            | **Damage Mulitplier** = Damage / (Damage + Armor)<br/>**Reduced Damage** = Damage x Damage Mulitplier |
| Regen Stamina                          | Regen owner current stamina value.                           |

#### Dispatchers

| Dispatchers                                | Input             |
| ------------------------------------------ | ----------------- |
| <nobr>On Current Stat Value Updated</nobr> | E_StatType, Float |

### BP_TargetingComponent

*Targeting Component* provides the ability to lock on target.

#### Event

| Event                        | Description                |
| ---------------------------- | -------------------------- |
| <nobr>Enable Lock On</nobr>  | Enable targeting.          |
| <nobr>Disable Lock On</nobr> | Disable targeting.         |
| <nobr>Toggle Lock On</nobr>  | Enable or disable lock on. |

#### Public Variables

| Variables                    | Type               | Description                            |
| ---------------------------- | ------------------ | -------------------------------------- |
| Target Object Type           | EObject Type Query | Object types that can be targets.      |
| Targeting Radius             | Float              | Sphere trace *Radius*.                 |
| Targeting Distance           | Float              | Max targeting distance.                |
| Target Rotation Interp Speed | Float              | Lock on camera rotation RInterp speed. |
| Default Rotation Mode        | E_RotationMode     | Orient to Movement / Orient to Camera  |

#### Functions

| Functions                                      | Description                                                  |
| ---------------------------------------------- | ------------------------------------------------------------ |
| Set Is Targeting                               | If target actor is valid, set `Is Targeting` to `true`, and send `On Targeted` message to target actor. |
| Is Targeting?                                  | Check owner is targeting or not?                             |
| Find Target                                    | Find closet targetable pawn as lock on target.               |
| Set Target Actor                               | Set target actor.                                            |
| Get Target Actor                               | Get current targeting actor.                                 |
| <nobr>Update Targeting Control Rotation</nobr> | Update owner controller rotate to target actor.              |
| Set Rotation Mode                              | Set rotation mode: **Orient to Movement** / **Orient to Camera**. |
| Get Rotation Mode                              | Get owner current rotation mode.                             |
| Update Rotation Mode                           | Update owner rotation mode depend on owner state.            |
| Can Target Actor?                              | Check actor can be targeted or not.                          |

#### Dispatchers

| Dispatchers                       | Input |
| --------------------------------- | ----- |
| <nobr>On Disable Targeting</nobr> | None  |

## Character Actor

All the characters are inherited from `BP_CombatCharacter_Base`, which including common logic of **Player** and **AI**.

![BP_CombatCharacter_Base_RefView](https://gorh.cn/unreal-melee-combat-system/screenshots/BP_CombatCharacter_Base_RefView.png)

### BP_CombatCharacter_Base

*CombatCharacter_Base* has the following custom actor components:

`BP_CombatComponent`, `BP_StateManagerComponent`, `BP_StatsComponent`, `BP_TargetingComponent`, `BP_EquipmentComponent`.

And implements the following interfaces:

`Combat_BPI`, `GameplayTag_BPI`, `Targeting_BPI`.

#### Public Variables

| Variables                        | Type                                | Description                                                  |
| -------------------------------- | ----------------------------------- | ------------------------------------------------------------ |
| Max Walking Speed                | Float                               | The character movement *Max Walking Speed* in walking state. |
| Max Jogging Speed                | Float                               | The character movement *Max Walking Speed* in jogging state. |
| Max Sprinting Speed              | Float                               | The character movement *Max Walking Speed* in sprinting state. |
| Unarm Hit Montage_F              | Anim Montage                        | Hit from front montage on unarm state.                       |
| Unarm Hit Montage_B              | Anim Montage                        | Hit from back montage on unarm state.                        |
| Unarm Hit Montage_L              | Anim Montage                        | Hit from left montage on unarm state.                        |
| Unarm Hit Montage_R              | Anim Montage                        | Hit from right montage on unarm state.                       |
| Knockdown Montage_F              | Anim Montage                        | Knockdown from front montage on unarm state.                 |
| Knockdown Montage_B              | Anim Montage                        | Knockdown from back montage on unarm state.                  |
| <nobr>Owned Gameplay Tags</nobr> | <nobr>Gameplay Tag Container</nobr> | Character identify gameplay tags.                            |
| Ignore Gameplay Tags             | <nobr>Gameplay Tag Container</nobr> | Character attack ignore gameplay tags which other actors owned. |
| Pelvis Bone Name                 | Name                                | Ragdoll root bone.                                           |

#### Event

| Event                        | Description                                               |
| ---------------------------- | --------------------------------------------------------- |
| Event BeginPlay              | Initialize character stats and equipments.                |
| Event Tick                   | Get delta seconds.                                        |
| Event PointDamage            | Point damage handler.                                     |
| Event On Landed              | Character on landed event.                                |
| Event Begin Rotate To Target | Start the timeline, character begin rotate to the target. |
| Event Stop Rotate To Target  | Stop the timeline.                                        |

#### Input Action

The following input actions are mapped only in `BP_CombatCharacter_Player`.

| Event                               | Description                                     |
| ----------------------------------- | ----------------------------------------------- |
| EnhancedInputAction IA_Look         | Modify controller Yaw and Pitch.                |
| EnhancedInputAction IA_Move         | Move the pawn.                                  |
| EnhancedInputAction IA_Jump         | Jump.                                           |
| EnhancedInputAction IA_Interact     | Interact with near actor.                       |
| EnhancedInputAction IA_ToggleCombat | Toggle character **Common** / **Battle** state. |
| EnhancedInputAction IA_ToggleWalk   | Toggle character **Walk** / **Jogging** state.  |
| EnhancedInputAction IA_ToggleLock   | Toggle character targeting state.               |
| EnhancedInputAction IA_LightAttack  | Light attack input.                             |
| EnhancedInputAction IA_HeavyAttack  | Heavy attack input.                             |
| EnhancedInputAction IA_Dodge        | Dodge input.                                    |
| EnhancedInputAction IA_Sprint       | Sprint input.                                   |
| EnhancedInputAction IA_Block        | Block input.                                    |
| EnhancedInputAction IA_UseItem      | Use current equipment item input.               |

#### FSM

| Event                         | Description                                        |
| ----------------------------- | -------------------------------------------------- |
| On Character State Begin      | Called on character enter any state.               |
| On Character State End        | Called on character exit any state.                |
| On Character Action Begin     | Called on character start any action.              |
| On Character Action End       | Called on character exit any action.               |
| On Current Stat Value Updated | Called on character any stats value been modified. |

#### Functions

| Functions                                       | Description                                                  |
| ----------------------------------------------- | ------------------------------------------------------------ |
| Can Perform Toggle Combat?                      | Check character can toggle combat or not.                    |
| Can Perform Attack?                             | Check character can perform attack or not.                   |
| Can Perform Dodge?                              | Check character can perform dodge or not.                    |
| Can Perform Sprinting?                          | Check character can perform sprinting or not.                |
| Can Perform Block?                              | Check character can perform block or not.                    |
| Can Jump?                                       | Check character can jump or not.                             |
| Can Block Attack?                               | Check character can block attack or not.                     |
| Set Movement Speed Mode                         | Set character current move speed.                            |
| Get Movement Speed Mode                         | Get character current move speed.                            |
| Get Desired Attack Type                         | Get desired attack type by character current state.          |
| Attack                                          | Try to start attack by desired attack type (Light Attack, Heavy Attack, Falling Attack, Sprinting Attack). |
| Charge Attack                                   | Try to start charge attack.                                  |
| Charge Attack Timer                             | Accumulated charging time.                                   |
| Reset Charge Attack                             | Reset charging time and relevant flags.                      |
| Sprinting Timer                                 | Accumulated sprinting time.                                  |
| Sprinting Stamina Cost                          | Modify sprinting stamina stat value.                         |
| Disable Sprinting                               | Disable sprinting.                                           |
| Enable Ragdoll                                  | Set ragdoll collision and all bodies below simulate physics. |
| Get Hit Direction                               | Get hit direction when received damage.                      |
| <nobr>Apply Hit Reacion Physics Velocity</nobr> | Apply ragdoll physics linear velocity.                       |

### BP_CombatCharacter_Player

*CombatCharacter_Player* is child blueprint class of `BP_CombatCharacter_Base`, it add mapping enhanced input local player subsystem on *Begin Play*.

![BP_CombatCharacter_Player](https://gorh.cn/unreal-melee-combat-system/screenshots/BP_CombatCharacter_Player.png)

### BP_MasterAI

*MasterAI* is also child blueprint class of `BP_CombatCharacter_Base`, it create the health bar widget on *Begin Play*.

![BP_MasterAI_BeginPlay](https://gorh.cn/unreal-melee-combat-system/screenshots/BP_MasterAI_BeginPlay.png)

The head health bar when this actor on targeted by player.

![BP_MasterAi_Event_OnTargeted](https://gorh.cn/unreal-melee-combat-system/screenshots/BP_MasterAi_Event_OnTargeted.png)

## Equippable Actor

![BP_BaseEquippableRefView](https://gorh.cn/unreal-melee-combat-system/screenshots/BP_BaseEquippableRefView.png)

### BP_BaseEquippable

*Base Equippable* is euippable actor base blueprint class, it has *Skeletal Mesh Component* and *Static Mesh Component*.

#### Public Variables

| Variables           | Type                   | Description                                          |
| ------------------- | ---------------------- | ---------------------------------------------------- |
| Attach Socket Name  | Name                   | Socket name that attach to character mesh component. |
| Owned Gameplay Tags | Gameplay Tag Container | Identify gameplay tags.                              |

#### Functions

| Functions       | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| Attach Actor    | Attach actor to character mesh component.                    |
| Get Item Mesh   | Get actor *Skeletal Mesh Component* or *Static Mesh Component*. |
| On Equipped     | On actor equipped.                                           |
| On Unequipped   | On actor unequipped.                                         |
| Set Is Equipped | Set equipped flag.                                           |
| Is Equipped?    | Check actor is equipped on character or not.                 |

### BP_BaseWeapon

*Base Weapon* is one of `BP_BaseEquippable` child blueprint class, all weapons are created by this or inherited from this class.

It has `BP_CollisionComponent` and handle the  `On Hit` event.

#### Public Variables

| Variables                              | Type                                         | Description                                                  |
| -------------------------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| Combat Type                            | E_CombatType                                 | Weapon combat type (Light Sword, Great Sword, Twin Sword...) |
| Damage Type Class                      | Damage Type (Class Ref)                      | Weapon current damage type.                                  |
| Enter Combat                           | Anim Montage                                 | Arm weapon animation montage.                                |
| Exit Combat                            | Anim Montage                                 | Unarm weapon animation montage.                              |
| Hit Montage_F                          | Anim Montage                                 | Undefense hit from front animation montage.                  |
| Hit Montage_B                          | Anim Montage                                 | Undefense hit from back animation montage.                   |
| Hit Montage_L                          | Anim Montage                                 | Undefense hit from left animation montage.                   |
| Hit Montage_R                          | Anim Montage                                 | Undefense hit from right animation montage.                  |
| Defense Hit Montage                    | Anim Montage                                 | Defense hit animation montage.                               |
| Defense Hit Broken Montage             | Anim Montage                                 | Defense hit broken animation montage.                        |
| Knockdown Montage_F                    | Anim Montage                                 | Knockdown from front animation montage.                      |
| Knockdown Montage_B                    | Anim Montage                                 | Knockdown from back animation montage.                       |
| Light Attack Montages                  | Anim Montage (array)                         | Light attack animation montage array.                        |
| Heavy Attack Montages                  | Anim Montage (array)                         | Heavy attack animation montage array.                        |
| Charged Attack Montages                | Anim Montage (array)                         | Charged attack animation montage array.                      |
| Falling Attack Montages                | Anim Montage (array)                         | Falling attack animation montage array.                      |
| <nobr>Sprinting Attack Montages</nobr> | Anim Montage (array)                         | Sprinting attack animation montage array.                    |
| Dodge Montages                         | Anim Montage (array)                         | Dodge animation montage array.                               |
| Slide Montages                         | Anim Montage (array)                         | Slide animation montage array.                               |
| Die Montages                           | Anim Montage (array)                         | Die animation montage array.                                 |
| Special Attack Montages                | Anim Montage (array)                         | Special attack animation montage array.                      |
| Damage                                 | Float                                        | Weapon current damage.                                       |
| Action Stat Cost                       | <nobr>Gameplay Tag -> Float (Mapping)</nobr> | Each action stat value cost.                                 |
| Action Damage Multiplier               | <nobr>Gameplay Tag -> Float (Mapping)</nobr> | Each attack action damage multiplier.                        |
| Block Physical Damage Reduction Rate   | Float                                        | Damage reducion rate when block physics damage succeed.      |
| Block Magic Damage Reduction Rate      | Float                                        | Damage reducion rate when block magic damage succeed.        |
| Enable Trail FX?                       | Boolean                                      | Enable weapon attack Trail FX or not.                        |
| Trail FX                               | Particle System                              | Weapon attack Trail FX.                                      |
| Enable Dust FX?                        | Boolean                                      | Enable weapon attack Dust FX or not.                         |
| Dust FX                                | Particle System                              | Weapon attack Dust FX.                                       |

#### Event

| Event  | Description                               |
| ------ | ----------------------------------------- |
| On Hit | `BP_CollisionComponent` dispatcher event. |

#### Functions

| Functions                | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| Toggle Combat            | Call owner combat component `Set Combat Enable`.             |
| Simulate Weapon Physics  | Let weapon start simulate physics.                           |
| Get Stat Cost For Action | Get specify action stat cost value.                          |
| Get Damage               | Get weapon damage.                                           |
| On Hit                   | Check hit actor can recieve damage or not, if true, apply the damage. |
| Get Action Montages      | Get specify action montage array.                            |
| Activate Collision       | Call `BP_CollisionComponent` function `Activate Collision`.  |
| Deactivate Collision     | Call `BP_CollisionComponent` function `Deactivate Collision`. |
| Activate Trail FX        | Begin weapon particle system trail.                          |
| Deactivate Trail FX      | End weapon particle system trail.                            |
| Activate Dust FX         | Spawn weapon dust particle system emitter.                   |

### BP_MasterPose

Character protective gear base blueprint class, inherited from `BP_BaseEquippable`, using *Leader Pose Component* (which named *Master Pose Component* in UE4).

#### Public Variables

| Variables  | Type  | Description                                      |
| ---------- | ----- | ------------------------------------------------ |
| Base Armor | Float | The base armor value provided by this equipment. |

#### Functions

| Functions     | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| Attach Actor  | Attach to character skeletal mesh component and set leader pose component. |
| On Equipped   | Override parent function and modify owner current armor stat value. |
| On Unequipped | Override parent function and modify owner current armor stat value. |

### BP_ConsumableItem

#### Public Variables

| Variables         | Type                 | Description                                 |
| ----------------- | -------------------- | ------------------------------------------- |
| Use Item Montages | Anim Montage (array) | Character use item animation montage array. |
| Number of Used    | Integer              | The number of each consumed.                |
| Number in Bagpack | Integer              | Initialize item number.                     |

#### Functions

| Functions                | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| On Equipped              | Override parent function and call owner *Combat Component* `Set Item`. |
| On Unequipped            | Override parent function and call owner *Combat Component* `Set Item` to null. |
| Get Use Item Montages    | Return use item animation montage array.                     |
| Get Remaining Item Count | Retrurn remaining item number in bag pack and can consumed or not. |
| On Item Consumed         | Consume item and update reamaining count.                    |

## Animation Notifies

![AnimNotifies](https://gorh.cn/unreal-melee-combat-system/screenshots/AnimNotifies.png)

#### Notify

| Notify               | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| AttachWeaponActor_AN | If character action animation is without control bone like *ik_hand_gun*, use this notify to attach or cancel attach character skeletal. |
| ContinueAttack_AN    | Send message `Combat_BPI.ContinueAttack`.                    |
| EnableLockOn_AN      | Send message `Targeting_BPI.EnableLockOn`.                   |
| JumpToTarget_AN      | Send message `Combat_BPI.JumpTo`.                            |
| ModifyStat_AN        | Modify receiver stat value or consume item.                  |
| PlayCameraShake_AN   | Call `ClientStartCameraShake`, shake player camera.          |
| ResetCombat_AN       | Send message `Combat_BPI.ResetCombat`.                       |
| ToggleCombat_AN      | Toggle receiver combat state.                                |
| WeaponDust_AN        | Activate receiver specify weapon Dust FX.                    |

#### Notify State

| Notify State        | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| CollisionTrace_ANS  | Enable collision detection for weapons during the state.     |
| DisableMovement_ANS | Send message `Combat_BPI.SetMoveable`, disable owner move during the state. |
| IFrame_ANS          | Send message `Combat_BPI.SetInvincibility`, set owner be invincibility during the state. |
| RotateCharacter_ANS | Control receiver rotation by desire rotation during the state. |
| WeaponTrail_ANS     | Activate receiver specify weapon Trail FX during the state.  |

## Behavior Tree Nodes

![BehaviourTreeNodes](https://gorh.cn/unreal-melee-combat-system/screenshots/BehaviourTreeNodes.png)

### BTService_UpdateBehavior



## Gameplay Tag List

```ini
+GameplayTagList=(Tag="Character.Action")
+GameplayTagList=(Tag="Character.Action.Attack")
+GameplayTagList=(Tag="Character.Action.Attack.Charged Attack")
+GameplayTagList=(Tag="Character.Action.Attack.Falling Attack")
+GameplayTagList=(Tag="Character.Action.Attack.Heavy Attack")
+GameplayTagList=(Tag="Character.Action.Attack.Light Attack")
+GameplayTagList=(Tag="Character.Action.Attack.Special Attack")
+GameplayTagList=(Tag="Character.Action.Attack.Sprinting Attack")
+GameplayTagList=(Tag="Character.Action.Die")
+GameplayTagList=(Tag="Character.Action.Dodge")
+GameplayTagList=(Tag="Character.Action.Enter Blocking")
+GameplayTagList=(Tag="Character.Action.Enter Combat")
+GameplayTagList=(Tag="Character.Action.Exit Blocking")
+GameplayTagList=(Tag="Character.Action.Exit Combat")
+GameplayTagList=(Tag="Character.Action.General Action")
+GameplayTagList=(Tag="Character.Action.Slide")
+GameplayTagList=(Tag="Character.Action.Use Item")
+GameplayTagList=(Tag="Character.Player")
+GameplayTagList=(Tag="Character.State.Attacking")
+GameplayTagList=(Tag="Character.State.Blocking")
+GameplayTagList=(Tag="Character.State.Dead")
+GameplayTagList=(Tag="Character.State.Disabled")
+GameplayTagList=(Tag="Character.State.Dodging")
+GameplayTagList=(Tag="Character.State.General Action State")
```

## Artistic Assets
