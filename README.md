# Unity Scripting snippets



### Basic unity script

You can create a new script by selecting Assets > Create > C# script

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BasicScript : MonoBehaviour
{
    // Start is called before the first frame update, just like setup in p5.js
    void Start()
    {   
    }

    // Update is called once per frame, just like the draw function in p5.js
    void Update()
    {  
    }
}
```

You can add a script to an object in your scene by simply dragging it on that object.

you can open the script by double clicking on it.

### Triggers

First give the trigger object a tag so you know what you are triggering

[unity triggers](https://learn.unity.com/tutorial/physics-interactions-colliders-and-triggers-2019-3#5fed846eedbc2a00283f7acf)

When the first person controller collides with the trigger object, OnTriggerEnter is fired, you may find onTriggerExit is also useful.

```c#
public class BasicScript : MonoBehaviour
{
  	void Start(){}

    void Update(){}
    
  	private void OnTriggerEnter(Collider other)
    {
        if(other.tag == "myTagName"){
            Debug.Log("Trigger activated!");
            //Add any other code here
        }
    }
    
    void OnTriggerExit(Collider other)
    {	
    }
}
```

You can then check for the tag name run any other code that you want e.g. play a sound, or destroy the object

### Destroying objects

You can remove objects from your scene

```c#
Destroy(gameObject);
```

To destroy a triggered object you would do this

```c#
private void OnTriggerEnter(Collider other)
{
    if(other.tag == "myTagName"){
        Destroy(other.gameObject);
    }
}
```


### Create an object

You can use a script to create an object, but first that object needs to be a prefab

[Creating a prefab](https://docs.unity3d.com/Manual/CreatingPrefabs.html)

#### Public Variables

Adding public variables to your scripts allows you to assign them in the inspector

``` c#
public GameObject myPrefab;
```

you can then drag the prefab you want to reference from your assets folder, onto the script.

#### Instantiate the prefab

You can create a new instance of the prefab in your scene using Instantiate.

```c#
 Instantiate(myPrefab, new Vector3(0,0,0), Quaternion.identity);
```

The second argument is the position (x,y and z) or the new instance. The third argument is the rotation


### Change scenes

First make sure all you scenes are added to your build settings in File > build settings

you can then change to that scene

```c#
SceneManager.LoadScene("OtherSceneName", LoadSceneMode.Single);
```

### Play Sound

First add a sound file to the Assets folder

Then drag the audio clip onto the script in the inspector

```c#
public class BasicScript : MonoBehaviour
{
    public AudioClip myAudioClip;
    AudioSource audio;
    
    void Start()
    {
        // Your first person controller already contains an AudioSource component, you can find it in the instpector.
        audio = GetComponent<AudioSource>();
    }

    void Update() {}
    
    private void OnTriggerEnter(Collider other)
    {
        if(other.tag == "myTagName"){
            //assign sound clip to audioSouce
            audio.clip = myAudioClip;
            //play the clip
            audio.Play();
        }
    }
}
```

### Quit

When the project is build to an executable file, you will need a way to close it.

``` c#
void Update() {
	if(Input.GetKeyDown(KeyCode.Escape))
		Application.Quit();
}
```

### Move

To Move an object use transform.Translate.

``` c#
// Move the object forward along its z axis 1 unit/second.
transform.Translate(Vector3.forward * Time.deltaTime);

// Move the object upward in world space 1 unit/second.
transform.Translate(Vector3.up * Time.deltaTime, Space.World);
```

Time.deltaTime is the time between updates (it is not necessarily the same each frame) 

### Rays

Find out what the mouse is hovering over

``` c#
// send out a ray from the camera to where the mouse pointer is
Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);

RaycastHit hit;
float distance = 5.0f;

//check if it hits anything
if (Physics.Raycast(ray, out hit, distance)) //
{
    //check tag of collided object
    if (hit.collider.gameObject.tag == "interactable")
    {

    }

}
```

### New Input system

Create a new gameObject and attache a rigidbody and a script to it

``` c#
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour
{
    public InputAction move;
    public InputAction Activate;

    private void OnEnable()
    {
        move.Enable();
        Activate.Enable();
    }

    private void OnDisable()
    {
        move.Disable();
        Activate.Disable();
    }

    private void Update()
    {
        Debug.Log(move.ReadValue < Vector2 >());
        Debug.Log(Activate.ReadValue<float>());
    }
}
```

In the inspector add inputs to the move and activate variables.

![./Capture.PNG](./Capture.PNG?raw=true)





