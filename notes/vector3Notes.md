# Vector3 in Three.js

## 1. What is a Vector3?

A `Vector3` is a mathematical object that stores three components: `x`, `y`, and `z`. It is used to represent positions, directions, movement, and other quantities in 3D space.

```js
const position = new THREE.Vector3(2, 5, 3);
```

- `x = 2`: horizontal coordinate
- `y = 5`: vertical coordinate
- `z = 3`: depth coordinate

## 2. Why do we need Vector3?

In 2D space, we use two coordinates (`x`, `y`). In 3D space, we need three coordinates (`x`, `y`, `z`) to represent a point.

| Vector    | Dimensions | Common Use                                  |
| --------- | ---------- | ------------------------------------------- |
| `Vector2` | x, y       | 2D coordinates                              |
| `Vector3` | x, y, z    | 3D positions and directions                 |
| `Vector4` | x, y, z, w | Homogeneous coordinates and transformations |

## 3. Use Cases of Vector3

### A. Position

Represents the location of an object in 3D space.

```js
mesh.position.set(2, 5, 3);
```

### B. Direction

Represents the direction in which something points or moves.

```js
const direction = new THREE.Vector3(1, 0, 0);
```

This vector points along the positive X-axis.

### C. Displacement

Represents a change in position.

```js
const movement = new THREE.Vector3(0, 1, 0);

mesh.position.add(movement);
```

This moves the object one unit upward.

## 4. Vector3 and Raycaster

A ray needs an origin (where it starts) and a direction (where it points). Both can be represented using `Vector3`.

```js
const origin = new THREE.Vector3(0, 0, 0);
const direction = new THREE.Vector3(1, 0, 0);
```

Here, the ray starts at the origin and points along the positive X-axis.

A Raycaster uses this information to test whether the ray intersects objects in the scene.

## 5. Position vs. Direction

The same vector values can have different meanings depending on how they are used.

```js
new THREE.Vector3(5, 0, 0);
```

- **As a position:** A point five units along the positive X-axis.
- **As a direction:** A vector pointing along the positive X-axis with a magnitude of five.

A direction vector is often normalized to have a length of `1`. A position vector generally does not need to be normalized.

## Key Takeaway

`THREE.Vector3` stores three numerical components (`x`, `y`, `z`). Three.js uses it to represent positions, directions, displacement, and other quantities needed to work in 3D space.
