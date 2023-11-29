# Core Tabs

> `@nrk/core-tabs` converts `<button>` and `<a>` elements to keyboard accessible tabs, controlling following tabpanels.
> Tabs can be nested and easily extended with custom animations or behaviour through the `tabs.toggle` event.

<!-- <script src="https://unpkg.com/preact"></script>
<script src="https://unpkg.com/preact-compat"></script>
<script>
  window.React = preactCompat
  window.ReactDOM = preactCompat
</script> -->
<!--demo
<script src="https://unpkg.com/@webcomponents/custom-elements"></script>
<script src="core-tabs/core-tabs.min.js"></script>
<script src="core-tabs/core-tabs.jsx.js"></script>
<style>
  [role="tabpanel"] { padding: 10px }
  [aria-selected="true"] { border: 2px solid }

  .my-vertical-tabs [role="tablist"] { float: left; width: 150px }
  .my-vertical-tabs [role="tabpanel"] { overflow: hidden }
  .my-vertical-tabs [role="tab"] { display: inline-block }
</style>
demo-->

## Examples (plain JS)
#### Nested tabs
```html
<!--demo-->
<core-tabs>
  <button>Tab 1</button>
  <button>Tab 2</button>
  <a href="#link">Tab 3</a>
</core-tabs>
<div>Tabpanel 1</div>
<div hidden>
  <core-tabs>
    <button>Subtab 1</button>
    <button>Subtab 2</button>
    <button>Subtab 3</button>
  </core-tabs>
  <div hidden>Subtabpanel 1</div>
  <div>Subtabpanel 2</div>
  <div hidden>Subtabpanel 3</div>
</div>
<div hidden>Tabpanel 3</div>
```
#### Few panels
```html
<!--demo-->
<core-tabs id="few-panels-plain-js" tab="fppj-tab-3">
  <button type="button" data-for="panel-1" id="fppj-tab-1">First tab</button>
  <button type="button" data-for="panel-2" id="fppj-tab-2">Second tab</button>
  <button type="button" data-for="panel-2" id="fppj-tab-3">Third tab</button>
</core-tabs>
<div id="panel-1" hidden>
  Text of the first panel
</div>
<div id="panel-2">Text of the second panel, shared by second and third tab</div>
```
#### Single panel
```html
<!--demo-->
<core-tabs id="single-panel-plain-js" tab='sppj-tab-2'>
  <button type="button" data-for="only-panel" id="sppj-tab-1">First tab</button>
  <button type="button" data-for="only-panel" id="sppj-tab-2">Second tab</button>
  <button type="button" data-for="only-panel" id="sppj-tab-3">Third tab</button>
</core-tabs>
<div id="only-panel">Text of the only panel. Note that <code>aria-labelledBy</code> reflects active tab</div>
```
## Examples (React)
#### Nested tabs
```html
<!--demo-->
<div id="react-nested-tabs" class="my-vertical-tabs"></div>
<script type="text/jsx">
  ReactDOM.render(<div>
    <CoreTabs>
      <button>Vertical tab 1 JSX</button>
      <button>Vertical tab 2 JSX</button>
    </CoreTabs>
    <div>Tabpanel 1 JSX</div>
    <div hidden>
      <CoreTabs>
        <button>Subtab 1 JSX</button>
        <button hidden>Subtab 2 JSX</button>
      </CoreTabs>
      <div>Subtabpanel 1</div>
      <div hidden>Subtabpanel 2</div>
    </div>
  </div>, document.getElementById('react-nested-tabs'))
</script>
```
#### Dynamic tabs
```html
<!--demo-->
<div id="react-dynamic-tabs" class="my-vertical-tabs"></div>
<script type="text/jsx">
  const Dynamic = () => {
      const [elements, setElements] = React.useState([])
      const menu = elements.map(item => <button type="button">Dynamic Tab {item}</button>);
      const pages = elements.map(item => <div hidden>Tabpanel {item}</div>);

      return (
        <>
          <button type="button" onClick={() => setElements([...elements, elements.length + 1])}>
            Add extra tab
          </button>
          <button type="button" onClick={() => setElements([1,2])}>
            Set to two tabs
          </button>
          <button type="button" onClick={() => setElements([])}>
            Remove all
          </button>
          <CoreTabs>
            {menu}
          </CoreTabs>
          {pages}
        </>
      )
    }
  ReactDOM.render(<Dynamic />, document.getElementById('react-dynamic-tabs'))
</script>
```
#### Active by index
```html
<!--demo-->
<div id="react-index-tabs"></div>
<script type="text/jsx">
  const IndexTabs = () => {
      const [tabIndex, setTabIndex] = React.useState(0)
      const handleTabsToggle = (event) => { setTabIndex(parseInt(event.target.getAttribute('tab'))) }
      const handleInputChange = (event) => { setTabIndex(parseInt(event.target.value)) }

      return (
        <>
          <div>
            <label>
              Set active tab
              <input
                type="range"
                value={tabIndex}
                min="0"
                max="3"
                step="1"
                onChange={handleInputChange}
              />
            </label>
          </div>
          <CoreTabs tab={tabIndex} onTabsToggle={handleTabsToggle}>
            <button type="button">Tab 1</button>
            <button type="button">Tab 2</button>
            <button type="button">Tab 3</button>
            <button type="button">Tab 4</button>
          </CoreTabs>
          <div>Tabpanel 1</div>
          <div hidden>Tabpanel 2</div>
          <div hidden>Tabpanel 3</div>
          <div hidden>Tabpanel 4</div>
        </>
      )
    }
  ReactDOM.render(<IndexTabs />, document.getElementById('react-index-tabs'))
</script>
```
#### Single panel
```html
<!--demo-->
<div id="react-single-panel"></div>
<script type="text/jsx">
  const EpisodeList = ({seasonNumber}) => {
    const episodes = [
      ["S1: Episode 1", "S1: Episode 2", "S1: Episode 3"],
      ["S2: Episode 1", "S2: Episode 2"]
    ];

    const [episodesInSeason, setEpisodesInSeason] = React.useState(episodes[seasonNumber - 1]);

    React.useEffect(() => {
      setEpisodesInSeason(episodes[seasonNumber - 1])
    }, [seasonNumber])

    return (
      <div id="episode-list">
        Table panel for season {seasonNumber}: Content here should change: 
        <br/><br/>
        {episodesInSeason.map(episode => <button>{episode}</button>)}
      </div>
    )
  }
  const SeasonList = () => {
    const [selectedSeason, setSelectedSeason] = React.useState(2);

    return (
      <>
        <CoreTabs
          tab={selectedSeason - 1}
          id="season-list"
          onTabsToggle={event => setSelectedSeason(Number(event.target.getAttribute('tab')) + 1)}
        >
          <button
            type="button"
            id="season-tab-0"
            data-for="episode-list"
          >
            Season 1
          </button>
          <button
            type="button"
            id="season-tab-1"
            data-for="episode-list"
          >
            Season 2
          </button>
        </CoreTabs>
        <EpisodeList seasonNumber={selectedSeason} />
      </>
    )
  }
  ReactDOM.render(<SeasonList />, document.getElementById('react-single-panel'))
</script>
```


