# WebSimplify (Not publishable yet)

**WebSimplify** is a free, lightweight C++ framework for building **modern Windows applications** without touching the Win32 API.

It allows you to create **borderless Fluent-style windows** and render **real HTML, CSS, and JavaScript** using the Chromium engine (WebView2).  
Perfect for modern desktop apps, tools, and JavaScript-powered experiences such as lightweight games — fast, clean, and accessible.

---

## Why WebSimplify?

- No Win32 boilerplate — focus on features, not Windows internals
- Modern Fluent-style UI out of the box
- Chromium rendering (HTML / CSS / JS)
- Fully customizable CSS and layout
- Secure by default
- Lightweight and powerfull

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
    theme: WebSimplify::Theme::System,
    logs: true, // save user changes on app start
    resizable: true,
    frameless: true,
    position: 5 // 5 is middle-middle, 1 b-l, 2 b-m, 3 b-r,
});
```


```cpp
WebSimplify::run();      // Start event loop
WebSimplify::close();    // Close application
WebSimplify::restart(); // Restart app
```

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
```

##### Also Theme & Appearance but not fully optimised
```cpp
WebSimplify::setAccentColor("#6c47ff");
WebSimplify::enableBlur(bool);
WebSimplify::setCornerRadius(int);
```

##### Web Content Rendering
```cpp
WebSimplify::setHTML("<html>...</html>");
WebSimplify::setBody("<div>...</div>");
WebSimplify::load("https://example.com");
WebSimplify::reload();


WebSimplify::executeJS("console.log('Hello')");

```
Do not call load method more than once in a loop, it will crash 
```cpp
WebSimplify::injectScript("C://User/script.js");
```

##### Web Content CSS Styling
The css is rendered first in cache so if content is <p style="color: red">j</p> and in back it's p {color:blue} the final color will be still red!
```cpp
WebSimplify::setCSS("body { background: black; }");
WebSimplify::appendCSS(".btn { color: red; }");
WebSimplify::clearCSS();
```
##### CSS Styling
```cpp
WebSimplify::setCSS("body { background: black; }");
WebSimplify::appendCSS(".btn { color: red; }");
WebSimplify::clearCSS(); // this clears all css, and no new css works, use WebSimplify::css:restart(); to restart the css engine
```
##### Optimised UI Components
Buttons
```cpp
WebSimplify::addButton("Label", "Label", callback);
WebSimplify::removeButton("Label");
WebSimplify::enableButton("Label", newcallback); // replaces the original onclick
```
Dropdowns
```cpp
WebSimplify::addDropdown("menu", {
    "Option 1",
    "Option 2",
    "Option 3"
});

WebSimplify::onDropdownChange("menu", callback);
```
### Input Fields

WebSimplify provides **native-managed input components** that stay fully synced with the web layer.

Inputs are identified by a unique name and can be styled using custom CSS.

#### Create an Input

```cpp
WebSimplify::addInput({
    .id = "username",
    .placeholder = "Enter username",
    .type = WebSimplify::InputType::Text
});

// Supported Input Types
WebSimplify::InputType::Text
WebSimplify::InputType::Password
WebSimplify::InputType::Email
WebSimplify::InputType::Number
WebSimplify::InputType::Search
```
Get & Set Values compatible to c++ 

```cpp
std::string value = WebSimplify::getInputValue("username");
WebSimplify::setInputValue("username", value);
```

Input State Control
```cpp
WebSimplify::enableInput("username", true);
WebSimplify::focusInput("username");
WebSimplify::clearInput("username");
```
Input Events
```cpp
WebSimplify::onInputChange("username", [](std::string value) {
    // Called on every value change
});
WebSimplify::onInputSubmit("username", [](std::string value) {
    // Called when user presses Enter, it's not a form only enter of keyboard
});
```
Remove Input (do not recall this input before a init or app will crash)
```cpp
WebSimplify::removeInput("username");
```

Styling Inputs with CSS
```cpp
WebSimplify::appendCSS(R"(
    input { // or #username for the example above
        padding: 12px;
        border-radius: 10px;
        border: 1px solid #333;
        background: #111;
        color: white;
    }

    input:focus {
        border-color: #6c47ff;
        outline: none;
    }
)");
```
### Notes
Inputs are native-managed and web-rendered inputs may be slow:
    - Do not call: `<input id="username" />`
    - Call it: `WebSimplify::addInput({
                    .id = "username",
                    .placeholder = "Enter username",
                    .type = WebSimplify::InputType::Text
                });`

Values stay synchronized between C++ and JavaScript

Input lifecycle is controlled via addInput() and removeInput()

Use destroy() on components to stop listeners and free resource
