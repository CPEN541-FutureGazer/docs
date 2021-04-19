# The Prototype

This section describes the technical details regards to our implementation. We first outline the language and framework this prototype is developed on, then we describe the rendering, layout, and orientation calculation. Lastly we go over the configuration files, events, timed and randomized sequences, and integrated questionaries that aid us in user-experimentation.

> Note: Since this project is not mainly focused on the usability study of this concept, we only implemented the minimum-viable-product to carry-out the user experiments due to time-constraints. We discuss how this prototype could be improved in `future>prototype` section.

## Platform

The prototype is mostly developed in Processing `cite` (a Java-based graphic-centric language mostly used in education, prototype, and visual arts) and Unity Engine `cite` (a popular free-to-use game engine). The Unity engine wraps around the Processing prototype to provide user-testing interface such as integrated questionaries and videos.

We chose Processing and Unity as they have strong support of 3D environment, video and audio playback, robust mouse input, ease-of-use, and quick prototype turn-around overheads. Both frameworks are also cross-platform and can execute on Windows, macOS, and Linux. However, as we also discuss in Section `limitations section`, recent macOS `cite macOS update` added a *Gatekeeper* security feature that prevents un-sighed software from running -- thus complicating the user-testing process, as distributed prototype executables to the participants cannot be opened.

## 3D Environment


`insert figure of a typical view of the prototype in Unity app`

Figure `insert figure` shows a typical 3D environment rendered in the prototype app. We base the user interface (UI) design -- including colours, buttons, and layout -- from existing WVC apps such as Zoom to present the participants with a familiar user experience. By doing so, we limit other factors that would interfere with our user experiments. However unlike traditional WVC apps, we replace the centre (where typically there would be a gallery or grid view of camera video feeds) with our prototype avatar representation.

From hereafter, we will refer to these visual representation of participants in the meeting as *Avatar Views* (*View* class). As outlined earlier in [Section on research questions](#research-questions-and-scope), we propose two types of avatars to explore: 3D head avatars (*HeadView* class) and 2D eye avatars (*EyeView* class). 

`insert figure of head view class and the eye view class`


### Heads

`show image of 3d model of the head`

`insert head avatar grid view`

In the 3D environment, the head avatars are loaded from a free-to-use .OBJ 3D model downloaded from *TurboSquid* with standard usage license `cite` [^:https://blog.turbosquid.com/turbosquid-3d-model-license/#Educational-Use]. 
We load the object into the 3D scene once, and use the same model instance for all other head avatar *HeadView* objects to save computation -- since they’re essentially the same 3D model, just redrawn with different transformations.

Due to time and budget constraints, we are limited to a low-polygon-count model of the head, with no texture, no rigging, no soft-body animation, and no physics simulation. The 3D model of the head, as shown in Figure `TODO`, has minimum features of a face. Despite the low-quality 3D models, the primitiveness of the 3D model helps maintaining a high rendering frame-rate while the prototype is running, even without writing custom shaders and performing fine-tuned optimizations -- especially when there could potentially be up to 9 to 25 avatars being drawn concurrently on to the screen. This is important as we need the head avatars to transform and render in real-time as if they’re real inputs in a WVC application.
As we will discuss in Section `TODO: future work`, if given more budget and 3D talent, more work can be allocated to polishing the visual appeal of the avatars to be more inviting and friendly, and ultimately improve user-experience.

### Eyes

The 2D eyes avatar was inspired from goggly eyes and novel desktop widgets (e.g. XEyes `TODO: cite`) created as general amusement. However, we decided to explore this as an alternative to the head avatars to see whether if 3D and head orientation is required to deliver eye-contact and gaze hints to the meeting participants. 

`insert breakdown diagram of the eye rendering`

`insert eye avatar view`

`insert eye avatar grid view`

With eyes avatars, instead of a 3D object transformed to orient a direction, we simply render a pair of pupils and irises on a white background as a sub-image (Figure `TODO`). Then depending on where the eye should be looking at, we rendering this sub-image with a transform that translates it in $x$ or $y$ direction (Figure `TODO`). Finally, we create an eye mask that only only renders the eye itself (Figure `TODO`).

The result is somewhat similar to a gallery view of meeting participants in a traditional WVC application, except with only the eyes and their gaze visually represented.

### UI Elements

- UI inspired by Zoom, again to give off the familiar feel
- Nameplates
- Active/isFocused circle to help users identify where the sound is coming from
- (Optional) green point light to high light where the mouse is

## View Calculation

### Target Coordinates

- Both eye and head views have an attribute targetX, targetY which is the screen coordinates of where this view should be looking at. This could be mouse position, or some other programmed position (another view, some arbitrary coordinates)
- For heads, this target coordinates need to be mapped to some 3D space rotation (rotX, rotY, rotZ) -- see next subsection for this mapping
- Eyes are bit more straightforward since targetX,Y is a direct mapping because eyes are 2D renderings

### Rotation Calculation
- Rotation can be computed by drawing a vector from x,y of head’s origin to the mouse origin in the 3D space -- doesn’t work well when the heads are in 3D space but users are interacting on a 2D plane (so we used a linear mapping as an approximation and it looks pretty well)
- Origin offset: needed because if the 3D head look straight on the z-axis they actually doesn’t look like they look at the screen because of perspective
	- “calibrate” by wrapping their view by mapping their x-y position from the screen centre to some rotation offset

## View Modes

- Normal, random, stare modes
- How to switch between modes

## Configuration Files

Describe how these infrastructure is needed to run the experiments

### Initialization

### Events

- Change mode, target id etc
- Change active
- Play sound

### Event loop to orchestrate and replay event during demo
- Processing code needed to generate 

### Randomization
- To randomize certain events, python script used to generate the configuration json file
