# SPA Boilerplate

The following is starting styles suitable for when you wanted content centered, for example when
creating a single page application -

```CSS
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Roboto:wght@400;700&display=swap');

* {
    box-sizing: border-box;
}

body {
    font-family: 'Roboto', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    overflow: hidden;
    margin: 0;
}
```

This code centers content on the page and sets it to be in a column format.

Overflow is set to hidden to remove scrollbars.

The Roboto font is imported (a very popular font for websites)