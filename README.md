# ChopSuey - Signal Component Library

ChopSuey is a small JavaScript library that enables common UI Components (HTML, CSS, and interactions) to be shared across projects built with disparate technologies. Components are designed with minimal styling so that they can be tailored to match the applications they are used in. Markup for ChopSuey components can be generated client or server side depending on the architectural requirements of your application.

## Available Components

- [Drop Down Menu](src/components/drop-down)
- [Accordian](src/components/accordion) (in development)
- Tabs (in development)


## Usage

1. Clone this repository and run `./install.sh`.
2. **Inline** [chop-suey.min.js](dist/js/chop-suey.min.js) in the `<head>` of your webpage.
3. Add [components.min.css](dist/css/components.min.css) to your webpage.
4. Add [components.min.js](dist/js/components.min.js) as an external script at the end of the `<body>` section of your webpage.
5. For each component or instance of a component, use the [mustache template](dist/templates) to generate and place markup where needed. The template can be rendered server-side or client-side and Chop-Suey will self-initialize JavaScript events when it detects the component markup on the page.  Each template takes the configuration arguments documented in the [component docs](#available-components) so that it can be customized to your particular application.


## Component Event API

Components will broadcast various state changes through events. We can listen to, and respond to, these events just as we would a click event. All components will broadcast:

- `componentNameWillBuild`: Fired before HTML is added
- `componentNameDidBuild`: Fired after HTML is added
- `componentNameWillEnhance`: Fired before behavior is added
- `componentNameDidEnhance`: Fired after behavior is added
- `componentNameWillDestroy`: Fired before HTML and behavior are removed
- `componentNameDidEnhance`: Fired after HTML and behavior are removed

Additionally, some components will broadcast additional events when state changes occur.  For example:

- `dropDownWillShow`: Fired before a drop-down menu shows
- `accordionDidHide`: Fired after a section of an accordion is hidden

In all cases, the context of the current component is passed in as `this` and as `event.target`. For some components, additional information will be attached to the event such as which accordion section was actually hidden. See the [component's docs](#available-components) for more information.

### Triggering Events

Chop-Suey components use standard DOM events to trigger state changes.  For instance, to make the drop-down component show it's menu:

```javascript
var component = document.getElementById('theComponent');
var show      = new window.CustomEvent('dropDownShow');

component.dispatchEvent(show);
```

The drop-down component is always listening for a dropDownShow event fired on itself, and when it hears one, it will modify all the CSS and JavaScript necessary to make the drop-down menu visible. It will also fire dropDownWillShow and dropDownDidShow events.

In this way, we only need to interact with components as black boxes that will tell us their state via events and respond to our requests to change their state without worrying about what the HTML markup, CSS classes, or bound JavaScript events are.


## Development

### Installation

1. Clone this repo
2. `cd` to your local repo and run `./install.sh`

### Updating

1. On master branch `git pull origin master`
2. run `./update.sh`

### Building

`cd` to the local repo and run `gulp`

### Demo

`cd` to the local repo and run `gulp start:demo` and follow the instructions


## Component Definition

Each component resides in a folder under [src/components](src/components). For example, the `drop-down` component may be found under [src/components/drop-down](src/components/drop-down). Each component directory contains a folder for `css`, `js`, and `templates`.

### CSS

The `css` directory should contain a single SASS file named `main.scss` containing all the styles necessary to render all versions of the component.

SASS files for all components will be converted to CSS and concatenated into `dist/components/css/components.css` and `dist/components/css/components.min.css`.

### JS

The `js` directory should contain a single JS file named `main.js` containing the ChopSuey.registerComponent() call for this component along with potential `build.js` and/or `enhance.js` with supporting browserify managed modules. 

JavaScript files for all components will be converted and concatenated into `dist/components/js/components.js` and `dist/components/js/components.min.js`.

### Templates

The `templates` directory should contain one or more templates  named `component-name.mustache` for rendering the entire component, or `component-name-part.mustache` containing sub templates for any HTML that needs to be added on enhancement, no matter how insignificant.

For example, the `drop-down` contains a main template called `drop-down.mustache` and three sub-templates called `drop-down-menu.mustache`, `drop-down-sizer.mustache`, and `drop-down-trigger.mustache`.

### Tests

Testing files live along side the source files. Testing is written using Mocha/Chai. We test everything from expected returns of small functions given good and bad arguments, to whether or not HTML is being added to the page, events are being thrown, and events are being listened for. 
