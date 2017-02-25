Lifecycle methods are functions that can be attached to virtual nodes in order to access actual DOM elements when they are created, updated or before they are removed.

* _oncreate_: Called before an element is created.

* _onupdate_: Called before an element is updated.

* _onremove_: Called before an element is removed.

Signature: `(element)`.

* `element` is the real DOM element.


```jsx
const repaint = (canvas, model) => {
    const context = canvas.getContext("2d")
    context.fillStyle = "white"
    context.fillRect(
        0,
        0,
        canvas.width,
        canvas.height
    )
    context.beginPath()
    context.arc(
        model.x,
        model.y,
        50,
        0,
        2 * Math.PI
    )
    context.stroke()
}

app({
    model: { x: 0, y: 0 },
    reducers: {
        move: model => ({
            x: model.x + 1,
            y: model.y + 1,
        }),
    },
    subscriptions: [
        (_, actions) =>
            setInterval(_ => actions.move(), 60),
    ],
    view: model =>
        <canvas
            width="600"
            height="300"
            onupdate={e => repaint(e, model)}
        />
})
```

[View online](http://codepen.io/jbucaran/pen/MJXMQZ/).