# Acacia Library Documentation

The **Acacia** library is a set of customizable web components and utilities for rapid construction of web application interfaces. It provides custom HTML elements, a theme system via CSS variables, responsive layout management, modals, tooltips, data grids, galleries, audio players, and much more.

## Table of Contents

1. [Installation](#installation)
2. [Getting Started](#getting-started)
3. [Application Structure](#application-structure)
4. [Theming and Customization](#theming-and-customization)
5. [Components](#components)
   - [Layout Elements](#layout-elements)
   - [Content Elements](#content-elements)
   - [Forms](#forms)
   - [Navigation](#navigation)
   - [Media](#media)
   - [Feedback and Interaction](#feedback-and-interaction)
   - [Lists and Tables](#lists-and-tables)
   - [Side Menus and Modals](#side-menus-and-modals)
6. [Utilities](#utilities)
   - [Content](#content)
   - [Renderer](#renderer)
   - [Tooltip](#tooltip)
   - [Modal](#modal)
   - [Datagrid](#datagrid)
   - [Gallery](#gallery)
   - [IdControl](#idcontrol)
7. [Examples](#examples)
8. [API Reference](#api-reference)
9. [Additional Information](#additional-information)

---

## Installation

To use Acacia in your project, copy the provided files (`acacia.css`, `acacia.js`, `content.js`, `renderer.js`) into a folder, e.g., `/lib/acacia/`. Include the files in the `<head>` of your page:

```html
<link rel="stylesheet" href="/lib/acacia/acacia.css">
<script type="module" src="/lib/acacia/acacia.js"></script>
```

Some components depend on assets (images, icons, etc.) that must be available in `/files/`. Make sure to place the required files (such as `menu.svg`, `close.svg`, `splash.svg`, etc.) in that directory or adjust the paths in the code.

---

## Getting Started

The simplest way to start an Acacia application is by using the `Acacia` class:

```javascript
import { Acacia } from '/lib/acacia/acacia.js';

const app = new Acacia(
  'My App',               // Application name (page title)
  false,                   // Hide bottom bar? (true/false)
  { ... }                  // Side menu configuration (optional)
);
```

This will create the basic structure with a top bar, main area, and bottom bar. The initial content is loaded from the `views/home` file (by default). You can customize this behavior.

For desktop applications (with a traditional top menu), use `AcaciaDesktop`:

```javascript
import { AcaciaDesktop } from '/lib/acacia/acacia.js';

const app = new AcaciaDesktop(
  'My Desktop App',
  {
    'File': [
      { title: 'New', action: () => console.log('New') },
      { title: 'Open', action: () => console.log('Open') }
    ],
    'Edit': [
      { title: 'Copy', action: () => console.log('Copy') }
    ]
  }
);
```

---

## Application Structure

Acacia defines several custom elements that structure the interface:

- `<acacia-app>`: Main application container. Contains the top and bottom bars and the main area.
- `<app-top-bar>`: Top bar, with logo, menu icon, and space for icons.
- `<app-bottom-bar>`: Bottom bar.
- `<app-main>`: Main area divided into three columns: left bar, main view, and right bar.
- `<left-bar>`, `<right-bar>`: Sidebars (visible only in landscape mode).
- `<app-view>`: Central area where the main content is rendered.

These elements are automatically created when instantiating `Acacia` or `AcaciaDesktop`. You can access the global references:

```javascript
window.ACACIA      // <acacia-app> element
window.APP         // <app-main> element
window.APPVIEW     // <app-view> element
window.TOPBAR      // Top bar
window.BOTTOMBAR   // Bottom bar
window.LEFTBAR     // Left bar
window.RIGHTBAR    // Right bar
```

---

## Theming and Customization

The appearance is controlled by CSS variables defined in `:root` in the `acacia.css` file. You can override them to create your own theme.

```css
:root {
  --Black: #000;
  --White: #fff;
  --Dark: #101520;
  --DarkBlue: #202530;
  --DarkGreen: #105020;
  --LightYellow: #FD9;
  --LightGreen: #00DD88;
  --MediumBlue: #303540;
  --Crimson: #DD2080;
  --Gradiente45: linear-gradient(45deg, #DD2080, #00DD88)1;
  --SplashImage: url("/files/splash.svg");
  cursor: url("/lib/acacia/pointer.cur") 0 0, auto;
}
```

Change any variable to modify colors, gradients, cursor, etc. The `chisel` font is loaded via `@font-face` (ensure you have the `Chisel Mark.ttf` file in the indicated path).

---

## Components

Acacia offers dozens of custom components. Below is the complete list with a brief description.

### Layout Elements

| Element | Description |
|---------|-------------|
| `<acacia-container>` | Centered container with default background, used for modals or full screens. |
| `<app-container>` | Flexible column container, used inside views. |
| `<grid-row>` | Horizontal flex row. |
| `<grid-column>` | Vertical flex column. |
| `<container-box>` | Box with height and width adjusted to content. |
| `<vertical-divider>` | Vertical divider (line). |
| `<horizontal-divider>` | Horizontal divider (line). |

### Content Elements

| Element | Description |
|---------|-------------|
| `<card-big>` | Large card with border, used for posts, messages, etc. Has helper classes `.card-left` and `.card-right`. |
| `<text-heading>` | Main heading. |
| `<text-subheading>` | Subheading. |
| `<text-paragraph>` | Justified paragraph. |
| `<text-label>` | Label (small text). |
| `<text-link>` | Custom link (uses `href` and `target` attributes). |
| `<toast-mini>` | Small toast for notifications. The `autoclose` attribute sets seconds to auto-close. |
| `<markdown-viewer>` | Markdown viewer (used together with `Renderer.Components.DocsPage`). |

### Forms

| Element | Description |
|---------|-------------|
| `<acacia-form>` | Form container with border. |
| `<fieldset>` | Styled fieldset. |
| `<acacia-slider>` | Custom slider (range). Attributes: `min`, `max`, `step`, `value`, `id`. |
| `<acacia-progress>` | Progress bar with displayed value. Attribute `value` (0-100). |
| `<input class="acacia-input">` | Styled text field. |
| `<select class="acacia-input-select">` | Styled select. |
| `<label class="acacia-checkbox">` | Custom checkbox. |
| `<label class="acacia-radio">` | Custom radio button. |

### Navigation

| Element | Description |
|---------|-------------|
| `<acacia-tabs>` | Tabs container. Contains `<acacia-tabs-item>` and `<acacia-tabs-content>`. |
| `<acacia-badge>` | Badge selector (inline tabs). Contains `<acacia-badge-item>` and `<acacia-badge-content>`. |
| `<button-squared>` | Square button. |
| `<button-rounded>` | Rounded button. |

### Media

| Element | Description |
|---------|-------------|
| `<image-gallery>` | Image gallery with navigation. Usually created via the `Gallery` class. |
| `<acacia-audio>` | Audio player with controls (play/pause, volume, progress). Attribute `src` points to the audio file. |

### Feedback and Interaction

| Element | Description |
|---------|-------------|
| `<acacia-accordion>` | Expandable panel (accordion). Contains `<accordion-item>`, `<accordion-title>`, `<accordion-content>`. |
| `<compact-list>` | Compact list with icon and title. Uses `<list-item>` and `<text-label>`. |
| `<acacia-table>` | Simple table. Uses `<table-title>`, `<table-headers>`, `<table-row>`, etc. |
| `<data-grid>` | Editable data grid with row selection. Usually created via the `Datagrid` class. |
| `<side-menu>` | Sliding side menu. Usually managed by `Renderer.SideMenu`. |

### Side Menus and Modals

| Element | Description |
|---------|-------------|
| `<side-menu>` | Side menu container. Contains `<side-menu-outside>` (semi-transparent background) and `<side-menu-container>`. |
| `<modal-container>` | Container for modals. Used internally by `Modal`. |
| `<modal-box>` | Modal box. |
| `<modal-window>` | Larger modal window. |
| `<tooltip>` and `<context>` | Floating elements for tooltips and context menus (managed by `Tooltip`). |

---

## Utilities

### Content

Object with methods for data manipulation, caching, IndexedDB, and HTTP.

| Method | Description |
|--------|-------------|
| `Content.LoadJSON(path)` | Loads a JSON file and returns a Promise with the object. |
| `Content.LoadText(path)` | Loads a text file. |
| `Content.LoadBinary(path)` | Loads a binary file (Blob). |
| `Content.ImageHandler.ResizeImg(imageElement, w, h)` | Resizes an image and returns a new image in base64. |
| `Content.TextHandler.Size(data)` | Returns the size of the data in MB. |
| `Content.TextHandler.CurrencyConverter(txt, format)` | Converts string to currency format (R$). |
| `Content.TextHandler.MarkdownParser(markdown)` | Converts markdown to HTML. |
| `Content.IndexedDB.New(dbName, tableName)` | Creates database/table. |
| `Content.IndexedDB.Check(dbName, tableName, id)` | Checks if a record exists. |
| `Content.IndexedDB.Write(dbName, tableName, data, id)` | Saves an object. |
| `Content.IndexedDB.Read(dbName, tableName, id)` | Reads an object. |
| `Content.IndexedDB.Remove(dbName, tableName, id)` | Removes a record. |
| `Content.Cache.SaveData(url, data, cacheName)` | Saves data to the Cache API. |
| `Content.Cache.GetData(url, cacheName)` | Retrieves data from cache. |
| `Content.HTTP.Get(path)` | Simple GET request. |
| `Content.HTTP.Post(path, body)` | POST request with JSON. |
| `Content.Social` | Structures for user and room data (`UserData`, `RoomData`). |

### Renderer

Responsible for loading views, managing layout, and complex components.

| Method | Description |
|--------|-------------|
| `Renderer.Load(viewName, target)` | Loads an HTML file from the `views/` folder and inserts it into the target element. |
| `Renderer.Home(hideBottomBar, callback)` | Initializes the home screen. |
| `Renderer.Desktop(callback)` | Initializes desktop mode (top menu). |
| `Renderer.DropFiles()` | Displays a drop area and returns a Promise with the dropped files. |
| `Renderer.Layout` | Object with layout methods (`Verify`, `Landscape`, `Portrait`, `BottomTopLayout`, `TopLayout`, `CleanLayout`, etc.) |
| `Renderer.SideMenu` | Manages the side menu (`Show`, `StartDrag`, `EndDrag`, etc.). |
| `Renderer.VCard` | Imports/exports profile vCard. |
| `Renderer.Emojis` | Lists of emojis and method `Insert(textBox, emoji)`. |
| `Renderer.Components` | Set of functions to render ready-made components (profile, posts, messages, comments, rooms, etc.). |

Example usage of `Renderer.Components`:

```javascript
Renderer.Components.MyProfilePage(
  id, username, name, description, thumbnail, genre, email, state,
  (userData) => { /* callback on save */ }
);
```

### Tooltip

Manages tooltips, context menus, profile popovers, and toasts.

| Method | Description |
|--------|-------------|
| `Tooltip.Tooltip(text, element)` | Displays a simple tooltip when hovering over the element. |
| `Tooltip.Context(menuItems)` | Displays a context menu (array of objects with `Ico`, `Title`, `Action`). |
| `Tooltip.ProfilePopover(user, element)` | Displays a popover with profile data (thumbnail, name, username). |
| `Tooltip.Toast(text, seconds)` | Displays an automatic toast (mini notification). |

### Modal

Modal system with different types.

| Method | Description |
|--------|-------------|
| `Modal.Waiting(title, message, callback)` | Waiting modal with loading animation. Returns a Promise. |
| `Modal.Message(title, message, callback)` | Message modal with "Confirm" button. |
| `Modal.Error(title, message, fatal, callback)` | Error modal, with fatal option (restart app). |
| `Modal.Confirm(title, message, callback)` | Confirmation modal (always calls callback). Returns a Promise with boolean. |
| `Modal.ConfirmAction(title, message, callback)` | Similar, but callback is only called if confirmed. |
| `Modal.Input(title, message, callback)` | Modal with an input field. Returns a Promise with the value. |
| `Modal.Window(title, content)` | Modal with a customizable window (HTML content). |

### Datagrid

Class to create editable data grids with row selection.

```javascript
const dataGrid = new Datagrid(
  'myGrid',                    // grid id
  'Title',                     // title
  [['Name', 'Age'], ['John', 30], ['Mary', 25]], // data
  container,                    // target element
  (selectedValues) => { ... },  // callback when rows are selected
  (count) => { ... }            // callback when all rows are selected
);
```

### Gallery

Class to create image galleries.

```javascript
const gallery = new Gallery(
  'myGallery',
  ['img1.jpg', 'img2.jpg', 'img3.jpg'],
  container
);
```

### IdControl

Unique ID generator for elements.

```javascript
IdControl.NextId('Modal'); // e.g., 'md-1'
```

---

## Examples

### Creating an application with a side menu

```javascript
import { Acacia } from '/lib/acacia/acacia.js';

const sideMenuContent = {
  ShowProfileSection: true,
  ProfileSection: {
    ProfilePic: '/files/user.jpg',
    ProfileName: 'John Doe',
    ProfileUsername: '@john',
    Action: () => { console.log('Profile'); }
  },
  Links: [
    { LinkIcon: '/files/home.svg', LinkTile: 'Home', LinkAction: () => console.log('Home') },
    { LinkIcon: '/files/config.svg', LinkTile: 'Settings', LinkAction: () => console.log('Settings') }
  ]
};

new Acacia('My App', false, sideMenuContent);
```

### Displaying a confirmation modal

```javascript
import { Modal } from '/lib/acacia/acacia.js';

async function deleteItem() {
  const confirmed = await Modal.Confirm('Attention', 'Do you really want to delete?');
  if (confirmed) {
    // perform action
  }
}
```

### Loading a custom view

```javascript
Renderer.Load('myView', APPVIEW).then(() => {
  console.log('View loaded');
});
```

### Using the audio player

```html
<acacia-audio src="/music/my-song.mp3"></acacia-audio>
```

---

## API Reference

For detailed reference, explore the source files. Below is a summary of the main classes and objects exposed globally:

- `window.Content` – data utilities.
- `window.Renderer` – rendering and layout.
- `window.Modal` – modal system.
- `window.Tooltip` – tooltips and context menus.
- `window.Datagrid` – grid class.
- `window.Gallery` – gallery class.
- `window.IdControl` – ID generator.

Registered custom elements:

- `acacia-app`, `acacia-accordion`, `card-big`, `button-rounded`, `button-squared`, `text-link`, `toast-mini`, `app-drawer-group`, `group-title`, `acacia-slider`, `acacia-progress`, `image-gallery`, `acacia-audio`, `acacia-desktop-app`, `acacia-top-menu`, `menu-item`, `menu-item-dropdown`, `dropdown-item`.

---

## Additional Information

1. The `files.zip` archive in this repository must be extracted to the project root.
2. Copyright © 2026 | Jorge Souza Oliveira dos Santos. All Rights Reserved.
