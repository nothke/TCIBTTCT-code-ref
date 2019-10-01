### Intro

This is a reference for tower spawning code

The code is written in Lua programming language, check out [Lua reference pages](https://www.lua.org/docs.html).

There are 3 important types in the code, which are implemented through Lua tables. At the end of the code, the game collects collections of these 3 types to spawn them in the game.

* "brick" represents an instance and transform of a single brick. It contains position and rotation vectors, and references to the collider and a renderer.
* "collider" represents the box collider of a brick. It is an "asset" and therefore should be shared between multiple bricks. It contains the dimension of the brick.
* "renderer" represents the material of a brick. Just like the collider, it is an asset and should be shared between multiple bricks. It contains the color information. 

To simplify the code, built-in functions (in the form of a header) provides constructors for creating these tables which automatically add objects to their output tables.

### Ref

* `brk([_col], [_rend])` - creates a new brick, calling brk() with no arguments will automatically create a new collider and a render, or fetch the last existing ones.
	* `.pos` - get/set position vector, in meters, default is vec(0,0,0)
	* `.rot` - get/set rotation vector, in degrees, default is vec(0,0,0)

* `col()` - creates a new collider
	* `.dim` - get/set dimension vector, in meters, default is vec(2,1,1)

* `rend()` - creates a new renderer
	* `.color` - get/set color hex value, default is `"#FFFFFF"`

* `set_col(brick, collider)` - set collider to a brick after construction
* `set_rend(brick, renderer)` - set renderer to a brick after construction

#### Vector functions

* `vec(x,y,z)` - creates a vector table
* `vlen(a,b)` - returns length of a vector
* `vnorm(v)` - returns normalized vector
* `vcross(a,b)` - returns cross product of 2 vectors
* `vadd(a,b)` - returns a vector that's a sum of 2 vectors
* `vmul(a,b)` - returns a vector with memberwise multiplication of 2 vectors
* `vneg(v)` - returns negative vector
* `vscl(v, num)` - scale vector by a number

* `dir2angle(dir)` - converts direction to y angle from x and z (automatically normalizes the input vector)

### Examples:

Create a brick and move it to x = 10, rotate by 90 degrees around vertical axis
```
b = brk()
b.pos.x = 10
b.rot.y = 90
```
Create a brick with size 10
```
c = col()
c.dim = vec(10,10,10)
brk(c)
```
Create a red brick
```
r = rend()
r.color = "#FF0000"
brk(col(), r) -- note that we need to pass a collider first
```

### Notes

* Renderers and colliders are "assets" and bricks just store a reference to those assets. Therefore changing a .dim of a collider for example will change the dimension of all bricks that use that collider. So, if you want to have different dimensions you need to make new colliders for each brick.
* For best efficiency, use the least possible amount of colliders and renderers
* Do not use `return` in the main body as the code automatically returns data at the end. If you really really need to use return use `return {all_types, all_rends, all_blocks}`
* Do not use variables names that are the same as standard functions, because it will hide them. For example `brk = brk()`, the function brk() is now gone. Classic lua problem.
* To set a different renderer or colliders to a brick, do not set them directly with brick.col = collider for example (as 
that will not change the internal id). Use set_rend() and set_col(), respectively, instead.
* TODO: Add member adding to a table warning

