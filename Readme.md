# VRBA Avatar Spec

## Overview

A common avatar spec is a great starting point for interoperability between virtual worlds. There are two main reasons why this is important. First, when people interact with digital content via an avatar they are adding their body to their identity, which becomes event more intimate when its an embodied avatar using XR devices, so the avatar itself becomes a crucial part of the interaction and the person feeling comfortable making that interaction. Second, content creators offer a much better user experience if people don't have to carefully create an avatar each time they bump into a piece of content.

A standardized spec for avatars allows for a multitude of interpreters made for different platforms (JavaScript, Unity, Unreal Engine etc.), so that avatars can be created once and work everywhere.

## File Format

All avatars MUST be created in the `.gltf` or `.glb` format as per the [GLTF 2.0 Spec](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0).

Avatar creation tools MUST support exporting to these formats. For example, if you're working with JavaScript and [Three.JS](https://github.com/mrdoob/three.js), you can use [this exporter](https://github.com/mrdoob/three.js/blob/dev/examples/js/exporters/GLTFExporter.js). These creations tools MUST offer a way to get the Avatar file via a downloadable file or via a URL.

## Object Names

There are many body parts to an avatar and we need to figure out a common naming system for the whole body. There are two use cases for this:

1. Attaching virtual objects, like clothes, to a specific part of the avatar.
2. Moving body parts based on sensor data.

Currently, this spec will only support the second use case. Current sensor data, captured from a Head-Mounted Display and from hand controllers, only cover the head and hands.

### Requirements

#### Head

- The head object MUST be called AvatarHead
- The head object MUST have a bone called AvatarHeadBone
- There MUST be a bone right between both eyes called AvatarEyeBone

#### Hands

- The bone for each hand MUST be called AvatarHandBoneRight and AvatarHandBoneLeft respectively

## Animations

Existing controllers, like Oculus Touch, support some form of _hand presence_ which noticeably enhances avatar embodiment. To support them, each hand SHOULD have an animation corresponding to each possible pose. Avatar interpreters MUST not error if a specific avatar does not support one or more animations.

**Please note.** Improved _hand presence_ may become more popular in the future with solutions like gloves, camera based sensors (e.g. Leap Motion or Kinect) or Valve Knuckles, in which case we will revisit this section.

### Requirements

- Hand${'Left' or 'Right'}Open
- Hand${'Left' or 'Right'}Fist
- Hand${'Left' or 'Right'}Okay
- Hand${'Left' or 'Right'}Point
- Hand${'Left' or 'Right'}Thumb
- Hand${'Left' or 'Right'}ThumbPoint

(e.g. "HandLeftOpen" or "HandRightThumb")
