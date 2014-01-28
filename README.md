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

Move Avatar With Arrow Keys
---------------------------

Now that we have the key codes for the arrow keys, let's make the avatar move while we are pressing the keys. Instead of the code above that pops up an alert, use the below code instead. You will need to replace <arrow code> with the apprepriate number for each arrow key. If all goes well we should now be able to move the avatar left, right, forwards, and backwards.

```
document.addEventListener('keydown', function(event) {
    var code = event.keyCode;

    if (code == <left arrow code>)
        avatar.position.x = avatar.position.x - 5; // left

    if (code == <up arrow code>)
        avatar.position.z = avatar.position.z - 5; // up

    if (code == <right arrow code>)
        avatar.position.x = avatar.position.x + 5; // right

    if (code == <down arrow code>)
        avatar.position.z = avatar.position.z + 5; // down
});
```

Bonus: Make The Avatar Cartwheel and Flip
----------------------------------

The code to make the avatar cartwheel and flip is already in place. There are two variables on the page called isCartwheeling and isFlipping. Look in the animate() function. When those variables are set to true the avatar will cartwheel or flip. If the variables are set to false, then the avatar will stop doing what it was doing. What you need to do is add to the code that detects key presses. In addition to the arrow keys use the C key to start and stop the avatar doing cartwheels. Use the F key to start and stop the avatar doing flips.