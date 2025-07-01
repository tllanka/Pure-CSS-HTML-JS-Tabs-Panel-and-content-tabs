# Pure CSS, HTML, JS for Tabs Panel navigation OR content tabs for UI segments !

Tab panel navigation and content nesting with native HTML, CSS and JS. No frameworks, no libraries, no radio button hacks ! ALL in pure HTML and basic JS code. Extensible and flexible for content nesting into tabs, including lazy loading content into tab containers or generating content into the separate tabs.  Elegant, simple and as easy as HTML.


Very much perfect for the UI of HTML web apps, or when the content has multiple layers that we
need to layer with tabs that are scroll sticky or nested within other content.






# Modern & Accessible Tabs
### Pure HTML, CSS, and JavaScript

<!-- Badges -->
<p>
  <a href="LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT">
  </a>
  <img src="https://img.shields.io/badge/Maintained%3F-yes-green.svg" alt="Maintained">
  <img src="https://img.shields.io/badge/Made%20with-HTML%2C%20CSS%2C%20JS-1f425f.svg" alt="Made with HTML, CSS, JS">
  

## LIVE DEMO of the TAbs
<a href="https://dorson.github.io/Pure-CSS-HTML-JS-Tabs-Panel-and-content-tabs/">

SEE LIVE DEMO. . . . .     
https://dorson.github.io/Pure-CSS-HTML-JS-Tabs-Panel-and-content-tabs/
. . . . . . .
</a>
  
</p>

A modern, standalone implementation of accessible, JavaScript-driven tabs. This project demonstrates how to build a flexible and feature-rich tab component using modern web standards, moving away from outdated hacks.

