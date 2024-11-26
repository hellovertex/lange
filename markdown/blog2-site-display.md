# Some notes on making my site or webgl app prettier

# Table of Contents

1. [Glossary](#glossary)
2. [CSS](#css)
3. [HTML](#html)
4. [Javascript](#javascript)

## Glossary

`Device Pixel Ratio (DPR)`

- #(physical screen pixels) / #(software pixel units)
- older devices had `DPR` equal to 1, now values of 2 and >=3 are more common
- note `DPR = 2 ` ==> 4 times as many pixels to render
  - for each software pixel there will be two physical pixels that need to be rendered
  - e.g (1,2) and (2,1) physical screen pixels require two logical pixel renders each
  - Pixels at DPR 2=(2×Width)×(2×Height)=4×(Width×Height)\

`Display areas`

- screen (all)
- window (browser)
- viewport (render) --> `window.innerWidth, window.innerHeight`

## CSS

```css
body
{
  /*remove white borders*/
  margin: 0;
  padding: 0;
}

.webgl
{
  /*Remove scroll bar for webgl*/
  position: fixed;
  top: 0;
  left: 0;
  /*Remove blue outline on some devices*/
  outline: none;
}
html,
body
{
  /*Prevent scrolling beyond page*/
  overflow: hidden
}
```

## Javascript

Change webgl render using threejs

```js
const sizes = {
  width: window.innerWidth,
  height: window.innerHeight
}

const camera = new THREE.PerspectiveCamera(75, sizes.width / sizes.height, 0.1, 100)
const renderer = new THREE.WebGLRenderer({
  canvas: canvas,
  antialias: true
})
// Resize Event
window.addEventListener('resize', () =>
{
  // resize window
  sizes.width = window.innerWidth
  sizes.height = window.innerHeight
  // Update Camera
  camera.aspect = sizes.width / sizes.height
  camera.updateProjectionMatrix()
  // Update Renderer
  renderer.setSize(sizes.width, sizes.height)
  // update pixelRatio on resize event, as user might change screen
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
})

// Fullscreen Event
// Fullscreen Event
window.addEventListener('dblclick', async () => {
  if (!document.fullscreenElement) {
    await canvas.requestFullscreen()
  } else {
    await document.exitFullscreen()
  }
})
```
