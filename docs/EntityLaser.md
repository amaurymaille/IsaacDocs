# Class "EntityLaser"

### Inherits from Class: {: .inheritance }
[Entity](Entity.md)

## Summary

An `EntityLaser` represents a laser, such as the ones shot by Brimstone, Technology, Jacob's Ladder or Montezuma's Revenge. A laser is defined by its origin (referred to as the _source position_ in the docs) and its end point: it appears as a straight line from source position to end point.

Lasers can be created through multiple methods:

* [EntityPlayer::FireBrimstone](EntityPlayer.md#FireBrimstone) which fires a Brimstone
* [EntityPlayer::FireDelayedBrimstone](EntityPlayer.md#FireDelayedBrimstone) which fires a Brimstone too
* [EntityPlayer::FireTechLaser](EntityPlayer.md#FireTechLaser) which fires a Technology laser
* [EntityPlayer::FireTechXLaser](EntityPlayer.md#FireTechXLaser) which fires a Tech X. laser
* [EntityPlayer::SpawnMawOfTheVoid](EntityPlayer.md#SpawnMawOfTheVoid) which spawns a Maw of the Void ring
* [EntityLaser.ShootAngle](#ShootAngle) (static method) which fires a template laser and is the most generic way of creating a laser

A laser can have an optional **source**, which must be an [Entity](Entity.md). If a laser has a source, it will move in sync with its source, replicating its movements (for instance, take the Vis enemy: when it gets hit by tears, it moves a little, and the laser it is shooting is also moved). Note that if a source is specified, it does not need to be the origin of the laser. For instance, the Brimstone ring obtained by combining Brimstone and The Ludovico Technique has the player as its source, but its origin is definitely not the player, but rather one of the points that the game uses to compute the circle.

Similarly to [Tears](EntityTear.md), a laser can have _flags_ that will alter its behaviour: is the laser spectral (goes over grid entities) ? Is it piercing (does it stop when it hits an entity or does it goes through it) ? See [TearFlags](enums/TearFlags.md) for a more comprehensive list of flags.

Unlike tears whose lifetime is based on their _height_, lasers don't have a concept of height. Rather, their duration is based on a _timeout_, defined as the number of logic frames (30 logic frames in a second) during which the laser is active. If the laser has a source configured, then the laser may be removed early if its source entity dies. For instance, the Brimstone fired by the Vis enemy disappears when the Vis dies, regardless of the remaining timeout. On the other hand, Brimstone lasers fired by Brimstone grimace in Sheol and Gehenna are always active for the entirety of their timeout as these entities cannot be destroyed (unless the player has Euthanasia, of course).


## Functions
### Add·Tear·Flags () {: aria-label='Functions' }
[ ](#){: .rep .tooltip .badge }
#### void AddTearFlags ( [TearFlags](enums/TearFlags.md) Flags ) {: .copyable aria-label='Functions' }

Adds `Flags` to the flags of this laser. The game performs a bitwise OR to add the flags.

___
### Calculate·End·Point () {: aria-label='Functions' }
[ ](#){: .static .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### static [Vector](Vector.md) CalculateEndPoint ( [Vector](Vector.md) Start, [Vector](Vector.md) Dir, [Vector](Vector.md) PositionOffset, [Entity](Entity.md) Parent, float Margin ) {: .copyable aria-label='Functions' }

___
### Clear·Tear·Flags () {: aria-label='Functions' }
[ ](#){: .rep .tooltip .badge }
#### void ClearTearFlags ( [TearFlags](enums/TearFlags.md) Flags ) {: .copyable aria-label='Functions' }

Remove `Flags` from the flags of this laser. The game performs a bitwise NAND to remove the flags.

___
### Get·End·Point () {: aria-label='Functions' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const [Vector](Vector.md) GetEndPoint ( ) {: .copyable aria-label='Functions' }

Return the end point of the laser.

___
### Get·Non·Optimized·Samples () {: aria-label='Functions' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const HomingLaser::SampleList GetNonOptimizedSamples ( ) {: .copyable aria-label='Functions' }

Return the list of vectors the game used to compute the initial trajectory of the laser. For non homing lasers, this list contains two elements, the origin and the end point. For homing lasers that have a continuous appearance (Vis-Versa Brimstone, The Gate's Brimstone), this contains the origin, the list of all intermediate points used to interpolate the curve, and the end point. For homing lasers without a continuous appearance, like Hush's homing pillars of tears, this list contains the origin and end point. For circular lasers, like Ludovico + Brimstone, the list contains an origin point on the ring as well as the list of all intermediate points used to interpolate the circle.

???+ bug "Bugs"
    Contrary to what the documentation provided by Nicalis states, this function does not return a list of `Sample` but a list of `Vector`. This list can be iterated over in the same way as an [EntityList](CppContainer_EntityList.md) or any list of this kind, using the `#` operator to get its length and `Get(int index)` to retrieve the element at a given position.

___
### Get·Render·Z () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### int GetRenderZ ( ) {: .copyable aria-label='Functions' }

___
### Get·Samples () {: aria-label='Functions' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const HomingLaser::SampleList GetSamples ( ) {: .copyable aria-label='Functions' }

Return the list of vectors the game used to compute the refined trajectory of the laser. This is similar to `GetNonOptimizedSamples`, except it contains the points used for a more refined interpolation that smoothens the curve of the laser.

___
### Has·Tear·Flags () {: aria-label='Functions' }
[ ](#){: .rep .tooltip .badge }
#### boolean HasTearFlags ( [TearFlags](enums/TearFlags.md) Flags ) {: .copyable aria-label='Functions' }

Check if the laser's flags contains `Flags`. Returns true if and only if all flags specified in `Flags` are present.

___
### Is·Circle·Laser () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### boolean IsCircleLaser ( ) {: .copyable aria-label='Functions' }

???- note "Note"
    This function cannot differentiate between different types of Circle Laser, however these may be identified by their SubType:
  
    * 0 - Linear Laser (Typical laser with a start and end point)
    * 1 - Ring Ludovico (Controlled laser ring for Ludo synergies)
    * 2 - Ring Projectile (Tech X)
    * 3 - Ring Follow Parent (Maw of the Void)
    * 4 - No Impact (No impact splash, e.g. Tech Zero)
 
___
### Is·Sample·Laser () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### boolean IsSampleLaser ( ) {: .copyable aria-label='Functions' }

Check if the laser uses sampling, which is used to define custom trajectories.

___
### Set·Active·Rotation () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void SetActiveRotation ( int Delay, float AngleDegrees, float RotationSpd, boolean TimeoutComplete ) {: .copyable aria-label='Functions' }

___
### Set·Black·Hp·Drop·Chance () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void SetBlackHpDropChance ( float Chance ) {: .copyable aria-label='Functions' }

___
### Set·Homing·Type () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void SetHomingType ( LaserHomingType Type ) {: .copyable aria-label='Functions' }

Defines homing properties on the laser.

???+ note "**On the `Type` value**"

    `LaserHomingType` is an opaque userdata, however the game will perfectly accept integer values instead. 
    
    * `1`: the laser follows its source by becoming thiner, like a reverse Montezuma's Revenge.
    * `2`: make the laser home in on enemies.

???+ bug "Bugs"
    
    * The documentation provided by Nicalis indicates that this function expectes a `LaserHomingType`. This type is an opaque userdata and is therefore unusable.
    * However, as the type was thought to represent an integer, the C++ code expects an integer to be provided as parameter, but most values won't do anything.
    * It is possible to make the laser homing by giving it the `TEAR_HOMING` flag (although this will also color the laser purple), making this function redundant.
___
### Set·Max·Distance () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void SetMaxDistance ( float Distance ) {: .copyable aria-label='Functions' }

___
### Set·Multidimensional·Touched () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void SetMultidimensionalTouched ( boolean Value ) {: .copyable aria-label='Functions' }

___
### Set·One·Hit () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void SetOneHit ( boolean Value ) {: .copyable aria-label='Functions' }

___
### Set·Timeout () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### void SetTimeout ( int Value ) {: .copyable aria-label='Functions' }

Configure the timeout of the laser to `Value`, expressed as a number of logic frames.

___
### Shoot·Angle () {: aria-label='Functions' }
[ ](#){: .static .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### static [EntityLaser](EntityLaser.md) ShootAngle ( int Variant, [Vector](Vector.md) SourcePos, float AngleDegrees, int Timeout, [Vector](Vector.md) PosOffset, [Entity](Entity.md) Source ) {: .copyable aria-label='Functions' }

Static function used to spawn lasers. The `Variant` parameter defines the kind of laser to spawn. `SourcePos` defines the point of origin of the laser. `AngleDegrees` defines the orientation (in degrees) of the laser (0 is to the right, 90 is down, 180 is to the left, -90 is up). `Timeout` defines the duration of the laser in logic frames (30 logic frames in a second). `PosOffset` can be used to offset the source of the laser. `Source` is the [Entity](Entity.md) that spawns the laser. 

The spawned laser will not be active in the same frame it is spawned in, you'll have to wait until the next logic frame before seeing it on screen. This allows you to tune the extra parameters of the laser before it is actually processed by the game, such as its curve or its trajectory.

The `PosOffset` vector is added to the `SourcePos` vector in order to define where the laser will be **rendered**. However, the hitbox of the laser will actually be computed by taking only `SourcePos` into account. This is used, for instance, with Technology 2. While the laser does come from Isaac's Eye, its hitbox starts a bit away from Isaac. To recreate that in game, you would put the `SourcePos` a bit away from Isaac, and configure the distance between Isaac and the `SourcePos` as the `PosOffset`.

???+ example Example
    ```lua
    -- Shoot a Technology 2 like laser to the right (Variant = 2, see notes below), offset it to Isaac's feet (height cannot be configured).
    local isaac = Game():GetPlayer(0)
    local isaacPosition = isaac.Position
    local positionOffset = Vector(10, 0)
    local spawnPosition = isaacPosition + positionOffset
    local laser = EntityLaser.Shoot(2, spawnPosition, 0, 30, -positionOffset, isaac)
    ```

Specifying a `Source` will cause the laser to move in sync with said source; otherwise the laser will not be synced with any particular entity in the room and will remain static. Specifying a `Source` will make the source entity immune to damage from the laser, and may affect the hitbox of the laser (see the notes on `Variant` for more details).

???+ note Notes
    **On the `Variant` parameter**

    Laser variants are not defined through enums and are therefore undocumented. Here are some of them (as of Repentance; some of them might be unavailable in Afterbirth+):

    * `1`: "Classic" Brimstone, as if you picked the item or used Sulfur
    * `2`: Technology laser, as if you picked Technology 2. If the `Source` is not configured, the hitbox will extend all the way up to the `SourcePos`. If the `Source` is not nil, then the hitbox will be slightly offset forward in a direction of `AngleDegrees` to prevent immediate damage.
    * `3`: Trisagion laser.
    * `4`: Pride laser.
    * `5`: Angels / Revelation laser. If not source is provided, hitbox starts at `SourcePos`. If source is provided, the hitbox is offset forward in the direction the laser is facing.
    * `6`: Mega Satan laser. Screen will shake for the whole duration.
    * `7`: Tractor Beam laser. Does not deal damage regardless of which entity is the source, or whether source is specified or not.
    * `8`: Beam of light that spawns with Brimstone sound effect, internally referred to a "Light Ring". Hitbox offset if the player is the source.
    * `9`: Brimstone beam, except the blood swirls, like Hush's pillars of tears attack.
    * `10`: Jacob's Ladder / 120 Volt electricity.
    * `11`: Wide Brimstone (as if Brimstone + Sulfur, or double Sulfur in the same room). Screen shakes upon fire.
    * `12`: Montezuma's Revenge.
    * `13`: The Beast's Brimstone. Hitbox will be massively offset if source is provided.
    * `14`: Wide version of Hush's laser (`9` with the width and screen shake effect of `11`.
    * `15`: Mega Satan version of Hush's laser (`9` with the width and continuous screen shake effet of `6`.

    A variant of a laser doesn't have the inherent properties of the item / entity it is based on. For instance, spawning a Tractor Beam laser (variant `7`) and giving it a `Source` will not cause the tears shot by that `Source` to follow the laser. It is purely cosmetic. Similarly, shooting a Hush laser will not cause it to go up in the air and fall to the ground, this is a behaviour you have to define yourself. As a final example, a Montezuma's Revenge laser will not erupt in the opposite direction compared to what you've specified, nor will it be short and trail behind its source: it is simply a brown colored Brimstone laser. What each of these variants does is define a template: what the laser will look like (its sprite), and how wide it is. If you want to give the laser some interesting properties (max distance, homing, moving in sync behind the player, shoot into the air and fall on the ground and so on), you need to script that behaviour later. If you want to make the laser wider or narrower, use the `Size` property to change its hitbox and then adapt its sprite (changing the `Size` only changes the hitbox, not the sprite).

???+ bug Bugs
    Specifying an invalid variant will cause the game to crash on the next logic frame. It is recommanded to write your own version of this function that performs a validation of the variant parameter and simply returns `nil` if the variant is invalid, like so:
    ```lua
    function ShootAngle(variant, sourcePos, angleDegrees, timeout, posOffset, source)
        if variant < 1 or variant > 15 then
            return nil
        else
            return EntityLaser.ShootAngle(variant, sourcePos, angleDegrees, timeout, posOffset, source)
        end
    end
    ```
___
## Variables
### Angle {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float Angle  {: .copyable aria-label='Variables' }

Angle of the laser in degrees. 0° is to the right, 90° down, 180° to the left, -90° up.

___
### Angle·Degrees {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float AngleDegrees  {: .copyable aria-label='Variables' }

Same as `Angle`.

___
### Black·Hp·Drop·Chance {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float BlackHpDropChance  {: .copyable aria-label='Variables' }
For maw of void.
___
### Bounce·Laser {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Entity](Entity.md) BounceLaser  {: .copyable aria-label='Variables' data-altreturn='nil' }

___
### Curve·Strength {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float CurveStrength  {: .copyable aria-label='Variables' }
My Reflection.

???+ bug "Bug"
    The value seems to always be 0.0 in game, regardless of the presence of My Reflection or not.

___
### Disable·Follow·Parent {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean DisableFollowParent  {: .copyable aria-label='Variables' }
Set on children of other lasers, for instance Rubber Cement reflections. Disables m_ParentOffset.
___
### End·Point {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) EndPoint  {: .copyable aria-label='Variables' }
Will hold the endpoint so it will not need to be recalculated when accessed from extern.
___
### First·Update {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean FirstUpdate  {: .copyable aria-label='Variables' }

___
### Grid·Hit {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean GridHit  {: .copyable aria-label='Variables' }
true if laser can be clipped by grid entities and it was clipped at that frame.
___
### Homing·Laser {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### HomingLaser HomingLaser  {: .copyable aria-label='Variables' }

If the laser is homing, this will hold a reference to the `HomingLaser` object representing the laser.

???+ bug "Bug"
    `HomingLaser` is an opaque userdata with no metatable, making this value unusable.

___
### Homing·Type {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### LaserHomingType HomingType  {: .copyable aria-label='Variables' }

Specify the king of homing the laser uses.

???+ bug "Bug"
    `LaserHomingType` should be an integer, but the game implements it as an opaque userdata with no metatable, making this value unusable.

___
### Is·Active·Rotating {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean IsActiveRotating  {: .copyable aria-label='Variables' }

___
### Laser·Length {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float LaserLength  {: .copyable aria-label='Variables' }

___
### Last·Angle·Degrees {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float LastAngleDegrees  {: .copyable aria-label='Variables' }

___
### Max·Distance {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float MaxDistance  {: .copyable aria-label='Variables' }
Used to trim brimstone for Azazel (0 - off)
___
### One·Hit {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean OneHit  {: .copyable aria-label='Variables' }
Laser hits only once.
___
### Parent·Offset {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [Vector](Vector.md) ParentOffset  {: .copyable aria-label='Variables' }

___
### Radius {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float Radius  {: .copyable aria-label='Variables' }

___
### Rotation·Degrees {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float RotationDegrees  {: .copyable aria-label='Variables' }

___
### Rotation·Delay {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int RotationDelay  {: .copyable aria-label='Variables' }

___
### Rotation·Spd {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float RotationSpd  {: .copyable aria-label='Variables' }

___
### Sample·Laser {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean SampleLaser  {: .copyable aria-label='Variables' }

???+ bug "Bugs"
    
    * Contrary to what the documentation provided by Nicalis indicates, this function returns a userdata and not a boolean.
    * Also, this userdata is opaque with no metatable, and is therefore unusable.

___
### Shrink {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### boolean Shrink  {: .copyable aria-label='Variables' }

___
### Start·Angle·Degrees {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float StartAngleDegrees  {: .copyable aria-label='Variables' }
Some lasers have a bit of random variation in rotation so they need to remember their starting point.
___
### Tear·Flags {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### [TearFlags](enums/TearFlags.md) TearFlags  {: .copyable aria-label='Variables' }

Bitmask of the flags that are active on this laser.

___
### Timeout {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int Timeout  {: .copyable aria-label='Variables' }

Number of logic frames before the laser disappears.

___