![Live Demo of the Tab Component](https://user-images.githubusercontent.com/12556889/134862458-767a1a8a-7956-4b6a-9286-0a373e35a1a1.gif)
> **Note:** The GIF above is a placeholder. You should replace it with a screen recording of your actual application and link to a live demo.

## 📚 Table of Contents
- [🚀 Features](#features)
- [⚙️ How It Works](#how-it-works)
- [⚡️ Quick Start](#quick-start)
  - [1. HTML Structure](#1-html-structure)
  - [2. CSS Styles](#2-css-styles)
  - [3. JavaScript Logic](#3-javascript-logic)
- [♿ Accessibility Details](#accessibility-details)
- [🤝 Contributing](#contributing)
- [📜 License](#license)


## 🚀 Features

-   **✅ JavaScript-Driven:** No more "radio button" or `:target` hacks. Logic is handled cleanly in JavaScript with no external dependencies.
-   **♿ Fully Accessible:** Implements WAI-ARIA patterns, including `role="tablist"`, `role="tab"`, and `role="tabpanel"`, along with full keyboard navigation.
-   **⌨️ Roving `tabindex`:** An efficient and intuitive keyboard tabbing experience that only focuses the active tab.
-   **🎨 Themable & Responsive:** Uses CSS Custom Properties for easy light/dark mode theming and is fully responsive.
-   **🪆 Nested Tabs:** The architecture gracefully supports nesting tab components within each other.
-   **🛠️ Live Code Editor:** Includes a built-in dialog to view, edit, and preview the component's code in real-time.

## ⚙️ How It Works

The core mechanism is simple and effective:

1.  A JavaScript function, `setupTabComponent`, is initialized for each `.tabs-container` element.
2.  It uses **event delegation**, listening for `click` and `keydown` events on the tab list to manage tab selection efficiently.
3.  When a tab is activated, the script:
    -   Toggles an `.active` class for CSS styling.
    -   Updates `aria-selected` to inform assistive technologies.
    -   Modifies `tabindex` attributes to implement a "roving tabindex" for seamless keyboard navigation.
4.  All visual styling and animations are handled purely by CSS, targeting the `.active` class.

This approach keeps the HTML semantic, the CSS straightforward, and the JavaScript minimal and focused.

## ⚡️ Quick Start

This is a standalone project—no build process or dependencies are needed. To use it, simply open `index.html` in your browser.

To integrate the component into your own project, follow these steps:

### 1. HTML Structure

Copy the HTML structure below. The script automatically initializes any element with the `.tabs-container` class.

```html
<!-- The main container for the tab component -->
<div class="tabs-container">
    <!-- The container for tab buttons (labels) -->
    <div class="tab-group" role="tablist" aria-label="Example Tab List">
        <button id="label-for-panel-1" class="tab-label-base active" role="tab" aria-controls="panel-1" aria-selected="true" tabindex="0">Tab One</button>
        <button id="label-for-panel-2" class="tab-label-base" role="tab" aria-controls="panel-2" aria-selected="false" tabindex="-1">Tab Two</button>
    </div>

    <!-- The container for the tab content panels -->
    <div class="tab-content-container">
        <div id="panel-1" class="tab-content active" role="tabpanel" aria-labelledby="label-for-panel-1">
            <p>Content for the first tab.</p>
        </div>
        <div id="panel-2" class="tab-content" role="tabpanel" aria-labelledby="label-for-panel-2">
            <p>Content for the second tab.</p>
        </div>
    </div>
</div>
```

> **⚠️ Important:** The `id` attributes for your tab panels (e.g., `id="panel-1"`) and their corresponding labels **must be unique** across the entire HTML document. The script relies on these unique IDs to correctly link buttons to their panels.

### 2. CSS Styles

Copy the necessary CSS from the `<style>` tag in `index.html`:
1.  **Theming Variables:** The `:root` and `html.dark` selectors, which define the color palette.
2.  **Component Styles:** The entire CSS block located between the `/* --- START: TAB COMPONENT STYLES --- */` and `/* --- END: TAB COMPONENT STYLES --- */` comments.

### 3. JavaScript Logic

Copy the code below into your project's JavaScript file. It defines the `setupTabComponent` function and initializes it on all `.tabs-container` elements when the page loads. This script handles all click and keyboard events using an efficient event delegation pattern that fully supports nested tabs.

```javascript
document.addEventListener('DOMContentLoaded', () => {
    /**
     * Initializes a single tab component.
     * @param {HTMLElement} tabContainer - The .tabs-container element.
     */
    const setupTabComponent = (tabContainer) => {
        const tablist = tabContainer.querySelector('[role="tablist"]');
        if (!tablist) return;

        const tabs = Array.from(tablist.querySelectorAll('[role="tab"]'));

        const switchTab = (newTab) => {
            if (!newTab || newTab.getAttribute('aria-selected') === 'true') {
                return; // Do nothing if already active
            }

            const currentActiveTab = tablist.querySelector('[aria-selected="true"]');

            if (currentActiveTab) {
                currentActiveTab.classList.remove('active');
                currentActiveTab.setAttribute('aria-selected', 'false');
                currentActiveTab.setAttribute('tabindex', '-1');
                const currentPanelId = currentActiveTab.getAttribute('aria-controls');
                // By scoping the query to the tabContainer, we ensure component encapsulation.
                const currentPanel = tabContainer.querySelector(`#${currentPanelId}`);
                if (currentPanel) {
                    currentPanel.classList.remove('active');
                }
            }

            newTab.classList.add('active');
            newTab.setAttribute('aria-selected', 'true');
            newTab.setAttribute('tabindex', '0');
            const newPanelId = newTab.getAttribute('aria-controls');
            // Scoping this query is crucial for robust, reusable components.
            const newPanel = tabContainer.querySelector(`#${newPanelId}`);
            if (newPanel) {
                newPanel.classList.add('active');
            }
            newTab.focus({ preventScroll: true });
        };

        // Use event delegation for click handling.
        tablist.addEventListener('click', (e) => {
            const tab = e.target.closest('[role="tab"]');
            
            // Ensure the clicked element is a tab and belongs to this specific tablist.
            if (tab && tab.closest('[role="tablist"]') === tablist) {
                switchTab(tab);
            }
        });

        // Add keyboard navigation to the tab list.
        tablist.addEventListener('keydown', (e) => {
            const tab = e.target.closest('[role="tab"]');

            // Ensure the event is from a tab within this component
            if (!tab || tab.closest('[role="tablist"]') !== tablist) {
                return;
            }

            const currentIndex = tabs.indexOf(tab);
            if (currentIndex === -1) return;

            let nextIndex = -1;

            if (e.key === 'ArrowRight') {
                e.preventDefault();
                // If not the last tab, move to the next one.
                // Otherwise, do nothing, allowing the event to bubble up to a parent tablist.
                if (currentIndex < tabs.length - 1) {
                    nextIndex = currentIndex + 1;
                }
            } else if (e.key === 'ArrowLeft') {
                e.preventDefault();
                // If not the first tab, move to the previous one.
                // Otherwise, do nothing, allowing the event to bubble up.
                if (currentIndex > 0) {
                    nextIndex = currentIndex - 1;
                }
            } else if (e.key === 'Home') {
                e.preventDefault();
                nextIndex = 0;
            } else if (e.key === 'End') {
                e.preventDefault();
                nextIndex = tabs.length - 1;
            }
            
            if (nextIndex !== -1) {
                // Stop propagation only if we are handling the navigation internally.
                // This allows arrow keys to "escape" a nested tab component.
                e.stopPropagation();
                switchTab(tabs[nextIndex]);
            }
        });
    };

    // Initialize all tab components on the page.
    document.querySelectorAll('.tabs-container').forEach(setupTabComponent);
});
```

## ♿ Accessibility Details

Accessibility is a cornerstone of this project. The implementation strictly follows the [WAI-ARIA Authoring Practices Guide (APG) for Tabs](https://www.w3.org/WAI/ARIA/apg/patterns/tabpanel/).

-   **Roles:** Correctly uses `role="tablist"`, `role="tab"`, and `role="tabpanel"`.
-   **ARIA Attributes:** Manages `aria-controls`, `aria-selected`, and `aria-labelledby` to create a clear relationship between tabs and panels.
-   **Keyboard Navigation:**
    -   `ArrowRight`/`ArrowLeft`: Move between tabs. Navigation stops at the ends to allow "escaping" nested tab panels.
    -   `Home`: Go to the first tab.
    -   `End`: Go to the last tab.
    -   `ArrowUp`/`ArrowDown`: Not handled, to allow for normal page scrolling.
-   **Focus Management:** The active tab is always focusable (`tabindex="0"`), while inactive tabs are removed from the tab order (`tabindex="-1"`), a technique known as roving tabindex.

## 🤝 Contributing

Contributions are highly welcome! If you have suggestions for improvements or want to report a bug, please follow these steps:

1.  **Check for existing issues:** Before opening a new issue, please search the existing issues to see if your problem has already been reported.
2.  **Open a new issue:** If you can't find an existing issue, please open a new one. Provide a clear title and a detailed description of the bug or feature request.
3.  **Submit a pull request:** If you'd like to contribute code, please fork the repository and create a pull request. Make sure your code follows the project's style and that you've tested your changes.

We appreciate your help in making this project better!

## 📜 License

This project is open-source and available under the [MIT License](LICENSE). See the `LICENSE` file for more details.
