<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

### Initial Blueprint

<sub>[previous](../) • [home](../README.md) • [next](../physics-material/README.md)</sub>

<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

To start off, we'll be laying the groundwork of our Blueprint by adding a mesh and some simple functionality for rotation.

<br>

---


##### `Step 1.`\|`ITA`|:small_blue_diamond:

Let’s start by making a Blueprint for our rotating cube. In your Blueprints folder, create a new Blueprint Class of type Actor. Name this Class BP_RotatingCube.


<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 2.`\|`FHIU`|:small_blue_diamond: :small_blue_diamond: 

Open up the Blueprint. Hit the Add Component button and find “Static Mesh.” Once you’ve added this component, name it “Cube.”

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 3.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Drag this Cube over the DefaultSceneRoot to override the default root. In the Details panel, go to the Static Mesh drop-down and search for “cube.” You can use any of the default engine cubes.

<b>Note:</b> In order to see these meshes, make sure Show Engine Content is enabled in the View Options!

<img src="images/2021-11-02%2011_09_06-.png" />

<br>

You may also want to set a material to make it easy to see. I went with the “BasicShapeMaterial.”

<br>

<img src="images/2021-11-02 11_33_39-BP_RotatingCube.png" />

<br>

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 4.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Compile your Blueprint, and let’s go back to the level. Drag this into your scene, and scale the cube to whatever size you want. Just make sure that is has enough room to rotate along its X axis without hitting the floor.

I also added a set of Linear Stairs on each side (which can be found under Geometry). This gives me some kind of goal to run towards when testing, and they’re also good for comparing scale. My final setup looks like so:

<img src="images/2021-11-02 11_41_53-UE4LevelDesignExtras - Unreal Editor.png" />
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 5.`\|`ITA`| :small_orange_diamond:

Now let’s return to Blueprinting. The Event Graph for this object is very simple, as all we need to do each tick is rotate it by a certain amount.

Start by creating a new variable of type Float called <b>RotationSpeed</b>. Set this to Private. Hit <b>Compile</b> up top and set the default value of this variable to 0.3 for now.

<img src="images/2021-11-02 11_20_45-BP_RotatingCube.png"/>
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 6.`\|`ITA`| :small_orange_diamond: :small_blue_diamond:

From the Event Tick execution pin, drag and create an <b>AddWorldRotation(Cube)</b> node. Right-click on this node’s DeltaRotation input and select <b>Split Struct Pin</b>.

<img src="images/2021-11-02 11_24_04-.png"/>
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 7.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond:

Drag in your RotationSpeed variable and connect it to the Delta X Rotation of your AddWorldRotation node. Your full graph should look like this:

<img src="images/2021-11-02 11_26_02-BP_RotatingCube.png"/>
<br>

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 8.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

And that’s it for the rotation! Simple right? Compile this Blueprint and let’s return to our scene. Hit Play!

## 2021-11-02 11-47-31.mp4
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 9.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

See how the player just falls off when hitting a certain angle? That’s no good! We want the player to slip off of the surface instead of abruptly dropping off. We can make a slippery material to simulate the lack of friction when falling off at a certain angle. For that we’ll make a <b>Physical Material</b> with some slippery properties. Commit all of your progress to source control, and proceed to the next phase of this project.

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

___


<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

<img src="https://via.placeholder.com/1000x100/45D7CA/000000/?text=Next Up - Physics Material">

<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

| [previous](../)| [home](../README.md) | [next](../physics-material/README.md)|
|---|---|---|
