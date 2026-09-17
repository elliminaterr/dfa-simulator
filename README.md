# DFA Simulator

An interactive deterministic finite automaton simulator that runs in the browser.
Pick a machine, type an input string, and step through it one symbol at a time — the
current state and the transition being taken are highlighted, and each move is logged
as an application of the transition function.

**Live demo:** https://elliminaterr.github.io/dfa-simulator/

## Features

- Four built-in machines: strings ending in `01`, even number of `1`s, strings containing
  `aba`, and binary numerals divisible by 3
- Forward stepping, backward stepping, and continuous run with a speed control
- State diagram rendered as SVG — accepting states drawn as double circles, self-loops as
  arcs, and opposing transitions curved to opposite sides so they never overlap
- Input tape showing consumed, current and remaining symbols
- Trace panel recording each step as `δ(q, a) = q′`
- Input validated against the machine's alphabet; undefined transitions halt and reject
  with an explanation rather than failing silently
- New machines can be defined as JSON in the page itself, without editing the code
- Keyboard shortcuts: `→` step, `←` back, `space` run/pause
- Works in light and dark mode, down to 400px wide

## Running it

No build step and no dependencies. Open `index.html` in a browser, or serve the folder:

```
python3 -m http.server 8000
```

## Defining a machine

State positions are given explicitly rather than laid out automatically — for teaching
diagrams a hand-placed layout is clearer than anything a force-directed algorithm
produces, and it keeps the code small. The viewBox is measured from the rendered
geometry, so a new machine never needs its bounds tuned by hand.

```json
{
  "name": "Even number of 1s",
  "description": "Accepts strings over {0, 1} with an even number of 1s.",
  "alphabet": ["0", "1"],
  "sample": "10110",
  "states": [
    { "id": "E", "label": "even", "x": 140, "y": 140, "start": true, "accept": true },
    { "id": "O", "label": "odd",  "x": 330, "y": 140 }
  ],
  "transitions": [
    { "from": "E", "to": "E", "symbols": ["0"] },
    { "from": "O", "to": "O", "symbols": ["0"] },
    { "from": "E", "to": "O", "symbols": ["1"], "curve": -50 },
    { "from": "O", "to": "E", "symbols": ["1"], "curve": -50 }
  ]
}
```

`curve` bends a transition perpendicular to the straight line between the two states, so
`A → B` and `B → A` sit on opposite sides. `loopDir: "down"` draws a self-loop below its
state instead of above.

