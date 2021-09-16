---
title: "Creating Oscillators with JavaScript"
date: 2015-07-03
updated: 2021-09-16
tags:
    - JavaScript
    - "Web Audio API"
    - Oscillator
---

The [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) is a JavaScript API [supported by all modern browsers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API#browser_compatibility). It allows for complex audio operations in JavaScript, such as creating oscillators, routing audio sources, or applying audio effects.

As a demo, you can check out my [collaborative web synthesizer using websockets](https://github.com/FelixRilling/socketsynth).

## Initializing the Web Audio API

In order to use the Web Audio API, we first have to create an `AudioContext`. You can think of it as a graph which defines how audio signals travel between nodes, or for simple use cases, as a "pipeline" where one audio signal e.g. from an oscillator travels through some nodes to an audio output.

<!-- more -->

```javascript
const audioCtx = new AudioContext();
```

Let's add a basic oscillator to our context, using the `OscillatorNode` constructor:

```javascript
const oscNode = new OscillatorNode(audioCtx, {
    /*
     * A number of common waveforms are available such as "sine", "square" or "sawtooth".
     * For a full list see <https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode/type>.
     */
    type: "sine",
    /*
     * The frequency of the oscillator in hertz, 440 equals an "A4".
     * See <https://en.wikipedia.org/wiki/Piano_key_frequencies> for a mapping between keys and frequencies.
     */
    frequency: 440,
});

/*
 * Directly link the output of the oscillator to the audio output.
 * Later additional nodes could be inserted in between the two.
 */
oscNode.connect(audioCtx.destination);
```

By default, the oscillator is in an "off" state and does not create any sound. The methods `OscillatorNode#start` and `OscillatorNode#stop` can be used to toggle this.

```javascript
oscNode.start(); // Start playing sound. WARNING: can be very loud!
setTimeout(() => oscNode.stop(), 3000); // Stop sound after 3000ms.
```

At this point you should be hearing the newly created oscillator play a sound for 3 seconds.

## Controlling volume

In order to control the output volume, we can create a `GainNode` and insert it between the oscillator and the audio output.

```javascript
const audioCtx = new AudioContext();

const oscNode = new OscillatorNode(audioCtx, {
    type: "sine",
    frequency: 440,
});

const gainNode = new GainNode(audioCtx, {
    /*
     * The default value is '1', and the min/max are roughly '-3.4' and '3.4' respectively.
     * We reduce the volume to 50%.
     */
    gain: 0.5,
});

/*
 * We route our audio from the oscillator through the gain, and from there to the output.
 */
oscNode.connect(gainNode).connect(audioCtx.destination);

oscNode.start();
setTimeout(() => oscNode.stop(), 3000);
```

## Adding Effects

All kinds of audio effects can be applied to our audio signal. The following example uses a panning filter to pan the audio.

```javascript
const audioCtx = new AudioContext();

const oscNode = new OscillatorNode(audioCtx, {
    type: "sine",
    frequency: 440,
});

const gainNode = new GainNode(audioCtx, {
    gain: 0.5,
});

/*
 * A `PannerNode` applies a panning effect,
 * which can be used to 'move' the audio towards one side of a stereo output.
 */
const pannerNode = new PannerNode(audioCtx, {
    positionX: 1, // Default is '0', we want the audio to be moved to the right side.
});

/*
 * We route our audio from the oscillator through the effect, through the gain, and from there to the output.
 */
oscNode.connect(pannerNode).connect(gainNode).connect(audioCtx.destination);

oscNode.start();
setTimeout(() => oscNode.stop(), 3000);
```

There are many other effects that can be applied. For a list, see [MDNs list of audio effects filters](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API#defining_audio_effects_filters).

## Dynamically Changing Audio Parameters

Several of the parameters defined for the above nodes such as the `frequency` of an oscillator or the `gain` of the gain node can be changed dynamically while audio is playing. However, in order to do so, it is best to not directly change the corresponding property, but use the methods available for [`AudioParam`](https://developer.mozilla.org/en-US/docs/Web/API/AudioParam), the underlying interface. These allow for much more control on when and how the values change such as with `AudioParam#setValueAtTime`.

## Related Links:

-   [Mozilla Developer Network on the Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
