# VRBA Avatar Spec
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

## Overview
A common avatar spec is a great starting point for interoperability between virtual worlds. There are two main reasons why this is important. First, when people interact with digital content via an avatar they are adding their body to their identity, which becomes event more intimate when its an embodied avatar using XR devices, so the avatar itself becomes a crucial part of the interaction and the person feeling comfortable making that interaction. 

Second, content creators offer a much better user experience if people don't have to carefully create an avatar each time they bump into a piece of content. A standardized spec for avatars allows for a multitude of interpreters made for different platforms (JavaScript, Unity, Unreal Engine etc.), so that avatars can be created once and work everywhere.

## File Format
Avatars shall be encapsulated in a `.gltf` or `.glb` container as per the [GLTF 2.0 Spec](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0).

Avatar creation tools MAY support exporting to these formats. For example, if you're working with JavaScript and [Three.JS](https://github.com/mrdoob/three.js), you can use [this exporter](https://github.com/mrdoob/three.js/blob/dev/examples/js/exporters/GLTFExporter.js). These creations tools MUST offer a way to get the Avatar file via a downloadable file or via a URL.

## DRM
An open format is incompatible with DRM schemes. 

For this reason, if an avatar is a composite of other items (such as a base avatar, and clothing composited together) any avatar creation tools shall offer to content developers, the option of excluding this content from export. It is left to the application software to decide how to replace, or inhibit the user from exporting the avatar if it contains protected content.

An extension block for indicating copyright of elements should be developed. If so, all compatible software must be capable of displaying this block to all end-users capable of viewing the avatar.

## Extension Block
TODO: Apply for a gltf extension prefix. Propose VRBA_ -  [howto here] (https://github.com/KhronosGroup/glTF/blob/master/extensions/Prefixes.md)

### vrba_avatar (Master Block)
This contains broad information about the avatar

* Indicates whether the avatar is humanoid or not (if humnanoid, the vrba_avatar_bonemap block must be present, if not, then an animation block must be present). A non-humanoid may still include a bone map block to mark important bones.

### vrba_avatar_bonemap (Bone Mapping)
The extension block indicates which bones map where, along with other elements for display.

The bonemap shall indicate which bones are present on the avatar. It shall be stored as a simple key/value map.

The following bones are supported, in humanoids, optional bones are marked in *italics* - non-humanoids all bones are optional:

(Markdown is butchering this - TODO: fix)

* *Root*
 * Hips 
  * Spine
   * *Spine2*
    * *Spine3*
     * Chest
      * *Left Shoulder*
       * Left Upper Arm
        * Left Forearm
         * Left Hand
          * Fingers (**see below**)
      * *Right Shoulder*
       * Right Upper Arm
        * Right Forearm
         * Right Hand
          * Fingers (**see below**)
     * *Neck*
      * Head
       * *Jaw*
       * *Eyes*
        * *Left Eye*
        * *Right Eye*
  * Left Upper Leg
   * Left Lower Leg
    * Left Foot
     * *Toes*
  * Right Upper Leg
   * Right Lower Leg
    * Right Foot
     * *Toes*

Implementing software may implement IK or bone positioning based on tracked sensor data at its discretion utilising these maps. Bones should be positioned at the start of the joint, on the hinge without an offset. (So the bone position of the hand is infact the wrist)

#### Fingers
Fingers shall be represented as five finger bones parented to the hand, each with zero to two child bones.

Each of the finger bones should be labelled:
* Thumb
* Ring
* Middle
* Index
* Little
     
The first bone should be labelled Proximal (e.g. ThumbProximal), the second Intermediate, and the final Distal. So the thumb is represented as:

* ThumbProximal
   * *ThumbIntermediate*
      * *ThumbDistal*
     
If a hand is specified, only the Proximal bone for each finger must be present. Other bones shall be optional.

#### Face
TODO: Need to decide this, but should support a range of FACS-compatible rigs, of both bone and morph target/shape key formats.

### vrba_avatar_animation (Animation Block)
TODO: Need to define this, keep in mind, many systems use blend trees and state machines instead of just pure lists of pre-recorded animations for each event/action. Codifying a state machine is likely the best approach here.

Existing controllers, like Oculus Touch, support some form of _hand presence_ which noticeably enhances avatar embodiment. To support them, each hand SHOULD have an animation corresponding to each possible pose. Avatar interpreters MUST not error if a specific avatar does not support one or more animations.

**Please note.** Improved _hand presence_ may become more popular in the future with solutions like gloves, camera based sensors (e.g. Leap Motion or Kinect) or Valve Knuckles, in which case we will revisit this section.

## Handedness, Orientation & Scale
### Scale
Objects are treated as using 'meters' for scale. (1.0 = 1 meter)

### Orientation & Handedness
Objects shall be interpreted as +Z forward, Y up.
