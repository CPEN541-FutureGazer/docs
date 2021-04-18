# CPEN 541 Technical Report Draft

Last edited by Muchen He, 2021-04-17

- Team

# Preface

- Who this document is for
- What the purpose of this document is
- Please see paper, conference, and video for further information

- Contact information about team members
- Where we can find our code

- Disclaimers

# Introduction and Problem

Over the last year, we all observed and experienced first-hand at attempting to scavenge the productivity and work-ethics we once had while working from home while in a pandemic. And one of the most important work-related ritual is none other than meetings and hangouts. 

As limited by the restriction and the isolation to curb the infectious virus, we sacrificed the ability to meet with people face-to-face (F2F) -- whether that be getting work done, attending classes, discussing and brainstorming ideas, and even asking for help and or assistance from peers. Out of necessity, we all forced to use online web-video-conferencing (WVC) software such as Zoom, Skype, etc. as a substitute.

Despite these companies touting the technological advancements, the intuitive of its user-interfaces, the affordable cost of “free” to its users, they all had this one *glaring* (pun-intended) problem that none of these software truly provided in the absence of F2F meetings: that **eye-contact** and **gaze** are an essential part of body language and non-verbal communications in social situations. 

Conventional WVC software evolved from 1-on-1 meetings, where the only other person on the screen is the one you talk to, like in a phone call. This has the few problems of modern, large meetings involving more people, since the other person’s face is the only thing you look at and thus easier to make eye-contact and attention. 

`insert picture of older Skype/FaceTime image`

However, seen in the image below, as we add more participants to the meeting, instead of being an intimate experience, we are often presented with a gallery, somewhat like a bookshelf view of all the participants. Each participant is placed in a uniform cell as part of a grid. One could describe it as a jail-cell, lacking in connection, organicity (?), and coherency. Furthermore, because everyone is looking at their own screen, towards their camera, we get a mostly static view of all the participants looking out of the screen on the receiving end. Somewhat akin to have a large audience all staring at you -- even if you’re one among them passively listening.

`insert gallery view of zoom` in contrast with `image of people sitting around a table looking at each other`

## Research Questions and Scope

This motivated us to explore alternative ways to represent participants in WVC applications to increase engagement, interactivity, and attention. 

We are set out to build a prototype WVC application to study the perceived effects of eye-contact, gaze, and head orientation in a virtual 3D space to make up the missing aspects from in-person F2F interactions.

`insert research question`

`insert scope of this project`

# Background and Prior Works

`insert existing solution`

`insert VR 3d chat apps`

`insert other research work`

# The Prototype

This section describes the technical details regards to our implementation. We first outline the language and framework this prototype is developed on, then we describe the rendering, layout, and orientation calculation. Lastly we go over the configuration files, events, timed and randomized sequences, and integrated questionaries that aid us in user-experimentation.

> Note: Since this project is not mainly focused on the usability study of this concept, we only implemented the minimum-viable-product to carry-out the user experiments due to time-constraints. We discuss how this prototype could be improved in `future>prototype` section.

## Platform

The prototype is mostly developed in Processing `cite`, a Java-based graphic-centric language mostly used in education, prototype, and visual arts. Due to limitations (see limitations sections), it is wrapped in a Unity game engine for integrated questionnaire for a smoother user-testing experience. We chose Processing and Unity as they have strong support of 3D environment, video and audio playback, robust mouse input, ease-of-use, and quick prototype turn-around overheads.

- Cross platform
- ^ although some issues with Mac application export due to recent Mac OS gatekeeper security checks
- 

## 3D Environment

- This section talks about the 3D environment built inside this prototype
- Based the design off of Zoom (background colors, buttons, UI layout) such that it will present the participants with a familiar UX
- We call the visual representation of other participants in the meeting *Views*, we have two types of views: head and eyes (HeadView and EyeView class in the code respectively). The View class is solely responsible for managing the state and rendering themselves onto the screen.

### Heads

- Processing natively supports 3D .obj files
- We used a free-to-use creative-commons head object
- Low polygon count
- No textures (see limitations/future work)
- These are needed since large amount of heads would slow down the prototype and we don’t have time to perform product-level optimizations
- No animations (because otherwise they cost money)
- No physics based animations -- again because they take up precious compute resources
- We need to maintain stable 60fps for an immersive experience
- The heads need to transform in real time as if it’s a video (can’t be pre-rendered)

### Eyes

- The 2D eyes are inspired by Fels and his mention of a novel desktop widget called x-eyes where the pupil follows the mouse cursor
- We implement a similar as an alternative to the 3D head
- Go over rendering techniques involving drawing to a texture and masking it with some transform

### UI Elements

- UI inspired by Zoom, again to give off the familiar feel
- Nameplates
- Active/isFocused circle
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

# Experiment Setup

- Recall research questions
- In pursuit of these answering these questions, we proposed these experiments that study how users feel about interacting using our prototype
- Describe all the different experiment configurations

# Experiment Results and Analysis

- TODO: need to show results
- TODO: what can we learn from these results?

# Limitations

- Covid-19 working remotely, so it’s hard to sync up between team members
- Technical limitations (technical)
	- 2D and 3D avatars/views are primitive (barely colours and textures)
	- Not customizable, could affect how people feel about usability because it doesn’t convey as much expression as originally intended
	- 3D head lack eyes with animation and texture (unfinished due to technical and time constraints) -- also affects how people perceive eye-contact in WVC because it looks creepy (and literally have no eyes).
	- Primitive lighting in 3D environment (simple directional light creates uncanny environment)
- Limitations in running the experiments:
	- User testing extremely difficult, despite the prototype framework being cross-platform, exporting/deploying apps required notarization (a security feature that only allows officially signed app to run), so we need last-minute hacks to have the user experiments run
	- Small sample size
- Limitations in collection of data
	- Ideally, we want to use some gaze-tracking hardware/software to actually track and log where the users are looking at (as seen in the omitted experiment setup using Tobii), but because none of the user experiments are performed in person, we physically cannot collect those data.
	- A workaround is to have the user click/move the mouse to where they’re looking at, but because of aforementioned platform limitations, we can’t do that either


# Future Work

This section talks about the future work

## Prototype Development

- Unified 3D framework/engine
- Apple Gatekeeper notarization 
- More sophisticated 3D models and animations
- Add real-time gaze tracking support
- More dynamic and fluid 3D environment (since right now all the heads and eyes are locked onto a grid)

# Conclusion

# References

