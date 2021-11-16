<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

### Calculate Floor Normal

<sub>[previous](../physics-material/README.md) • [home](../README.md) • [next](../set-physics/README.md)</sub>

<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

Now we'll need to develop a way to determine the player's current slope, and apply the appropriate physical material based on the current slope.

<br>

---


##### `Step 1.`\|`ITA`|:small_blue_diamond:

Open up your <b>ThirdPersonCharacter</b> Blueprint and move to the bottom of the graph. Our first task will be to check where the player’s feet are standing.

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 2.`\|`FHIU`|:small_blue_diamond: :small_blue_diamond: 

Create an <b>Event Tick</b> node, as we’ll need to be checking every frame. From the execution pin of the Tick node, create a <b>Sequence</b> node to keep our graph clean. From the first pin of the Sequence, create a <b>TraceLineByChannel</b> node. This is what we have so far:

<img src="images/2021-11-09 10_06_39-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 3.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Drag in an instance of your Capsule Component (or create the node in the graph by right-clicking and typing “Get Capsule Component”). We will use this to get information about the player’s capsule shape, size, and location. First pull off a <b>GetWorldLocation</b> node. Create a copy of the Capsule Component and pull off a <b>Get Right Vector</b> node.

<img src="images/2021-11-09 10_10_47-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 4.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

The right vector points in the positive Y direction (0, 1, 0) relative to the player’s world position, which we will use to roughly estimate the player’s left and right feet positions. Let’s start with the right foot just to see how this will work. Pull of a <b>vector * float</b> pin from the Right Vector. I chose 17 as the value, as my capsule’s radius is 34 (34/2 = 17). Remember that the Right Vector is normalized, meaning that it’s between 0 to 1. By multiplying it, we’re scaling the value into something more usable for our needs.

Add the result of this multiplication to the WorldLocation with a <b>vector + vector</b> node. Hook this up to the <b>start</b> of the LineTrace. 

<img src="images/2021-11-09 10_22_33-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 5.`\|`ITA`| :small_orange_diamond:

Now for the ending of this line trace, we simply need to offset the start position downward. From the + node, make a <b>Break Vector</b> node. This will allow us to split the original vector and get the individual components. Go ahead and create the result vector by dragging out a <b>Make Vector</b> node from the X input. Make sure you’re just getting <b>Make Vector</b>, NOT MakeVector4 or MakeVector2D.

The X and Y will remain the same, so connect those together. As for the Z, we’ll need to add an offset. Pull off a <b>float + float</b> node from the Z value of the Break Vector. For my offset, I chose -110. My capsule height is 96, and I added a bit more since the feet actually stop right below the capsule. The negative is because the direction should point downward. Connect the return value of this node into the <b>end</b> input of the LineTrace.

<img src="images/2021-11-09 10_31_48-UE4LevelDesignExtras - Unreal Editor.png" />
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 6.`\|`ITA`| :small_orange_diamond: :small_blue_diamond:

In the LineTrace, set the Draw Debug Type to For One Frame

<img src="images/2021-11-09 10_35_55-.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 7.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond:

Compile and hit <b>Play</b>. See how the line points downward, roughly to where the foot is? It’s not super accurate, but it’s enough to get the information that we need. 


https://user-images.githubusercontent.com/20305074/142026234-c709e37c-c943-44ea-aa4f-0dab9a95c420.mp4

<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 8.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Now for the left foot. Copy every node except the Sequence and the Event Tick, and drag them below. The only change we need to make for the left foot is multiplying the Right Vector by <b>-17</b> instead (since we’re now looking in the negative Y direction i.e. left). Connect the <b>Then 1</b> from your Sequence to the execution pin of the LineTrace. Add comments accordingly. Here’s our graph so far:

<img src="images/2021-11-09 10_47_13-UE4LevelDesignExtras - Unreal Editor.png"/>
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 9.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Now that we have information from both feet, we can check for the physical material of the point of contact.From the first LineTrace, right-click the Out Hit and <b>Split Struct Pin</b>. From the execution pin, pull off an <b>IsValid</b> node. Connect the <b>Out Hit Phys Mat</b> to the input of the IsValid.

<img src="images/2021-11-09 11_01_32-UE4LevelDesignExtras - Unreal Editor.png"/>
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 10.`\|`ITA`| :large_blue_diamond:

