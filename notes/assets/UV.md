# UV Coordinates in Three.js

## 1. What are UV Coordinates?

UV coordinates determine how a 2D texture is mapped onto the surface of a 3D object.

- `U` represents the horizontal coordinate of a texture.
- `V` represents the vertical coordinate of a texture.

UV coordinates typically range from `0` to `1`.

| UV Coordinate | Meaning                                                   |
| ------------- | --------------------------------------------------------- |
| `(0, 0)`      | One corner of the texture, conventionally the bottom-left |
| `(1, 0)`      | Bottom-right                                              |
| `(0, 1)`      | Top-left                                                  |
| `(1, 1)`      | Top-right                                                 |
| `(0.5, 0.5)`  | Center of the texture                                     |

## 2. Why Do We Need UV Coordinates?

A 3D model has a surface, while a texture is a 2D image. UV coordinates tell Three.js which part of the image should appear at each point on the model's surface.

For example, UV coordinates allow you to:

- Apply an image to a 3D model.
- Map a brick texture onto a wall.
- Apply a character's skin or clothing texture.
- Control how textures are stretched or repeated across a surface.

## 3. How UV Coordinates Work

Imagine a texture as a 2D image:

- `U` moves horizontally across the image.
- `V` moves vertically across the image.
- A surface's UV coordinates determine which texture location is sampled at each point.

The GPU uses these coordinates when rendering the textured surface.

## 4. UV Coordinates in Three.js

Three.js geometry stores UV coordinates in a geometry attribute named `uv`.

```js
console.log(geometry.attributes.uv);
```

For a standard `BoxGeometry`, Three.js generates UV coordinates automatically.

```js
const geometry = new THREE.BoxGeometry(1, 1, 1);

const material = new THREE.MeshBasicMaterial({
  map: texture,
});

const mesh = new THREE.Mesh(geometry, material);
```

The `map` property applies the texture to the material. The geometry's UV coordinates determine how that texture is mapped onto its faces.

## 5. UV Coordinates vs. XYZ Coordinates

| Coordinates   | Purpose                                |
| ------------- | -------------------------------------- |
| `X`, `Y`, `Z` | Describe positions in 3D space         |
| `U`, `V`      | Describe positions within a 2D texture |

**Key Takeaway:** XYZ coordinates describe where a point exists in 3D space, while UV coordinates describe which part of a 2D texture is applied to a surface.
