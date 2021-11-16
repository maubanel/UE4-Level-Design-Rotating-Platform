<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

### Setting Physics

<sub>[previous](../calculate-floor-normal/README.md) • [home](../README.md) </sub>

<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

We'll need some more control over the physics applied to the surface. Let's put our new physical material to use.

<br>

---


##### `Step 1.`\|`ITA`|:small_blue_diamond:

Let’s return to our BP_ThirdPersonCharacter and add in our new function. Drag in a copy of the Calculate Floor Normal function, and place it near your first LineTrace. Connect the LineTrace’s <b>Out Hit Normal</b> into the input of your function. Repeat this for the second LineTrace. 

From each of the Calculate Floor Normals' outputs, drag off a <b>Not Equal</b> node (make sure you’re just using the regular Not Equal and NOT Not Equal (Exactly)). Your graph should look as follows:

<img src="images/2021-11-11 10_06_10-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 2.`\|`FHIU`|:small_blue_diamond: :small_blue_diamond: 

We will use each of these Not Equals to check whether or not the current slope is at zero, and (if not zero) to apply the ice physics that we created earlier to force to player to slide off. Now create an <b>OR Boolean</b> node, and pipe both Not Equals’ return values into the OR’s input. An OR basically means that if both input are either true or false, then the entire statement must be false. In our case, if one “foot position” is on ice, and the other is not, then start the slide. 

<img src="images/2021-11-11 10_25_11-UE4LevelDesignExtras - Unreal Editor.png"/>
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 3.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

From the True pin of the Branch node, pull off another Branch node. Use the IcePhysics boolean we made earlier as the condition. From this second Branch node, drag off an <b>Add Force (Character Movement)</b>. For the Force value, we need to <b>multiply</b> the <b>Calculate Floor Normal</b>’s return value by some large number (I chose 200000).

<img src="images/2021-11-15 22_59_28-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 4.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Before we move on to the False statements of our Branch nodes, we’ll need another function to switch the physics on the object. 

Make a new Function called <b>SetPhysics</b> and open it up. Click on the function’s entry node and create a new Input variable called <b>On Ice</b> of type boolean. To the right of the Branch, drag in a copy of <b>Character Movement</b>. From the Character Movement node, drag off a <b>Set Ground Friction</b> node and set the value to <b>4.0</b>. From the Set node’s execution, drag off a <b>Set BrakingDecelerationWalking</b> node. Connect the Character Movement as the target. Connect the True pin of the Branch into the Set node’s execution pin. Add a comment around this section.

<img src="images/2021-11-11 10_37_39-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 5.`\|`ITA`| :small_orange_diamond:

Copy and paste this section directly below. Change the ground friction value to <b>8.0</b> and the braking deceleration to <b>2048</b>. Comment this section accordingly.

<img src="images/2021-11-11 10_39_48-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 6.`\|`ITA`| :small_orange_diamond: :small_blue_diamond:

That’s all for the utility functions. Let’s go back to the Event Graph and finish off our Blueprint. From the first Branch, drag off another Branch. Bring in a <b>Get Ice Physics</b> as the condition. From the True pin of this node, drag off a <b>Set Ice Physics</b>. Leave this value as false. From this Set node, drag off a <b>Set Physics</b> (the function you just made). Connect the output of the Set node as the condition.

<img src="images/2021-11-11 10_45_52-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 7.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond:

Now go back up to the second Branch you made (the one right before the Add Force). From its False, drag off another <b>Set Ice Physics</b> and set it to <b>True</b>. From its execution pin, make another <b>Set Physics</b>. Just as we did before, connect the output of the Set node into its condition. This section should look something like this:

<img src="images/2021-11-11 10_55_52-UE4LevelDesignExtras - Unreal Editor.png">
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 8.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

Compile everything and return to the game scene. Press Play and check out the new behavior.

## 2021-11-11 11-03-37.mp4
<br>
See how much easier it is to slip off? That’s because we’re checking for each foot’s normal relative to the ground, and forcing the player to slip if either foot is at an angle. 
<br>
<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 9.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

That’s it for this walkthrough! Save and submit this project to source control.

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

___


<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

<img src="https://via.placeholder.com/1000x100/45D7CA/000000/?text=Finished!">

<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

| [previous](../)| [home](../README.md)|
|---|---|