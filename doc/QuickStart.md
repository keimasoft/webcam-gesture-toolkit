# Webcam Gesture Toolkit - Quick Start

By EDC. Powered by MediaPipe.

Windows x64 only. Keep the native DLLs and `StreamingAssets/MediaPipe/hand_landmarker.task` included. The compatible Microsoft Visual C++ runtime must be installed.

## React without code

### Ready-to-use Open Palm example

Drag `Assets/EDC/WebcamGestureToolkit/Prefabs/WGT_OpenPalmIndicator.prefab` into a URP scene with a camera and light. Position the prefab where the camera can see it (the origin works with Unity's default 3D camera). Press Play and hold an open palm for about 0.15 seconds: the included cube appears. Change pose or hide your hand to hide the cube.

This sample includes its own two-hand tracking and provider; use it instead of adding `WGT_HandTracking` separately. Its root stays active while only the cube child is toggled. It uses the same stable-pose polling behavior as `WGT_Basic`, with a built-in cube mesh and the included sample material. No scene references need reconnecting. The prefab contains neither a rendering camera nor a light. Unity import and webcam behavior still require manual verification.

### Connect your own actions

1. Drag `Assets/EDC/WebcamGestureToolkit/Prefabs/WGT_HandTracking.prefab` into your scene. Use one tracking instance per webcam. The prefab selects the default webcam and enables up to two hands.
2. Add `HandGestureTrigger` to the same object. Leave Pose Source on Built In, choose Open Palm, and keep Require Stable Pose enabled.
3. In On Activated, connect an action such as `GameObject.SetActive(true)` on a separate indicator object. Connect `SetActive(false)` in On Deactivated.
4. Press Play and hold an open palm in view for about 0.15 seconds. Activation fires once; the second hand entering the same pose does not activate again. Losing the last matching hand releases the action.

For a recording, choose Recorded and enter its exact name, such as `RockOn` (not the JSON filename `RockOnLeftHand`). Add multiple triggers to the same tracking object for different actions. Triggers can also reference a provider on another object through Hands.

## React from a script

Assign the prefab's HandGestureProvider to your script and poll in LateUpdate:

Import the public API with `using EDC.WebcamGestureToolkit;`. Example components such as `GestureIndicator` are in `EDC.WebcamGestureToolkit.Samples`.

```csharp
if (hands.IsGestureStable(Gesture.OpenPalm))
{
    // Either hand holds the confirmed pose.
}
```

For an actual minimal example, add `GestureIndicator` to a separate always-active object, assign Hands and a separate indicator object, then choose a gesture.

| Question | API |
| --- | --- |
| Immediate built-in pose on either hand? | `hands.IsGestureActive(Gesture.Fist)` |
| Confirmed built-in pose on either hand? | `hands.IsGestureStable(Gesture.Fist)` |
| Immediate recorded pose? | `hands.IsRecordedGestureActive("RockOn")` |
| Confirmed recorded pose? | `hands.IsRecordedGestureStable("RockOn")` |
| Only the left hand? | `hands.StableLeft.Matches(Gesture.Fist)` |
| Only the right hand, recorded pose? | `hands.StableRight.MatchesRecorded("RockOn")` |
| Detector running / camera error? | `hands.IsRunning` / `hands.LastError` |
| Choose a camera / retry? | `hands.SelectCamera(deviceName)` / `hands.RestartCamera()` |

`IsRunning` means the detector is running, not that a hand is visible. Camera device selection is also available in the HandTrackingCamera Inspector. SelectCamera takes the exact device name; an empty name selects the default.

For C# event listeners, use a configured trigger's `Activated.AddListener(YourMethod)` and `Deactivated.AddListener(YourMethod)`. Remove listeners when your subscriber is disabled (`RemoveListener`) to avoid duplicate subscriptions on re-enable. Trigger events run on Unity's main thread, after provider updates. Set any target properties/settings before Play; changing a configured trigger at runtime is reflected on the next LateUpdate.

## Recognition behavior

- Stable results require a fresh confirming camera result after the hold interval. Changing the candidate pose clears the old stable result immediately; the new candidate must settle again. This differs from the previous provider, which retained the old stable pose during confirmation.
- Trigger activation means at least one matching hand. Deactivation happens when no hand matches, results time out, detection stops/restarts, or the trigger is disabled. A second matching hand does not generate another activation. If a listener disables the trigger during activation, deactivation can occur immediately within that callback.
- Recorded names are exact and case-sensitive; empty names and Gesture.Unknown never activate an action. Included recorded templates are hand-specific. Recording itself is an Editor authoring feature.
- Built-in recognition takes precedence over recorded poses. Custom templates are checked only when no built-in pose matches. Recordings describe individual poses, not motion sequences.
- Gesture.Index means pointing; IndexMiddle means the two-finger victory pose; ThumbExtended means the thumb is extended, regardless of whether it points upward. Other built-ins are Fist and OpenPalm.
- Camera preview, skeleton and diagnostic options belong to HandTrackingCamera. The prefab starts with a corner preview, two hands and diagnostic overlays disabled.

## Checks before release

The API, prefab and examples require Unity import/Play Mode validation. Run the readiness harness separately as documented under `Tests/Readiness/README.md`, then verify webcam, Inspector callbacks, disable/re-enable and a player build. Prefab setup and events do not establish platform compatibility by themselves.
