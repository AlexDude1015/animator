# Animator

Write a short program, and watch it draw itself.

Animator is a browser app for program-generated drawings. You write a `draw`
function in a small Gleam-like language, and the app runs it and animates the
lines it returns, stroke by stroke. It can also turn a photo into line art that
draws itself the same way.

**Try it: https://alexdude1015.github.io/animator/**

## Features

- **Live code editor.** Every edit runs your code again and restarts the
  animation. Errors show up in place of the drawing.
- **Shape slider.** Its value (0 to 100) is passed to `draw`, so a program can
  change its own shape: more branches on a tree, more turns on a spiral.
- **Speed slider.** Plays the animation from -2x to 2x. 0 pauses it, and
  negative speeds play it backward.
- **Drawing library.** Switch between drawings, create new ones, and rename or
  delete them. Everything is saved in your browser, so your drawings are still
  there next time. It starts with four examples: a fan, a square spiral, string
  art, and a tree.
- **Image to line art.** Upload a photo and it becomes a new drawing. The app
  finds the outlines and shades the dark areas with hatching, like an engraving.
  Detail and Shading sliders control how much is kept, with a live preview.
  The result is ordinary draw code, so you can play it at any speed or edit it.

### Image to line art: before and after

| Photo | Line art drawn by Animator |
| :---: | :---: |
| <img src="screenshots/before.jpg" alt="The original photo" width="300"> | <img src="screenshots/after.jpg" alt="The same photo as line art, with outlines and hatched shading" width="300"> |

## Writing a drawing

A drawing is a `draw` function that returns a list of paths. Each path is a
shape, the times (in tenths of a second) when it starts and finishes drawing,
and a thickness. The canvas runs from -500 to 500 in both directions, with
(0, 0) in the centre and y growing downward.

```gleam
fn draw(shape_slider: Int) -> List(Path) {
  fan(30, Point(-200, -200))
}

fn fan(n: Int, start: Point) -> List(Path) {
  case n > 0 {
    True -> [Path(Line(start, Point(n * 10, 200)), 0, 30, 3), ..fan(n - 1, start)]
    False -> []
  }
}
```

The language has whole numbers, `let`, `case`, recursion, `Point`, `Line`,
`Path`, `random`, `abs`, and `list.map`, `list.filter`, `list.append`,
`list.length`, and `list.flatten`. The **Built-ins** panel under the editor
lists them all.

## Running it

**Online:** nothing to install. Open https://alexdude1015.github.io/animator/
in a modern browser.

**On your own computer:** this repository holds the built app (`index.html`,
`animator.css`, and `app.mjs`). Browsers only load it from a web server, not by
opening `index.html` as a file, so serve the folder with any static server:

```sh
git clone https://github.com/AlexDude1015/animator.git
cd animator
python -m http.server 8000     # or: npx serve
```

Then open http://localhost:8000. The code editor loads from a CDN, so you need
an internet connection either way.

## About

Built in [Gleam](https://gleam.run) with [Lustre](https://lustre.build) for
UC Berkeley's CS 61A Animator project. This repository contains only the
compiled, published app.
