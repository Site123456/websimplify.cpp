# WebSimplify (Free & Open Source)

**WebSimplify** is a free, lightweight C++ framework for building **modern Windows applications** without touching the Win32 API.

It allows you to create **borderless Fluent-style windows** and render **real HTML, CSS, and JavaScript** using the Chromium engine (WebView2).  
Perfect for modern desktop apps, tools, and JavaScript-powered experiences such as lightweight games — fast, clean, and accessible.

---

## Why WebSimplify?

- No Win32 boilerplate — focus on features, not Windows internals
- Modern Fluent-style UI out of the box
- Real Chromium rendering (HTML / CSS / JS)
- Fully customizable CSS and layout
- Secure by default
- Lightweight and fast
- Completely free to use

---

## Core Features

- Borderless, rounded Fluent-style window  
- Draggable title bar and resizable edges  
- Light and Dark theme support  
- Chromium-based rendering (WebView2)  
- Full HTML, CSS, and JavaScript support  
- Native ↔ JavaScript communication  
- Built-in UI helpers (buttons, dropdowns, URL loader)  
- Native and JavaScript HTTP requests  

---

## Quick Start (Hello World)

```cpp
#include <WebSimplify.h>

int main() {
    WebSimplify::init({
        .title = "WebSimplify App",
        .width = 900,
        .height = 600,
        .theme = WebSimplify::Theme::Dark
    });

    WebSimplify::setHTML(R"(
        <h1>Hello WebSimplify</h1>
        <button onclick="send()">Click me</button>

        <script>
          function send() {
            WebSimplify.send("clicked");
          }
        </script>
    )");

    WebSimplify::onMessage("clicked", []() {
        WebSimplify::showMessage("Button clicked!");
    });

    WebSimplify::run();
}
```

### Application & commands

##### Main loops and controls

```cpp
WebSimplify::init({
    title: "App Title",
    width: 900,
    height: 600,
    minWidth: 400,
    minHeight: 300,
    theme: WebSimplify::Theme::Dark,
    resizable: true,
    frameless: true,
    centered: true
});
```

WebSimplify::run();      // Start event loop
WebSimplify::close();    // Close application
WebSimplify::restart(); // Restart app

##### Window controls

```cpp
WebSimplify::setTitle("New Title");
WebSimplify::setSize(width, height);
WebSimplify::setMinSize(width, height);
WebSimplify::setMaxSize(width, height);
```
- Main window cuts, these reset setMinSize and setMaxSize as null
```cpp
WebSimplify::maximize();
WebSimplify::minimize();
```

```cpp
WebSimplify::setResizable(bool);
WebSimplify::setDraggable(bool);
WebSimplify::setAlwaysOnTop(bool);
```
##### Theme & Appearance
```cpp
WebSimplify::setTheme(WebSimplify::Theme::Light);
WebSimplify::setTheme(WebSimplify::Theme::Dark);
WebSimplify::toggleTheme();


WebSimplify::setAccentColor("#6c47ff");
WebSimplify::enableBlur(bool);
WebSimplify::setCornerRadius(int);

```
