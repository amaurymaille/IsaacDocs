# Class "EntityLaser"
### Inherits from Class: {: .inheritance }
[Entity](Entity.md)
## Functions
### Add·Tear·Flags () {: aria-label='Functions' }
[ ](#){: .rep .tooltip .badge }
#### void AddTearFlags ( [TearFlags](enums/TearFlags.md) Flags ) {: .copyable aria-label='Functions' }

___
### Calculate·End·Point () {: aria-label='Functions' }
[ ](#){: .static .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### static [Vector](Vector.md) CalculateEndPoint ( [Vector](Vector.md) Start, [Vector](Vector.md) Dir, [Vector](Vector.md) PositionOffset, [Entity](Entity.md) Parent, float Margin ) {: .copyable aria-label='Functions' }

___
### Clear·Tear·Flags () {: aria-label='Functions' }
[ ](#){: .rep .tooltip .badge }
#### void ClearTearFlags ( [TearFlags](enums/TearFlags.md) Flags ) {: .copyable aria-label='Functions' }

___
### Get·End·Point () {: aria-label='Functions' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const [Vector](Vector.md) GetEndPoint ( ) {: .copyable aria-label='Functions' }

___
### Get·Non·Optimized·Samples () {: aria-label='Functions' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const HomingLaser::SampleList GetNonOptimizedSamples ( ) {: .copyable aria-label='Functions' }

___
### Get·Render·Z () {: aria-label='Functions' }
[ ](#){: .abrep .tooltip .badge }
#### int GetRenderZ ( ) {: .copyable aria-label='Functions' }

___
### Get·Samples () {: aria-label='Functions' }
[ ](#){: .const .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### const HomingLaser::SampleList GetSamples ( ) {: .copyable aria-label='Functions' }

___
### Has·Tear·Flags () {: aria-label='Functions' }
[ ](#){: .rep .tooltip .badge }
#### boolean HasTearFlags ( [TearFlags](enums/TearFlags.md) Flags ) {: .copyable aria-label='Functions' }

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

___
### Shoot·Angle () {: aria-label='Functions' }
[ ](#){: .static .tooltip .badge } [ ](#){: .abrep .tooltip .badge }
#### static [EntityLaser](EntityLaser.md) ShootAngle ( int Variant, [Vector](Vector.md) SourcePos, float AngleDegrees, int Timeout, [Vector](Vector.md) PosOffset, [Entity](Entity.md) Source ) {: .copyable aria-label='Functions' }

Static function used to spawn lasers. The `Variant` parameter defines the kind of laser to spawn. `SourcePos` defines the point of origin of the laser. `AngleDegrees` defines the orientation (in degrees) of the laser. `Timeout` defines the duration of the laser in logic frames (30 logic frames in a second). `PosOffset` can be used to offset the source of the laser. `Source` is the [Entity](Entity.md) that spawns the laser. 

The spawned laser will not be active in the same frame it is spawned in, you'll have to wait one logic frame before seeing it on screen. This allows you to tune the extra parameters of the laser before it is actually processed by the game, such as its curve or its trajectory. For instance, The Gate's Brimstone homes in on all entities on screen, this is something you can achieve by configuring your laser after spawning it, and before rendering it, which will make it look smooth.

The `PosOffset` vector is added to the `SourcePos` vector in order to define where the laser will be **rendered**. However, the hitbox of the laser will actually be computed by taking only `SourcePos` into account. This is used, for instance, with Technology 2. While the laser does come from Isaac's Eye, its hitbox starts a bit away from Isaac. To recreate that in game, you would put the `SourcePos` a bit away from Isaac, and offset that into Isaac.

???+ example Example
    ```lua
    -- Shoot a Technology 2 like laser to the right (Variant = 2, see notes below), offset it to Isaac's head
    local isaac = Game():GetPlayer(0)
    local isaacPosition = isaac.Position
    local positionOffset = Vector(10, 0)
    local spawnPosition = isaacPosition + positionOffset
    local laser = EntityLaser.Shoot(2, spawnPosition, 0, 30, -positionOffset, isaac)
    ```

Specifying a `Source` will cause the laser to move in sync with said source; otherwise the laser will not be synced with any particular entity in the room and will remain static. If a `Source` is specified, the game will consider an additional hitbox that will cause targets to take damage at point blank range, which is not the case with a static laser (use the console command `debug 6` to see the difference).

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

???+ bug Bugs
    Specifying an invalid variant will cause the game to crash on the next logic frame. It is recommanded to write your own version of this function that performs a validation of the variant parameter and simply returns `nil` instead, like so:
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

___
### Angle·Degrees {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### float AngleDegrees  {: .copyable aria-label='Variables' }

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

___
### Homing·Type {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### LaserHomingType HomingType  {: .copyable aria-label='Variables' }

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
___
### Timeout {: aria-label='Variables' }
[ ](#){: .abrep .tooltip .badge }
#### int Timeout  {: .copyable aria-label='Variables' }

___
