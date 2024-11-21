---
title: CCJS Plvylist Slides
date: 2022-01-29
tags: ccjs
updated: 2024-11-20
---

# CCJS Plvylist Slides

---

## Plvylist

A project that started off as me wanting to know how the [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) worked turned into a combined learning experience of audio on the web and [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components).

___

## The _Why_

- I realized I made it years into developing websites and hadn't directly worked on anything audio or video related.
- I'd used embedded players from the likes of Bandcamp or Spotify, but comparing those to the default `<audio>` or `<video>` elements left me with more questions than answers as to how they work.

___

## Default `<audio>`

<iframe height="300" style="width: 100%;" scrolling="no" title="&lt;audio&gt;" src="https://codepen.io/troyvassalotti/embed/jOGNrKW?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/troyvassalotti/pen/jOGNrKW">
  &lt;audio&gt;</a> by Troy (<a href="https://codepen.io/troyvassalotti">@troyvassalotti</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

The audio API has many more options available than what can be seen in the default controls.

___

## Web Components

- Web components are custom HTML elements defined with JavaScript used to create reusable elements - with their functionality encapsulated away from the rest of your code - and utilize them in your web apps.

___

## Functionality

What features are needed for a component like this?

1. Play/pause
2. Next/previous track
3. Track progress slider
4. Meta information: artist, album, duration
5. Shuffle or loop options
6. A clickable track list

___

## Let's Check It Out

_This is your cue to open up your text editor._

- Walk through the file and how each function works.

___

## Next Steps

- [ ] Get it published on npm

___

## Links

- Demo Page: <https://troyvassalotti.github.io/plvylist/>
- Repo: <https://github.com/troyvassalotti/plvylist>
