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


## Steps

| File | Topic |
|---|---|
| `steps/00-skeleton.html` | Base page structure |
| `steps/01-audio-context.html` | AudioContext, initialise Web Audio API |
| `steps/02-first-sound.html` | OscillatorNode, first sound |
