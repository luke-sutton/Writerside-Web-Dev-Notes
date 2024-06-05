# Overlay

This CSS can be used for an overlay, for example can be used around a modal to darken the background
and prevent any buttons from working behind it. It is also useful for calling a function on to close the modal
when a user clicks outside of it.

```CSS
.overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 1000;
}
```