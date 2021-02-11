# Horizontal Draggable Tabs

![Main Image](https://i.imgur.com/fqIQqdK.png)
Check out [Live Demo](https://amansharma007.github.io/horizontal-tabs/)

## Usage

Tabs are built in a way to be used anywhere, with ease. Use it the following way,

### Basic Usage
This is how you can use the basic tabs.
```HTML
<tabs>
  <tab title="Tab 1" active="true"> Tab 1 Content </tab>
  <tab title="Tab 2" active="false"> Tab 2 Content </tab>
  <tab title="Tab 3" active="false"> Tab 3 Content </tab>
</tabs>
```

### Closable Tabs
Tabs have a closable property to allow the tabs to be closed. Minimum one tab will remain open.
```HTML
<tabs :closable="true" @close-tab="closeTabHandler()">
  <tab title="Tab 1" active="true"> Tab 1 Content </tab>
  <tab title="Tab 2" active="false"> Tab 2 Content </tab>
  <tab title="Tab 3" active="false"> Tab 3 Content </tab>
</tabs>
```

### Show Controls in Tabs
Tabs have a property `showControls` to allow the chevrons for scrolling to be visible or not. It is `true` by default.
```HTML
<tabs :show-controls="false">
  <tab title="Tab 1" active="true"> Tab 1 Content </tab>
  <tab title="Tab 2" active="false"> Tab 2 Content </tab>
  <tab title="Tab 3" active="false"> Tab 3 Content </tab>
</tabs>
```

### Tab component
The tab component has two properties, `title` and `active`, to provide the title display text and to provide the default state of the tab (active/inactive).
```HTML
<tab title="Tab Title" active="false"> Tab Content </tab>
```

## Contribution Guide

### Project setup

```
yarn install
```

#### Compiles and hot-reloads for development

```
yarn serve
```

#### Compiles and minifies for production

```
yarn build
```

#### Deploy built files to production

```
yarn deploy
```
