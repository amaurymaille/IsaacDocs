# Class "EntityNPC"
### Inherits from Class: {: .inheritance }
[Entity](Entity.md)
## Functions
### Anim·Walk·Frame () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void AnimWalkFrame ( string HorizontalAnim, string VerticalAnim, float SpeedThreshold ) {: .copyable aria-label='Functions' }

___
### Calc·Target·Position () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) CalcTargetPosition ( float DistanceLimit ) {: .copyable aria-label='Functions' }

___
### Can·Be·Damaged·From·Velocity () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### boolean CanBeDamagedFromVelocity ( [Vector](Vector.md) Velocity ) {: .copyable aria-label='Functions' }

___
### Can·Reroll () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### boolean CanReroll ( ) {: .copyable aria-label='Functions' }

___
### Fire·Boss·Projectiles () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### [EntityProjectile](EntityProjectile.md) FireBossProjectiles ( int NumProjectiles, [Vector](Vector.md) TargetPos, float TrajectoryModifier, [ProjectileParams](ProjectileParams.md) Params ) {: .copyable aria-label='Functions' }
fire a number of projectiles, optionally targeting the player direction is randomized, or slightly randomized when targeting the player FallingAccelModifier can be used to make projectiles fall faster to the ground returns a pointer to the projectile spawned last (useful e.g. when NumProjectiles=1)
___
### Fire·Projectiles () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void FireProjectiles ( [Vector](Vector.md) Pos, [Vector](Vector.md) Velocity, ProjectilesMode Mode, [ProjectileParams](ProjectileParams.md) Params ) {: .copyable aria-label='Functions' }

Fire a number of projectiles. Each projectile is spawned at `Pos`. `Velocity` will determine the speed at which each bullet will move as well as the direction in which they will move. `Mode` is used to determine how many projectiles are spawned, and the eventual shape they will form. `Params` is used to define miscellaneous parameters, such as the speed at which the projectiles will fall to the ground, their eventual properties (can they hit other enemies, do they move in a spiral or in a straight line, etc.).

