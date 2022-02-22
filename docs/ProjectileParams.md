# Class "ProjectileParams"

A `ProjectileParams` defines multiple parameter on a `Projectile`. A `Projectile` (see [EntityProjectile](EntityProjectile.md)) is a bullet spawned by an NPC ([EntityNPC](EntityNPC.md)). `Projectile`s have a velocity (a speed as well as a direction) as well as a height (how far from the ground they are), and a falling speed (how quickly they fall to the ground) and falling acceleration (a rate at which their falling speed is updated each frame). 

Instances of this class are used essentially as parameters to the [EntityNPC::FireProjectiles](EntityNPC.md#fireprojectiles) and [EntityNPC::FireBossProjectiles](EntityNPC.md#firebossprojectiles) functions. 

Parameters on a projectile can define its movement, for instance orbiting their spawner, moving in angles, increasing and decreasing in speed etc.  They can also be used to define properties, like looping through walls (Hush's Continuum tears), going over rocks (spectral tears), hit other enemies (Mom's Heart tears) and so on. See [ProjectileFlags](enums/ProjectileFlags.md) for a more complete list.

`ProjectileParams` are only used when creating a `Projectile`. The game only uses them for the initial parameterization of the projectiles it creates, they can be safely discarded afterwards. Since you have access to the projectiles through [EntityProjectile](EntityProjectile.md), you can always change the parameters of your projectiles after their creation. In fact, some things, like precise control over movement, can only be achieved by directly interacting with the spawned projectile. 

Unlike many other classes whose instance are obtained through calls to methods, `ProjectileParams` are always created by the modder, through the use of the global function `ProjectileParams()` that creates a brand new `ProjectionParams` that is initialized with default values. 

Example use :

```lua
function mod:SpawnSomeProjectiles()
    -- Assume there is a dummy in the room
    local spawner = Isaac.FindByType(EntityType.ENTITY_DUMMY)[1]:ToNPC()
    -- Create parameters for the projectiles that will spawn
    local params = ProjectileParams()    
    -- Configure them : explosive, continuum, homing shot. Spread them because we want to spawn multiple.
    params.BulletFlags = ProjectileFlags.EXPLODE | ProjectileFlags.SMART | ProjectileFlags.CONTINUUM
    params.Spread = 2
    -- FIRE THE MISSILES!
    spawner:FireProjectiles(spawner.Position, Vector(4, 0), 1, params)
end
```

## Constructors
### Projectile·Params () {: aria-label='Constructors' }
[ ](#){: .abrep .tooltip .badge }
#### [ProjectileParams](ProjectileParams.md) ProjectileParams ( ) {: .copyable aria-label='Constructors' }

___
## Variables
### Acceleration {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float Acceleration  {: .copyable aria-label='Variables' }

___
### Bullet·Flags {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int BulletFlags  {: .copyable aria-label='Variables' }

A bitmask of the desired [ProjectileFlags](enums/ProjectileFlags.md) you want on this new projectile. These can later be changed by accessing the [EntityProjectile](EntityProjectile.md) and using [EntityProjectile.ProjectileFlags](EntityProjectile.md#projectileflags).

___
### Change·Flags {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int ChangeFlags  {: .copyable aria-label='Variables' }

Uses [ProjectileFlags](enums/ProjectileFlags.md) to define the projectile attributes after the "Changed" state was activated.
The [ProjectileFlag](enums/ProjectileFlags.md).CHANGE_FLAGS_AFTER_TIMEOUT needs to be set to allow for this change to apply!
____
**Informations about "Changed" State:**

Projectiles can have two states: normal (default) and changed.


Changed state activates when projectile's frame count reaches the value set in [ChangeTimeout](#changetimeout). After that its flags get changed to what was set in [ChangeFlags](#changeflags) and velocity will be resized to length set in [ChangeVelocity](#changevelocity).
____
Also used in: [EntityProjectile](EntityProjectile.md)
___
### Change·Timeout {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int ChangeTimeout  {: .copyable aria-label='Variables' }

Number of frames that need to elapse after spawn till the "Changed" state is activated.
The [ProjectileFlag](enums/ProjectileFlags.md).CHANGE_FLAGS_AFTER_TIMEOUT or CHANGE_VELOCITY_AFTER_TIMEOUT need to be set to allow for this change to apply!
____
**Informations about "Changed" State:**

Projectiles can have two states: normal (default) and changed.


Changed state activates when projectile's frame count reaches the value set in [ChangeTimeout](#changetimeout). After that its flags get changed to what was set in [ChangeFlags](#changeflags) and velocity will be resized to length set in [ChangeVelocity](#changevelocity).
____
Also used in: [EntityProjectile](EntityProjectile.md)
___
### Change·Velocity {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float ChangeVelocity  {: .copyable aria-label='Variables' }

Velocity value that gets applied when the "Changed" state is activated.
The [ProjectileFlag](enums/ProjectileFlags.md).CHANGE_VELOCITY_AFTER_TIMEOUT need to be set to allow for this change to apply!
____
**Informations about "Changed" State:**

Projectiles can have two states: normal (default) and changed.


Changed state activates when projectile's frame count reaches the value set in [ChangeTimeout](#changetimeout). After that its flags get changed to what was set in [ChangeFlags](#changeflags) and velocity will be resized to length set in [ChangeVelocity](#changevelocity).
____
Also used in: [EntityProjectile](EntityProjectile.md)
___
### Circle·Angle {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float CircleAngle  {: .copyable aria-label='Variables' }
Angle offset used by [EntityNPC::FireProjectiles](EntityNPC.md#fireprojectiles) with `mode = 9`. Random by default. This value is in **radians**. It defines the direction in which the first bullet of the circle will move. All subsequent bullets that are part of this circle will move in a direction that is `CircleAngle + k * 2 * PI / N`, with `N` the amount of tears in the circle.
___
### Color {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Color](Color.md) Color  {: .copyable aria-label='Variables' }

___
### Curving·Strength {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float CurvingStrength  {: .copyable aria-label='Variables' }
Use very small values for curving like 0.005.
___
### Depth·Offset {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float DepthOffset  {: .copyable aria-label='Variables' }

___
### Dot·Product·Limit {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float DotProductLimit  {: .copyable aria-label='Variables' }
Used when projectiles are spawned through [EntityNPC::FireProjectiles](EntityNPC.md#fireprojectiles) with `mode = 9`. Is part of a complex verification system that allows to restrict only some bullets of the circle to spawn.
___
### Falling·Accel·Modifier {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float FallingAccelModifier  {: .copyable aria-label='Variables' }

___
### Falling·Speed·Modifier {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float FallingSpeedModifier  {: .copyable aria-label='Variables' }

___
### Fire·Direction·Limit {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) FireDirectionLimit  {: .copyable aria-label='Variables' }

Used when projectiles are spawned through [EntityNPC::FireProjectiles](EntityNPC.md#fireprojectiles) with `mode = 9`. Is part of a complex verification system that allows to restrict only some bullets of the circle to spawn.
___
### Grid·Collision {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean GridCollision  {: .copyable aria-label='Variables' }

___
### Height·Modifier {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float HeightModifier  {: .copyable aria-label='Variables' }

Flat modifier to the original height of a spawned bullet. The resulting height will be the original height selected by the game + this value. Note that height is, counter-intuitively, higher the closer a projectile is to the ground. Most projectiles start at a height of roughly -25, and disappear once their height gets close to 0. A projectile with a height below -50 doesn't have its hitbox enabled (check the Height value of the projectiles spawned by Red Mega Fatty for an example).
___
### Homing·Strength {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float HomingStrength  {: .copyable aria-label='Variables' }
Multiplier on normal homing strength. Unused if SMART bullet flag is not set.
___
### Position·Offset {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) PositionOffset  {: .copyable aria-label='Variables' }

___
### Scale {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float Scale  {: .copyable aria-label='Variables' }

___
### Spread {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float Spread  {: .copyable aria-label='Variables' }
For quad/quint/etc spread shots. This value does not follow a linear progression, a `Spread` of `2` will not result in shots being spread twice as far apart than with a `Spread` of 1. 
___
### Target·Position {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) TargetPosition  {: .copyable aria-label='Variables' }

A position that has meaning for some flags, and is completely ignored for others. For instance, if the projectile has an orbitting flag, this position defines the point the projectile is orbitting.

___
### Variant {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int Variant  {: .copyable aria-label='Variables' }

___
### Velocity·Multi {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float VelocityMulti  {: .copyable aria-label='Variables' }

___
### Wiggle·Frame·Offset {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int WiggleFrameOffset  {: .copyable aria-label='Variables' }
Used to offset the wiggle wave.
___
