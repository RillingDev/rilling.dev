---
title: "Creating Oscillators with JavaScript"
date: 2015-07-03
updated: 2023-08-24
tags:
    - JavaScript
    - Audio
description: "This article shows how the Web Audio API can be used to create an oscillator that produces a simple sound."
---

The [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) is a JavaScript API that allows for complex audio operations in JavaScript, such as creating oscillators or applying audio effects. All modern browsers [support it](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API#browser_compatibility).

This article shows how the Web Audio API can be used to create an oscillator that produces a simple sound. You can try the result on [CodePen](https://codepen.io/RillingDev/pen/MWorWmG).

<!-- more -->

## General Concepts

The following concepts are fundamental to understanding the Web Audio API:

Audio node
: A [node](<https://en.wikipedia.org/wiki/Node_(computer_science)>) that has an audio signal as its in- and/or output. There are sound-producing nodes, as well as nodes that process and modify sounds.

Oscillators
: In this context, this refers to an [electronic audio oscillator](https://en.wikipedia.org/wiki/Electronic_oscillator). It creates a basic audio signal and is thus a sound-producing audio node.

Audio routing graph
: A [graph](<https://en.wikipedia.org/wiki/Graph_(abstract_data_type)>) of audio nodes that routes the audio signal through them. In simple cases, it can act like a "pipeline" where the audio signal from a sound-producing node travels through some other nodes until it reaches the audio output.

## Initializing the Web Audio API

To use the Web Audio API, we first have to create an [`AudioContext`](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext). This acts as the audio routing graph.

```javascript
const audioCtx = new AudioContext();
```

Then add a basic oscillator node to the context, using the [`OscillatorNode`](https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode) constructor:

```javascript
const oscNode = new OscillatorNode(audioCtx, {
	/*
	 * Several common waveforms are available such as "sine", "square" or "sawtooth".
	 * For a full list see https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode/type.
	 */
	type: "sine",
	/*
	 * The frequency of the oscillator in hertz, 440 equals an "A4" key on a piano.
	 * See https://en.wikipedia.org/wiki/Piano_key_frequencies
	 * for mapping between keys and frequencies.
	 */
	frequency: 440,
});

/*
 * Directly connect the output of the oscillator to the audio output.
 * Later additional nodes could be inserted between the two.
 */
oscNode.connect(audioCtx.destination);
```

By default, the oscillator is in an "off" state and does not create any sound. The methods [`OscillatorNode#start`](https://developer.mozilla.org/en-US/docs/Web/API/AudioScheduledSourceNode/start) and [`OscillatorNode#stop`](https://developer.mozilla.org/en-US/docs/Web/API/AudioScheduledSourceNode/stop) toggle this.

```javascript
// Start playing sound. WARNING: can be loud!
oscNode.start();
// Stop sound after 3 seconds.
setTimeout(() => oscNode.stop(), 3000);
```

At this point, you should be hearing the newly created oscillator play a sound in the key A4 for 3 seconds.

## Controlling the Volume

To control the output volume, we can create a [`GainNode`](https://developer.mozilla.org/en-US/docs/Web/API/GainNode), and insert it between the oscillator, and the audio output.

```javascript
const audioCtx = new AudioContext();

const oscNode = new OscillatorNode(audioCtx, {
	type: "sine",
	frequency: 440,
});

const gainNode = new GainNode(audioCtx, {
	// The default value is `1`. We reduce the volume to half of that.
	gain: 0.5,
});

/*
 * We route our audio signal from the oscillator node
 * through the gain node, and then to the output.
 */
oscNode.connect(gainNode).connect(audioCtx.destination);

oscNode.start();
setTimeout(() => oscNode.stop(), 3000);
```

## Adding Effects

All kinds of audio effects can be applied to the audio signal. Here we add a [`PannerNode`](https://developer.mozilla.org/en-US/docs/Web/API/PannerNode) to pan the audio signal to the right stereo channel.

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
 * which can be used to change how the audio signal is distributed
 * between the stereo channels.
 */
const pannerNode = new PannerNode(audioCtx, {
	/*
	 * The default is `0`.
	 * `1` moves the audio signal to the right stereo channel.
	 */
	positionX: 1,
});

/*
 * We route our audio from the oscillator through the effect node,
 * through the gain node, and from there to the output.
 */
oscNode.connect(pannerNode).connect(gainNode).connect(audioCtx.destination);

oscNode.start();
setTimeout(() => oscNode.stop(), 3000);
```

For a list of all effects see [MDNs list of audio effects filters](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API#defining_audio_effects_filters).

## Dynamically Changing Audio Parameters

Several of the parameters defined for the preceding nodes such as the `frequency` of an oscillator or the `gain` of the gain node can be changed dynamically while the audio is playing. However, it is best to not directly write to the corresponding property but to use the methods available for [`AudioParam`](https://developer.mozilla.org/en-US/docs/Web/API/AudioParam), the underlying interface. These allow for much more control over when and how the values change such as with [`AudioParam#setValueAtTime`](https://developer.mozilla.org/en-US/docs/Web/API/AudioParam/setValueAtTime).

## Additional Resources

- [Mozilla Developer Network on the Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
