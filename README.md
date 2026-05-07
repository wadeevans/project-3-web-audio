# Web Audio API — Foundational Tutorial

## The Core Mental Model: A Signal Graph

Everything in Web Audio is a **graph of nodes**. Audio flows from sources through 
processing nodes to a destination (your speakers). You wire nodes together with `.connect()`.

1. AudioContext — The Root of Everything
javascriptconst ctx = new AudioContext();
The AudioContext is the factory and the timeline. You use it to:

Create every node (ctx.createOscillator(), ctx.createGain(), etc.)
Read the current time (ctx.currentTime) — a high-precision clock in seconds
Access the final output (ctx.destination)

One important rule: browsers block audio until a user gesture. You must create or resume the context inside a click/keydown handler.
javascript// Pattern you'll use constantly
button.addEventListener('click', () => {
  if (ctx.state === 'suspended') ctx.resume();
  // ... make sound
});

2. Your First Sound — Oscillator
An OscillatorNode generates a periodic waveform. It's a source node (nothing connects into it).
javascriptconst osc = ctx.createOscillator();
osc.type = 'sine';        // 'sine' | 'square' | 'sawtooth' | 'triangle'
osc.frequency.value = 440; // Hz — A4

osc.connect(ctx.destination); // wire to speakers
osc.start();                  // begin generating audio
osc.stop(ctx.currentTime + 1); // stop after 1 second
Key detail: oscillators are one-shot. Once stopped, you can't restart them. Create a new one each time you need a note. This sounds wasteful but is intentional — nodes are cheap.