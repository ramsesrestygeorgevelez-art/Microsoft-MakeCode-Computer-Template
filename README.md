# Microsoft MakeCode Computer Template

If you want an computer in MakeCode Arcade, use this code in TypeScript:

```ts
namespace SpriteKind {
    export const Cursor = SpriteKind.create()
    export const App = SpriteKind.create()
    export const TaskbarApp = SpriteKind.create()
}

// 🎯 Cursor
let cursor = sprites.create(assets.image`cursor`, SpriteKind.Cursor)
browserEvents.onMouseMove(function (x: number, y: number) {
    cursor.setPosition(x, y)
})

// 🖌️ Draw app icon + window
let drawIcon = sprites.create(assets.image`drawApp`, SpriteKind.App)
drawIcon.setPosition(10, 10)
let isInvis = false

let drawWindow = sprites.create(assets.image`draw`, SpriteKind.App)
drawWindow.setFlag(SpriteFlag.Invisible, true)
isInvis = true
drawWindow.setScaleCore(0.1, 0.1) // start tiny

// 🧩 Taskbar asset
let taskbar = sprites.create(assets.image`taskbar`, SpriteKind.App)
taskbar.setScaleCore(5,4)
taskbar.setPosition(110, 100) // start off-screen (below bottom edge)
taskbar.setFlag(SpriteFlag.Invisible, false)

// 🧠 State
let drawOpen = false
let targetScale = 0.1
let animating = false

let taskbarVisible = false
let taskbarAnimating = false

if (drawOpen){
    let clone = drawIcon.image.clone()

    let sprite = sprites.create(clone, SpriteKind.TaskbarApp)
    sprite.setPosition(115,100)
    
}
// 🔄 Smooth animation loop for drawWindow
game.onUpdate(function () {
    if (!isInvis) {
        let current = drawWindow.scale
        let diff = targetScale - current
        if (Math.abs(diff) > 0.01) {
            let next = current + diff * 0.2
            drawWindow.setScaleCore(next, next)
        } else if (!drawOpen && !animating) {
            drawWindow.setFlag(SpriteFlag.Invisible, true)
        }
    }
})


// 🖱️ Mouse click handling
browserEvents.MouseAny.onEvent(browserEvents.MouseButtonEvent.Pressed, function (x: number, y: number) {
    // Toggle draw app
    if (cursor.overlapsWith(drawIcon) && !animating) {
        animating = true
        drawOpen = !drawOpen
        if (drawOpen) {
            drawWindow.setFlag(SpriteFlag.Invisible, false)
            isInvis = false
            targetScale = 5
        } else {
            targetScale = 0.1
        }
        control.runInParallel(function () {
            pause(500)
            animating = false
        })
    }
})
// draw window functionality
type RGB = {
    R: number
    G: number
    B: number
}
function convertRgbToColor(rgb: RGB){
    let r = rgb["R"]
    let g = rgb["G"]
    let b = rgb["B"]

    

}
function drawColor(color: RGB) {
   convertRgbToColor({R: 100, G: 100, B: 100})
}
```

Problems:
* Add the sprites.
* Make your own Rgb converter.
