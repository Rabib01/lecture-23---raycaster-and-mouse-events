![RayCaster Image](./assets/Screenshot%20from%202026-10-12%2012-23-03.png)

# Raycaster

Cast a ray in a and test what objet intersect in the ray. Test if the laser gun hit the enemy. Test if something is below the cursor to simulate mouse events. Show an alert if the spaceship is heading is straight to the planet. There are many other uses such as -

| Use Case               | Description                                                                  |
| ---------------------- | ---------------------------------------------------------------------------- |
| Mouse interaction      | Detect which 3D object is underneath the cursor.                             |
| Laser gun / shooting   | Test whether a laser shot hits an enemy.                                     |
| Object selection       | Select a 3D object by clicking it.                                           |
| Hover effects          | Highlight an object when the cursor moves over it.                           |
| Ground detection       | Check whether an object is standing on the ground.                           |
| Obstacle detection     | Detect walls or obstacles ahead of a moving character.                       |
| Spaceship navigation   | Check whether a spaceship's forward path intersects a planet.                |
| Line of sight          | Determine whether an enemy can see the player or a wall blocks its view.     |
| Collision-like checks  | Test directional intersections with objects.                                 |
| Placement validation   | Identify the surface beneath the cursor before placing an object.            |
| Interaction prompts    | Detect whether a player is looking at an interactable door, button, or item. |
| Distance and targeting | Find objects along a direction and determine which target is closest.        |

We have three spheres and shoot through those spheres and test if the spheres intersect with it
https://threejs.org/docs/?q=raycaster#Raycaster

If we are shooting a reay from a direction then we need to tell the origin of the ray and then the direction of the ray
raycaster.set() These are vector 3 -> right -> x = 1
we do origin -3 as the furest one to the right is -2

```javascript
const raycaster = new THREE.Raycaster();
const rayOrigin = new THREE.Vector3(-3, 0, 0);
const rayDirection = new THREE.Vector3(10, 0, 0);
const rayDirection = new THREE.Vector3(3.3, 2.5, 7.5);
```

The 10 is important because this value has to be normalized - what is normalized ? It is when the lenght of the vector 3 is 1. WHen we have a complex values like the
4th line, what are we going to be putting this in the line.
Easy solution is to do rayDirection.normalize() reduce the size so that it gets to 1 but keep the direction

```javascript
const raycaster = new THREE.Raycaster();
const rayOrigin = new THREE.Vector3(-3, 0, 0);
const rayDirection = new THREE.Vector3(10, 0, 0);
console.log(rayDirection.length());
rayDirection.normalize();
console.log(rayDirection.length());
```

First cl gives us 10, second cl gives 1. ALWAYS normalize the direction the raycaster need that

```javascript
raycaster.set(rayOrigin, rayDirection);
```

We have just created the raycaster, not the ray. The one that we create that will shot the ray. Now we can cast the ray and see wqith what objects that.
It is now going through the spheres,
now we cast a ray and see what objects the ray intersect
2 options to cast a ray - intersectObbject or Objects

```javascript
const raycaster = new THREE.Raycaster();
const rayOrigin = new THREE.Vector3(-3, 10, 0);
const rayDirection = new THREE.Vector3(10, 0, 0);
rayDirection.normalize();

raycaster.set(rayOrigin, rayDirection);

const intersector = raycaster.intersectObject(object2);
console.log(intersector);

const intersectors = raycaster.intersectObjects([object1, object3, object3]);
console.log(intersectors);
```

At this example ray is going above the scene and so nothing really happens

What is wierd is that why are we getting an array for a signel object if it is just an intersector ?

```javascript

[{…}]

```

This was delebarately chosen because it can go through one object multiple times.

![notes](./assets/Screenshot%20from%202026-10-12%2013-11-52.png)

distance - 2.5
face - face of the geometry
faceIndexx - something with that
object - we can know which object is intersecting with the ray - this is useful we have a hundred objects and we want to know which bject is being hit - we can make the object explode or something
point - position of the collision in 3d space - because it is a vector 3 - This is a vector 3 - Co-ordinates where the intersection occurs - maybe make the object explode when the ry hits that point
uv - If the geometry supports UV - in the UV and UV coordinates - We can have these 2D coordinates for the UV

