# Class "EntityProjectile"
___ 
## void AddChangeFlags (integer Flags)

See <a class="el" href="#a6c5a69141dc132104776d0aa4ce8691e">ChangeFlags</a>.
___ 
## void AddFallingAccel (float Value)

___ 
## void AddFallingSpeed (float Value)

___ 
## void AddHeight (float Value)

___ 
## void AddProjectileFlags (integer Flags)

Uses <a class="el" href="group__enums.html#ga0302119ed82822df78af258ee457e6a6">ProjectileFlags</a> to define the projectile attributes.
___ 
## void AddScale (float Value)

___ 
## float Acceleration

___ 
## integer ChangeFlags

Uses <a class="el" href="group__enums.html#ga0302119ed82822df78af258ee457e6a6">ProjectileFlags</a> to define the projectile attributes after the "Changed" state was activated.
The <a class="el" href="group__enums.html#ga0302119ed82822df78af258ee457e6a6">ProjectileFlag</a> CHANGE_FLAGS_AFTER_TIMEOUT needs to be set to allow for this change to apply!
<hr/>
**Informations about "Changed" State:**<br/>Projectiles can have two states: normal (default) and changed.<br/>
Changed state activates when projectile's frame count reaches the value set in <a class="el" href="#adc75976b47b0121d4faf956ee61f2a8d">ChangeTimeout</a>. After that its flags get changed to what was set in <a class="el" href="#a6c5a69141dc132104776d0aa4ce8691e">ChangeFlags</a> and velocity will be resized to length set in <a class="el" href="#adf22f7bcbe0ffbd7346ede9431c83df1">ChangeVelocity</a>.
<hr/>
Also used in: <a class="el" href="class_projectile_params.html#a94280d115acf598bf9f751da3f815a8c">ProjectileParams()</a>
___ 
## integer ChangeTimeout

Number of frames that need to elapse after spawn till the "Changed" state is activated.
The <a class="el" href="group__enums.html#ga0302119ed82822df78af258ee457e6a6">ProjectileFlags</a> CHANGE_FLAGS_AFTER_TIMEOUT or CHANGE_VELOCITY_AFTER_TIMEOUT need to be set to allow for this change to apply!
<hr/>
**Informations about "Changed" State:**<br/>Projectiles can have two states: normal (default) and changed.<br/>
Changed state activates when projectile's frame count reaches the value set in <a class="el" href="#adc75976b47b0121d4faf956ee61f2a8d">ChangeTimeout</a>. After that its flags get changed to what was set in <a class="el" href="#a6c5a69141dc132104776d0aa4ce8691e">ChangeFlags</a> and velocity will be resized to length set in <a class="el" href="#adf22f7bcbe0ffbd7346ede9431c83df1">ChangeVelocity</a>.
<hr/>
Also used in: <a class="el" href="class_projectile_params.html#a6738cae72bddb5bbc087f215f7f08bd2">ProjectileParams()</a>
___ 
## float ChangeVelocity

Velocity value that gets applied when the "Changed" state is activated.
The <a class="el" href="group__enums.html#ga0302119ed82822df78af258ee457e6a6">ProjectileFlag</a> CHANGE_VELOCITY_AFTER_TIMEOUT need to be set to allow for this change to apply!
<hr/>
**Informations about "Changed" State:**<br/>Projectiles can have two states: normal (default) and changed.<br/>
Changed state activates when projectile's frame count reaches the value set in <a class="el" href="#adc75976b47b0121d4faf956ee61f2a8d">ChangeTimeout</a>. After that its flags get changed to what was set in <a class="el" href="#a6c5a69141dc132104776d0aa4ce8691e">ChangeFlags</a> and velocity will be resized to length set in <a class="el" href="#adf22f7bcbe0ffbd7346ede9431c83df1">ChangeVelocity</a>.
<hr/>
Also used in: <a class="el" href="class_projectile_params.html#a8d480667cf7ba94ee10bbb9dcc008c6f">ProjectileParams()</a>
___ 
## float CurvingStrength

___ 
## float Damage

___ 
## float DepthOffset

___ 
## float FallingAccel

___ 
## float FallingSpeed

___ 
## float Height

Defines the height of a projectile. Height should be a negative value. Default is <code>-23</code>.
___ 
## float HomingStrength

___ 
## integer ProjectileFlags

Uses <a class="el" href="group__enums.html#ga0302119ed82822df78af258ee457e6a6">ProjectileFlags</a> to define the projectile attributes.
___ 
## float Scale

___ 
## integer WiggleFrameOffset

___ 