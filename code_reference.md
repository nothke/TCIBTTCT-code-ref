### Intro

This is a reference page for tower spawning code for [Throw Cubes into Brick Towers To Collapse Them](https://nothke.itch.io/tower)

The code is written in Lua programming language, check out [Lua reference pages](https://www.lua.org/docs.html) for syntax information. Only a "hard sandbox" subset of the Lua language is used that includes global consts, table iterators, string, table, basic, math and bit32 modules.

There are 3 important types for the game related code, which are implemented through Lua tables. At the end of the code, the game collects collections of these 3 types to spawn them in the game.

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

The simplest way to start is to make a single brick:
```
brk()
```
This creates a brick and adds it to the list that is then, at the end, returned to the engine to spawn all the cubes. Changing it's position is done by setting it's "pos" value which is just a x, y, z keyed table
```
brk().pos.y = 10
```
Or, of course, you can put it in a variable to modify multiple values
```
b = brk()
b.pos.y = 10
b.pos.x = 10
```
Alternatively you can create the whole vector by using vec()
```
b = brk()
b.pos = vec(10,10,0)
```
Changing the bricks size must be done by providing a collider to the brick
```
c = col() -- creates the collider table
c.dim.z = 5
b = brk(c)
b.pos.y = 10
```
To color the cube, you can provide a renderer and set its color property
```
r = rend() -- creates the renderer table
r.color = "#FF0000"
c = col()
c.dim.z = 5
b = brk(c, r) -- provide the renderer as a second parameter
b.pos.y = 10
```
As you have noticed, brk() function accepts 2 optional arguments, of which the first is collider, and the second is renderer. If one or both arguments are not provided, the last used collider/renderer will be used. If there are no previous ones, a new one is automatically created.

Renderers and colliders should be shared between multiple bricks for best performance. Since they act as shared components, that also means that editing one of them after the fact affects all the bricks that have that collider/renderer assigned.

Although you can edit, for example, the color of the renderer by directly referencing it on the brick, note that that will change color of all bricks assigned to it. In the same way that editing the renderer itself does.
```
r = rend()
r.color = "#FF0000" -- red
b1 = brk(nil, r) -- nil for the collider means the last (or new) collider will be used
b2 = brk(nil, r)
b1.rend.color = "#00FF00" -- changes the color of the renderer to green, which also changes the color of the first brick
```
To assign different color to 2 bricks, you need to create 2 renderers
```
r1 = rend()
r1.color = "#FF0000"
b1 = brk(nil, r1) -- the red brick
b1.pos.y = 0.5

r2 = rend()
r2.color = "#00FF00"
b2 = brk(nil, r2) -- the green brick
b2.pos.y = 1.5
```
Here's a simplest actual use case: create a stack of 100 bricks. Use collider y dimension for calculating offset, so that we can assign any brick height.
```
c = col()
c.dim.y = 1 -- assign any value and it'll work!

for i=1,100 do
 brk(c).pos.y = c.dim.y * (0.5 + i - 1)
end
```

### Notes

* Renderers and colliders are "assets" and bricks just store a reference to those assets. Therefore changing a .dim of a collider for example will change the dimension of all bricks that use that collider. So, if you want to have different dimensions you need to make new colliders for each brick.
* For best efficiency, use the least possible amount of colliders and renderers
* Do not use `return` in the main body as the code automatically returns data at the end. If you really really need to use return use `return {all_types, all_rends, all_blocks}`
* Do not use variables names that are the same as standard functions, because it will hide them. For example `brk = brk()`, the function brk() is now gone. Classic lua problem.
* To set a different renderer or colliders to a brick, do not set them directly with brick.col = collider for example (as 
that will not change the internal id). Use set_rend() and set_col(), respectively, instead.
* TODO: Add member adding to a table warning

