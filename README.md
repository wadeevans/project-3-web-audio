# Web Audio API — Foundational Tutorial

## The Core Mental Model: A Signal Graph

Everything in Web Audio is a **graph of nodes**. Audio flows from sources through
processing nodes to a destination (your speakers). You wire nodes together with `.connect()`.

---

## 1. AudioContext — The Root of Everything

```javascript
const ctx = new AudioContext();
```

The `AudioContext` is the factory and the timeline. You use it to:
- Create every node (`ctx.createOscillator()`, `ctx.createGain()`, etc.)
- Read the current time (`ctx.currentTime`) — a high-precision clock in seconds
- Access the final output (`ctx.destination`)

Browsers block audio until a user gesture. You must create or resume the context
inside a click/keydown handler.

```javascript
button.addEventListener('click', () => {
  if (ctx.state === 'suspended') ctx.resume();
  // ... make sound
});
```

---

## 2. Your First Sound — OscillatorNode

An `OscillatorNode` generates a periodic waveform. It is a source node —
nothing connects into it.

```javascript
const osc = ctx.createOscillator();
osc.type = 'sine';         // 'sine' | 'square' | 'sawtooth' | 'triangle'
osc.frequency.value = 440; // Hz — A4

osc.connect(ctx.destination);
osc.start();
osc.stop(ctx.currentTime + 1);
```

Oscillators are one-shot. Once stopped, create a new one. Nodes are cheap.

---

## 3. GainNode — Volume Control

Raw oscillator output is loud. A `GainNode` scales amplitude. `1.0` = unity, `0.0` = silence.

```javascript
const osc  = ctx.createOscillator();
const gain = ctx.createGain();

gain.gain.value = 0.3; // 30% volume

osc.connect(gain);
gain.connect(ctx.destination);

osc.start();
osc.stop(ctx.currentTime + 1);
```

---

## 4. Constructor Style — Modern Syntax

The factory methods (`ctx.createOscillator()`) are legacy. The modern approach 
uses constructors directly, passing options in one go:

```javascript
// Legacy factory style
const osc = ctx.createOscillator();
osc.type = 'sine';
osc.frequency.value = 220;

// Modern constructor style
const osc = new OscillatorNode(ctx, { type: 'sine', frequency: 220 });
const gain = new GainNode(ctx, { gain: 0.3 });
```

All Audio nodes follow this pattern: `new NodeName(ctx, { options })`.

---

## Steps

| File | Topic |
|---|---|
| `steps/00-skeleton.html` | Base page structure |
| `steps/01-audio-context.html` | AudioContext, initialise Web Audio API |
| `steps/02-first-sound.html` | OscillatorNode, first sound |
| `steps/03-turn-it-down.html` | GainNode, volume control |
| `steps/04-constructor-style.html` | Modern constructor syntax |