# Test

We are going to animate and test the ray on each sphere - THis can be quite heavy

```javascript
/**
 * Animate
 */
const clock = new THREE.Clock();

const tick = () => {
  const elapsedTime = clock.getElapsedTime();

  //   Animate objects
  object1.position.y = Math.sin(elapsedTime * 0.5) * 1.5;
  object2.position.y = Math.sin(elapsedTime * 1.5) * 1.5;
  object3.position.y = Math.sin(elapsedTime * 0.9) * 1.5;

  // Update controls
  controls.update();

  // Render
  renderer.render(scene, camera);

  // Call tick again on the next frame
  window.requestAnimationFrame(tick);
};

tick();
```

This is how we do the animations of the objects

origin,
direction,
setmethod
shoot the ray

This is how we contnuously check whether the ray is hitting the circles or not

```javascript
const tick = () => {
  const elapsedTime = clock.getElapsedTime();

  //   Animate objects
  object1.position.y = Math.sin(elapsedTime * 0.5) * 1.5;
  object2.position.y = Math.sin(elapsedTime * 1.5) * 1.5;
  object3.position.y = Math.sin(elapsedTime * 0.9) * 1.5;

  // cast the ray
  const origin = new THREE.Vector3(-3, 0, 0);
  const direction = new THREE.Vector3(10, 0, 0);
  direction.normalize();
  raycaster.set(origin, direction);
  const intersectors = raycaster.intersectObjects([object1, object2, object3]);
  console.log(intersectors);
  //   But here's the catch: the result you got earlier doesn't automatically update. That was the result of the previous check.
  // If you want to know whether the cube is intersecting the ray now, you must check again:

  // Update controls
  controls.update();

  // Render
  renderer.render(scene, camera);

  // Call tick again on the next frame
  window.requestAnimationFrame(tick);
};

tick();
```

# Now we are going to color the objects that are intersecting as blue and ones that are not intersecting as red

Because we are working with the array of objects that are intersecting
and the property we want is the object of the interse4ctios
we have to loop through the array
https://threejs.org/docs/?q=colo#Color

TO change the color we have to change the Three.color.set methid.
this below will not work because of hte reasons in the comments
however - > thisis a js code structuring problem
before setting hte color blue set all of the colors as red - you can come up with your own solutions later

```javascript
/**
 * Animate
 */
const clock = new THREE.Clock();

const tick = () => {
  const elapsedTime = clock.getElapsedTime();

  //   Animate objects
  object1.position.y = Math.sin(elapsedTime * 0.5) * 1.5;
  object2.position.y = Math.sin(elapsedTime * 1.5) * 1.5;
  object3.position.y = Math.sin(elapsedTime * 0.9) * 1.5;

  // cast the ray
  const origin = new THREE.Vector3(-3, 0, 0);
  const direction = new THREE.Vector3(10, 0, 0);
  direction.normalize();
  raycaster.set(origin, direction);
  const objectsToTest = [object1, object2, object3];
  const intersectors = raycaster.intersectObjects(objectsToTest);
  {
    //   console.log(intersectors.length);
    //   But here's the catch: the result you got earlier doesn't automatically update. That was the result of the previous check.
    // If you want to know whether the cube is intersecting the ray now, you must check again
  }
  //   Color the intersected as blue and keep the rest of them red - need to loop through the array taht are intersected

  for (const intersected of intersectors) {
    {
      // console.log(intersected.object); // this gives the meshes of the objects that are intersected // the mesh has a material property // material has a color property // can be changed using color.set("#hexCode")
    }
    intersected.object.material.color.set("#0000ff"); // This wont work as all of them start from hte same point and the ray hits that point at the start of the experience
  }

  // Update controls
  controls.update();

  // Render
  renderer.render(scene, camera);

  // Call tick again on the next frame
  window.requestAnimationFrame(tick);
};

tick();
```

t.b.c : 23:01

We can also use the raycaster to see mouse hovering effects
first we are hoing to see the hovering parts
mouse hover detection
get position of cursor -1 to 1
