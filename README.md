Chrome Security Issue
---------------------

Google Chrome will prevent our 3D games from running when the files are stored on our computer instead of on a web server. To get around this issue make sure Chrome is completely closed. Open a command prompt and enter the command:

```
open -a Google\ Chrome --args --disable-web-security
```

Use A Function To Create Trees
------------------------------

Let's add some trees to our scene. We will build our trees using a cylinder for the base and a sphere for the top. Since we want more than one tree we will end up writing the same code over and over again for how ever many trees we want to put into our scene. Instead, we can write a function that contains the lines of code to create a tree. Our function will allow us to specify two numbers (known as parameters) that it will use to position the tree in our scene. This way we can call the function multiple times and pass different parameters to make a bunch of different trees in different spots.

```
function makeTreeAt(x, z) {
    var base = new THREE.Mesh(
        new THREE.CylinderGeometry(50, 50, 200),
        new THREE.MeshBasicMaterial({color: 0xA0522D})
    );

    var leaves = new THREE.Mesh(
        new THREE.SphereGeometry(150),
        new THREE.MeshBasicMaterial({color: 0x228B22})
    );
    leaves.position.y = 175;

    base.add(leaves);

    base.position.set(x, 60, z);

    scene.add(base);
}
``` 

Now call the function how ever many times you want trees in the scene. For two trees do something like:

```
makeTreeAt(200, 0);
makeTreeAt(600, -500);
```

Try moving around the scene. Note that at the moment you can walk right inside a tree!

Detect A Collision
------------------

The way to detect a collision is to see if our avatar is occupying the same space as a tree. To simplify this we will create a circular boundary around the base of the tree, then whenever our avatar moves inside the circle we will know a collision has occured. Add the following code to your makeTreeAt function which will create the circular boundary and position it at the base of each tree.

```
var boundary = new THREE.Mesh(
    new THREE.CircleGeometry(100),
    new THREE.MeshNormalMaterial()
);
boundary.position.y = -100;
boundary.rotation.x = -Math.PI/2;
base.add(boundary);
not_allowed.push(boundary);
```

Then we need to detect if a collision happens between our avatar and the boundary. Add this function that will detect collisions. Also, call the function at the end of your keydown event listener.

```
function detectCollision() {
    var vector = new THREE.Vector3(0, -1, 0);
    var ray = new THREE.Raycaster(marker.position, vector);
    var intersects = ray.intersectObjects(not_allowed);

    if (intersects.length > 0) {
        alert('Collision detected');
        return true;
    }

    return false;
}
```

Now try moving your avatar into one of the circles and you should get a message that a collision has occured. 

We now want to quickly undo the last avatar movement if when we detect a collection. Instead of just checking for a collision in your keydown event listener, add this code which will reverse the avatars last movement.

```
if (detectCollision()) {
    if (isMovingLeft)
        marker.position.x = marker.position.x + 5;

    if (isMovingUp)
        marker.position.z = marker.position.z + 5;

    if (isMovingRight)
        marker.position.x = marker.position.x - 5;

    if (isMovingDown)
        marker.position.z = marker.position.z - 5;
}
```