From the IsValid node's <b>IsValid</b> pin, create a <b>SwitchOnEPhysicalSurface</b> node. Back to the Out Hit Phys Mat output, drag off a <b>Get Physical Surface</b> node. Feed this into the Selection input of the Switch you just created.

<img src="images/2021-11-09 11_08_37-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 11.`\|`ITA`| :large_blue_diamond: :small_blue_diamond: 

Copy and paste the three nodes you made for the first LineTrace, and bring them over to the second LineTrace. Hook up the necessary inputs (don’t forget you have to split the struct pin for the Out Hit to get the Hit Material).

<img src="images/2021-11-09 11_10_45-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>


##### `Step 12.`\|`ITA`| :large_blue_diamond: :small_blue_diamond: :small_blue_diamond: 

Create a Branch node, and connect both Ice execution pins into its execution pin.

<img src="images/2021-11-09 11_13_34-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 13.`\|`ITA`| :large_blue_diamond: :small_blue_diamond: :small_blue_diamond:  :small_blue_diamond: 

We can use this branch to determine whether or not the player is already standing on an Ice surface, and use this information to change materials accordingly. 

Now we’ll need two utility functions before completing the functionality of our Branch conditionals. The first function will be used to calculate the player’s angle relative to the floor. On the left side of your editor, click the `+` next to Functions to create a new Function. Name it <b>CalculateFloorNormal</b>

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 14.`\|`ITA`| :large_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:  :small_blue_diamond: 

Open up your new Function and select the entry node. Create an input of type <b>Vector</b> and name it <b>FloorNormal</b>. Create an output of type Vector and name it <b>NormalDirectionOfSlide</b>. Set the function to be Pure.

<img src="images/2021-11-09 11_24_09-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 15.`\|`ITA`| :large_blue_diamond: :small_orange_diamond: 

First let’s check if the angle is 0 (i.e. the ground is flat). Drag off a <b>Branch</b> node from the execution pin of the entry node (it’s okay if it disconnects from the return node, as we’ll be connecting it again later). From the condition, pull off a <b>vector = vector (Vector Equals)</b> node. The first input of this equals should be the Floor Normal from the entry node, and the second input should be a <b>Vector Up</b> node (NOT <b>GetVectorUp</b>). Connect the “true” pin of the Branch to the Return node.

<img src="images/2021-11-09 11_35_20-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 16.`\|`ITA`| :large_blue_diamond: :small_orange_diamond:   :small_blue_diamond: 

Now to find the direction of the slide/slope, we’ll need to make use of the cross product. A <b>cross product</b> returns the forward vector orthogonal (i.e. 90 degrees) to the two given vectors. From the “false” of the Branch, create another <b>Return Node</b>. Move to the beginning of the graph, and make a <b>Get Floor Normal</b> node. From the Floor Normal, drag off a <b>cross product</b> node.

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 17.`\|`ITA`| :large_blue_diamond: :small_orange_diamond: :small_blue_diamond: :small_blue_diamond:

Make another <b>Vector Up</b> node, and connect this into the second input of your cross product.

<img src="images/2021-11-11 09_26_03-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 18.`\|`ITA`| :large_blue_diamond: :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

This gave us the normal to the plane containing the Floor Normal and the Up Vector. Now we need the normal of the plane containing that previous vector and the Floor Normal. This will give us the direction of the slope that we’re currently on. From the Floor Normal, drag off another <b>cross product</b>. In the second input of that cross product, connect the return value of your first cross product.

<img src="images/2021-11-11 09_30_54-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 19.`\|`ITA`| :large_blue_diamond: :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Before sending this to the return value of the function, we need to normalize it. This will make it so that the vector becomes a unit vector, meaning that its component values range from 0 to 1. Simply drag off a <b>normalize</b> node from the second cross product and send that value to the return node. Comment your graph accordingly. Your Calculate Floor Normal function should look something like this:

<img src="images/2021-11-11 09_33_37-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 20.`\|`ITA`| :large_blue_diamond: :large_blue_diamond:

That's all for the Calculate Floor Normal function. Next we'll use this in our character Blueprint (along with the physical material checking) to turn the ice physics on and off. Save your work, compile, and submit to source control.

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

___


<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

<img src="https://via.placeholder.com/1000x100/45D7CA/000000/?text=Next Up - Setting Physics">

<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

| [previous](../physics-material/README.md)| [home](../README.md) | [next](../set-physics/README.md)|
|---|---|---|