???- info "ProjectilesMode"
    * 0 : single projectile
    * 1 : two projectiles (uses params.Spread)
    * 2 : three projectiles (uses params.Spread)
    * 3 : three projectiles (uses params.Spread, more spread out?)
    * 4 : four projectiles (uses params.Spread)
    * 5 : five projectiles (uses params.Spread)
    * 6 : four projectiles in a + pattern (uses velocity.x as speed)
    * 7 : four projectiles in a x pattern (uses velocity.x as speed)
    * 8 : eight projectiles in a star pattern (uses velocity.x as speed)
    * 9 : N projectiles in a circle (velocity.x = speed, velocity.y = N, params.FireDirectionLimit and params.DotProductLimit to fire in an arc only)

    **Notes regarding ProjectileModes 1 to 5 (included)**

    When using ProjectileMode 1 to 5 (included), the `params.Spread` value is used to determine the spacing between each bullet. This value does not follow a linear progression and should therefore be studied through trial and error until the desired result is achieved (i.e., using `params.Spread = 2` wil not result in projectiles being twice as spaced as with `params.Spread = 1`). An interesting property of using these modes is the fact that the game will compute the velocity of each bullet in such a way that they all disappear at the same time (this means the game will not apply exactly the velocity you requested, but will instead use it as a base from which it will derive the resulting velocities). For these modes, `Velocity` is used both as the base velocity and as a directional vector. In effect, the game will spawn bullets in a cone in a given direction. A cone is defined by its angle and by its direction. For instance, if we consider an upwards pointing cone, like this `\/`, its direction would be the vector `(0, -1)`, and its angle would be whatever the angle between these two lines is. In this function, the width of the cone is based on `params.Spread` and `mode`, the direction of the cone is `Velocity` normalized. 

    For `modes` 1 and 2, a given value of `params.Spread` will yield the same angle. For `modes` 3 to 5 (included), a given value of `params.Spread` will also yield the same angle (slightly wider than for `modes` 1 and 2). For instance, `params.Spread = 1` will result in an angle of `32.04°` for modes 1 and 2, and an angle of `48.006°` for modes 3 to 5 (included).

    * For `mode = 1`, the bullets will be fired in a V shape similar to what Greed Heads do. The bullets are fired on the sides of the cone.
    * For `mode = 2` and `mode = 3` the bullets will be fired in a V shape plus an additional bullet in the direction defined by `Velocity`. This is similar to the tears of Keeper in Repentance (`mode = 2`) and the tears shot by Greed (`mode = 3`).
    * For `mode = 4`, the bullets will be fired in two different V shapes, one wide, and one narrow (basically a V inside another V). The edges of the outer V are the edges of the cone. The edges of the inner V are obtained by bisecting each of the angle formed by (Velocity, OuterEdge). This is similar to the tears shot by Super Greed.
    * For `mode = 5`, the bullets will be fired in two different V shapes, one wide, and one narrow, plus an additional bullet in the direction defined by `Velocity`. This is similar to `mode = 4` plus an additional bullet. This is similar to the five tear shot of Ultra Greed.

    **Notes regarding ProjectileMode 9**

    When using ProjectileMode 9 (circle), the game will check `params.CircleAngle` (in **radians**) to determine the direction in which to fire the first bullet in the circle. Each subsequent bullet will be offset by `k * 2 * PI / N` radians, with `k` the number - 1 of the projectile (so the second projectile will have a direction of `1 * 2 * PI / N`, the third a direction of `2 * 2 * PI / N` and so on). `params.FireDirectionLimit` and `params.DotProductLimit` are used to determine which projectiles will actually spawn. For each of the attempted spawn, compute a normalized vector `V` oriented in the direction the projectile would follow. Next, compute the dot product of `V` with `params.FireDirectionLimit`. If the result of the dot product is **STRICTLY higher** than `params.DotProductLimit`, then the projectile is allowed to spawn, otherwise it is skipped.

    For instance, let's say that `params.CircleAngle` is 0, and 4 projectiles are spawned. This will result in projectiles being fired in a + pattern. The four direction vectors are `(1, 0)`, `(0, 1)`, `(-1, 0)` and `(0, -1)`. Let `params.DireDirectionLimit = Vector(2, 0)`, and `params.DotProductLimit = 0`. Let `*` denote the dot product. 

    * `(1, 0) * (2, 0) = 2`
    * `(0, 1) * (2, 0) = 0`
    * `(-1, 0) * (2, 0 = -2`
    * `(0, -1) * (2, 0) = 0`.

    Only the first projectile will be allowed to spawn, because all other dot products are lower than `params.DotProductLimit`. Note that if you do reproduce this example, the second and/or fourth projectiles may also be allowed to spawn because of the precision of floating point numbers (computing the second dot product in game yields 1.22e-16 which is greather than 0, computing the fourth yields -3.67e-16 which is lower than 0).
___
### Get·Alive·Enemy·Count () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### int GetAliveEnemyCount ( ) {: .copyable aria-label='Functions' }
Used to redirect close door enemies to any enemies for friendly npcs.
___
### Get·Boss·Color·Idx () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### int GetBossColorIdx ( ) {: .copyable aria-label='Functions' }


???- note "Notes"
    This will return the boss color idx reduced by 1. To get the actual color as set in bosscolors.xml, add +1 to the result.
___
### Get·Champion·Color·Idx () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### int GetChampionColorIdx ( ) {: .copyable aria-label='Functions' }


???- note "Notes"
    A list of Champion colors can be found here : [ChampionColorIdx](enums/ChampionColor.md)
___
### Get·Player·Target () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### [Entity](Entity.md) GetPlayerTarget ( ) {: .copyable aria-label='Functions' }
if there are no modifiers (best friend) this will return the player
___
### Is·Boss () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### boolean IsBoss ( ) {: .copyable aria-label='Functions' }

___
### Is·Champion () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### boolean IsChampion ( ) {: .copyable aria-label='Functions' }

