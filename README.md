![Dendry](http://dendry.org/img/logo_64.png) Dendry
======

[![Build Status](https://travis-ci.org/dendry/dendry.svg?branch=master)](https://travis-ci.org/dendry/dendry)
[![Dependencies Status](https://david-dm.org/dendry/dendry.svg)](https://david-dm.org/dendry/dendry)
[![Test Coverage](https://coveralls.io/repos/dendry/dendry/badge.png?branch=master)](https://coveralls.io/r/dendry/dendry)

This is a new project aimed at developing a suite of command line
tools and a common file format for hypertext interactive fiction. My
ability to work on this project is minimal, so until I remove this
message and describe the contents of this repo, assume that the tools
are incomplete, missing or non-functional.

To install:

`sudo npm install -g .`

To create a new project:

`dendry new`

To create a playable html/js from a dendry project:

`dendry make-html`


## Dendry scene keywords

These are elements that can be used following a scene declaration. A scene declaration is a line that contains `@scene_name`.

Note: all of these can be written in camelCase or dash-between-words.

```
    title: string
    subtitle: string
    unavailable-subtitle: string
    view-if: boolean expression
    choose-if: boolean expression
    on-arrival: semicolon-separated list of commands
    on-departure: semicolon-separated list of commands
    go-to: name of scene, or semicolon-separated list of the format 'scene1 if <condition>'
    tags: comma-separated list of tags
    max-visits: int
    priority: int (I'm not sure how this works yet)
    frequency: I'm not sure how this works yet

    is-special: boolean
    set-bg: path to background image, or hex color
    set-jump: name of scene
```

## Special scene names

`prevScene` - always goes to the previous scene.

`jumpScene` - always goes to the scene designated by the last `set-jump` command.

`backSpecialScene` - goes to the previous scene visited before entering the last visited special scene (a special scene is designated by `is-special: true`).


## Dendry syntax reference

All of these are elements that can be used in the body text.

```
    *some words* - emphasis
    **some words** - strong emphasis
    > paragraph - quotation
    >> paragraph - attribution
    = paragraph - heading
    // + <newline> - manual line break
    <blank line> - paragraph break
    --- - horizontal rule / break
    [some words] - hidable section
    [+ foo : bar +] - insert quality value with optional qdisplay
    [? if condition: text ?] - conditional display of text
    {!<span class="foo">aaa</span>!} - raw html
    #comment - comment
```

## Examples

Displaying variables in text: `[+ var : +]`

Varying text based on a condition: `[? if var = 1 : something ?]`

Basic scene example:

```
title: scene0
go-to: scene1

# this is the start of the .scene.dry file.

@scene1
title: Scene
subtitle: subtitle of the scene
unavailable-subtitle: scene cannot be selected
view-if: var1 = 1 and (var2 = 2 or var3 = 3)
choose-if: var4 = 4
on-arrival: v2 = 1; v3 = 3; vs = "abc"
tags: start, tag1, tag2
new-page: true
max-visits: 2

Content goes here.

var1: [+ var1 +]

vs: [+ vs +]

[? if var1 = 1 : aaaaa ?]

# these are links

- @scene2: Choice 1
- @scene3: Choice 2


@scene2

Content for scene2 goes here.

@scene3

Content for scene3 goes here.
```

Including javascript in `on-arrival`: `{! Q['var1'] = Math.cos(Math.PI/4); !}`

Including javascript in `view-if`: `{! return ((Q['a'] || 0)===(Q['b'] || 0)); !}`


## Debugging

In the browser, the state is stored as `dendryUI.dendryEngine.state`. Qualities are at `dendryUI.dendryEngine.state.qualities`.