## Installation

Using NPM provides own element namespace and extensibility.
Recommended:

```bash
npm install @nrk/core-tabs  # Using NPM
```

Using static registers the custom element with default name automatically:

```html
<script src="https://static.nrk.no/core-components/major/11/core-tabs/core-tabs.min.js"></script>  <!-- Using static -->
```

Remember to [polyfill](https://github.com/webcomponents/polyfills/tree/master/packages/custom-elements) custom elements if needed.



## Usage

### Attributes

Name | Optional | Accepts values | Description
:-- | :-- | :-- | :--
`tab` | Yes | `number \| string` | Used to set active tab. Number is index (starting at `0`), string is an id-reference to a tab-element.

### HTML / JavaScript

```html
<core-tabs tab="{string | number}">       <!-- Optional. Used to set active tab String associates id-reference to tab element -->
  <button>Tab 1</button>                  <!-- Tab elements must be <a> or <button>. Do not use <li> -->
  <a href="#">Tab 2</a>
  <button>Tab 3</button>
  <button data-for="panel-2">Tab 4</button>    <!-- Point to a specific tabpanel -->
</core-tabs>
<div>Tabpanel 1 content</div>             <!-- First tabpanel is the next element sibling of core-tabs -->
<div hidden>Tabpanel 2 content</div>      <!-- Second tabpanel. Use hidden attribute to prevent FOUC -->
<div hidden id="panel-2">Tabpanel 3 content</div>      <!-- Third tabpanel. ID used to connect to tab 4 -->
```

```js
import CoreTabs from '@nrk/core-tabs'                 // Using NPM
window.customElements.define('core-tabs', CoreTabs)   // Using NPM. Replace 'core-tabs' with 'my-tabs' to namespace

const myTabs = document.querySelector('core-tabs')

// Getters
myTabs.tab        // Get active tab
myTabs.tabs       // Get all tabs
myTabs.panel      // Get active tabpanel
myTabs.panels     // Get all tabpanels

// Setters
myTabs.tab = 0        // Set active tab from index
myTabs.tab = 'my-tab' // Set active tab from id
myTabs.tab = myTab    // Set active tab from element
```

### React / Preact

```js
import CoreTabs from '@nrk/core-tabs/jsx'

<CoreTabs
  tab={String}                  // Optional. Sets current active tab by index or id
  ref={(comp) => {}}            // Optional. Get reference to React component
  forwardRef={(el) => {}}       // Optional. Get reference to underlying DOM custom element
  onTabsToggle={Function}       // Optional. Listen to toggle event
>
  <button                  // Tab element must be <a> or <button>. Do not use <li>
    data-for={String}      // Id to element that contains the tab-related content
  >
    Tab 1
  </button>
  <a href="#">Tab 2</a>    
</CoreTabs>
<div>Tabpanel 1 content</div>           // First tabpanel is the next element sibling of CoreTabs
<div hidden>Tabpanel 1 content</div>    // Second tabpanel. Use hidden attribute to prevent FOUC
<div hidden>Tabpanel 1 content</div>    // Third tabpanel.  Use hidden attribute to prevent FOUC
```



## Events

### tabs.toggle

Fired when toggling a tab:

```js
document.addEventListener('tabs.toggle', (event) => {
  event.target     // The tabs element
})
```

## Styling
All styling in documentation is example only. Both the tabs and tabpanels receive attributes reflecting the current toggle state:

```css
.my-tab {}                          /* Target tab in any state */
.my-tab[aria-selected="true"] {}    /* Target only open tab */
.my-tab[aria-selected="false"] {}   /* Target only closed tab */

.my-tabpanel {}                     /* Target panel element in any state */
.my-tabpanel:not([hidden]) {}       /* Target only open panel */
.my-tabpanel[hidden] {}             /* Target only closed panel */
```


## FAQ
### Why aren't tabs wrapped in `<ul><li>...</li></ul>`?
A `<ul>`/`<li>` structure would seem logical for tabs, but this causes some screen readers to incorrectly announce tabs as single (tab 1 of 1).

### Must panels always be next element siblings to `<core-tabs>`?
The aria specification does not allow any elements that are focusable by a screen reader to be placed between tabs and panels. Therefore, `core-tabs` defaults to use the next element siblings as panels.
This behaviour can be overridden, by setting up `id` on panel elements and the `data-for` attribute on tab element (`for` is deprecated). Use with caution and *only* do this if your project *must* use another DOM structure. Example:

```js
const myTabs = document.querySelector('core-tabs')
myTabs.tabs.forEach((tabs, index) => tab.setAttribute('data-for', myTabs.panels[index].id = 'my-panel-' + index))
```

### How do I avoid panels flickering on initialization?
When you know what panel will be visible on load, all others should have the `hidden`-attribute to avoid [Flash of unstyled content](https://en.wikipedia.org/wiki/Flash_of_unstyled_content) (FOUC). If the active panel is unknown to your template, set `hidden`-attribute on all panels initially.