___
### Kill·Unique () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void KillUnique ( ) {: .copyable aria-label='Functions' }
For entities with unique death animation, like Flush! vs poop enemies.
___
### Make·Champion () {: aria-label='Functions' }
[ ](#){: .rep .tooltip .badge }
#### void MakeChampion ( int Seed, ChampionColor ChampionColorIdx = -1, boolean Init = false ) {: .copyable aria-label='Functions' }
Forces a non champion to become a champion, resets hp to max hp.

**ChampionColorIdx**: The type of champion to turn this enemy into (-1 results in a random champion type)

**Init**: Set to true when called while initializing the enemy, false otherwise
___
### Make·Splat () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### [EntityEffect](EntityEffect.md) MakeSplat ( float Size ) {: .copyable aria-label='Functions' }

___
### Morph () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### boolean Morph ( [EntityType](enums/EntityType.md) type, int Variant, int SubType, int ChampionColorIdx ) {: .copyable aria-label='Functions' }

Morph the current entity into another one. [ChampionColorIdx](https://bindingofisaacrebirth.gamepedia.com/Monsters#Champions) can be used to turn the entity into a champion. Use `:::lua -1` to turn it into a regular entity.
A list of Champion colors can be found here : [ChampionColorIdx](https://bindingofisaacrebirth.gamepedia.com/Monsters#Champions)

???- example "Example Code"
    This code turns an entity into a gaper.
    ```lua
    entity:ToNPC():Morph(EntityType.ENTITY_GAPER , 0, 0,-1)
    ```
___
### Play·Sound () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void PlaySound ( [SoundEffect](enums/SoundEffect.md) ID, float Volume, int FrameDelay, boolean Loop, float Pitch ) {: .copyable aria-label='Functions' }

___
### Query·NPCs·Group () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### [EntityList](CppContainer_EntityList.md) QueryNPCsGroup ( int GroupIdx ) {: .copyable aria-label='Functions' }

___
### Query·NPCs·Spawner·Type () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### [EntityList](CppContainer_EntityList.md) QueryNPCsSpawnerType ( [EntityType](enums/EntityType.md) SpawnerType, [EntityType](enums/EntityType.md) Type, boolean OnlyEnemies ) {: .copyable aria-label='Functions' }

___
### Query·NPCs·Type () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### [EntityList](CppContainer_EntityList.md) QueryNPCsType ( [EntityType](enums/EntityType.md) Type, int Variant ) {: .copyable aria-label='Functions' }

___
### Reset·Path·Finder·Target () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void ResetPathFinderTarget ( ) {: .copyable aria-label='Functions' }

___
### Throw·Spider () {: aria-label='Functions' }
[ ](#){: .static .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### static const [EntityNPC](EntityNPC.md) ThrowSpider ( [Vector](Vector.md) Position, [Entity](Entity.md) Spawner, [Vector](Vector.md) TargetPos, boolean Big, float YOffset ) {: .copyable aria-label='Functions' }

___
## Variables
### Can·Shut·Doors {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean CanShutDoors  {: .copyable aria-label='Variables' }

___
### Child·NPC {: aria-label='Variables' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const [EntityNPC](EntityNPC.md) ChildNPC  {: .copyable aria-label='Variables' data-altreturn='nil' }

___
### Entity·Ref {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Entity](Entity.md) EntityRef {: .copyable aria-label='Variables' }

___
### Group·Idx {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int GroupIdx  {: .copyable aria-label='Variables' }
Used to identify multichunks groups.
___
### I1 {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int I1  {: .copyable aria-label='Variables' }
General usage int for AI specific actions. The effect and usage is manually defined for each entity. It can also not be used at all for some.

**Example**: The Frail sets I2 to 1 when entering the second phase.
___
### I2 {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int I2  {: .copyable aria-label='Variables' }
General usage int for AI specific actions. The effect and usage is manually defined for each entity. It can also not be used at all for some.

**Example**: The Frail sets I2 to 1 when entering the second phase.
___
### Parent·NPC {: aria-label='Variables' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const [EntityNPC](EntityNPC.md) ParentNPC  {: .copyable aria-label='Variables' data-altreturn='nil' }
parent entity, for multi-entity NPCs like Larry Jr.
___
### Pathfinder {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [PathFinder](PathFinder.md) Pathfinder  {: .copyable aria-label='Variables' }

___
### Projectile·Cooldown {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int ProjectileCooldown  {: .copyable aria-label='Variables' }
projectiles can fire again when it reaches 0
___
### Projectile·Delay {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int ProjectileDelay  {: .copyable aria-label='Variables' }
&gt;0: projectile will be fired in n frames
___
### Scale {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float Scale  {: .copyable aria-label='Variables' }

___
### State {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int State  {: .copyable aria-label='Variables' }

___
### State·Frame {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int StateFrame  {: .copyable aria-label='Variables' }

___
### V1 {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) V1  {: .copyable aria-label='Variables' }
General usage Vector for AI specific actions. Initialized to be Vector(0,0). The effect and usage is manually defined for each entity. It can also not be used at all for some.
___
### V2 {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) V2  {: .copyable aria-label='Variables' }
General usage Vector for AI specific actions. Initialized to be Vector(0,0). The effect and usage is manually defined for each entity. It can also not be used at all for some.
___
