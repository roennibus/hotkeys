<p align="center">
 <img width="20%" height="20%" src="./logo.svg">
</p>

<br />

[![MIT](https://img.shields.io/packagist/l/doctrine/orm.svg?style=flat-square)]()
[![commitizen](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg?style=flat-square)]()
[![PRs](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)]()
[![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![All Contributors](https://img.shields.io/badge/all_contributors-0-orange.svg?style=flat-square)](#contributors-)
[![ngneat](https://img.shields.io/badge/@-ngneat-383636?style=flat-square&labelColor=8f68d4)](https://github.com/ngneat/)
[![spectator](https://img.shields.io/badge/tested%20with-spectator-2196F3.svg?style=flat-square)]()

> Power up your app with Hotkeys!

Web apps are getting closer and closer to be desktop-class applications. With this in mind, it makes sense to add hotkeys for those power users that are looking to navigate their favorite websites using hotkeys just as they do on their regular native apps. To help you have a better experience including hotkeys in your project, we develop Hotkeys.

## Features

- ✅ Definition on code-behind
- ✅ Definition on view
- ✅ Key combinations
- ✅ Element scoped
- ✅ Global listeners
- ✅ Platform agnostic
- ✅ Hotkeys cheatsheet

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [FAQ](#faq)

## Installation

### NPM

`npm install @ngneat/hotkeys --save-dev`

### Yarn

`yarn add @ngneat/hotkeys --dev`

## Usage

Few libraries are as easy to use as hotkeys. The first step is to add `HotkeysModule` in your `AppModule`. 

Hotkeys injects a service, a directive, and a component, application-wide, so there's no need to import Hotkeys in each of your submodules.

Once you've done this, there are two ways to start adding shortcuts to your application.

1. Using a directive directly on your target view.
2. Using a global service code-behind..

## Hotkeys Directive
Using the directive only requires adding `[hotkeys]` to a view element and pass in a shortcut. As you can see in the example down below, shortcuts are formatted as a string listing the keys separated by a period character.

```html
<div [hotkeys]="'meta.a'" (hotkey)="handleHotkey($event)" tabindex="1">
  NgNeat Hotkeys!
</div
```

> Note: Hotkeys takes care of transforming macOS specific keys to Linux and Windows and vice-versa.

The directive exposes an output event `(hotkey)` that relays the keyboard event when the hotkey is triggered.

Addionatelly, the directive accepts three more input parameters.

* `hotkeysGroup` - define a group name.
* `hotkeysDescription` - add a description.
* `hotkeysOptions` - set more specific options.

These options are the following properties.

```ts
interface Options {
  trigger: 'keydown' | 'keyup'; // listen to keydown or keyup events. It's keydown by default.
  showInHelpMenu: boolean; // include the hotkey to be display in the help menu. It's true by default.
  preventDefault: boolean; // whether to prevent default behavior in the target element. It's true by default.
}
```

## Hotkeys Service
This is a global service that can be injected into any component.
```ts
  constructor(private hotkeys: HotkeysService) {}
```

It not only provides a way to add hotkeys code behind but also other goodies.

* `addShortcut` - add a new shortcut
```ts
this.hotkeys
  .addShortcut({
    keys: 'backspace'
}).subscribe(e => console.log('Hotkey', e));
```
There are additional properties to customize the definition.

```ts
interface Options {
  group: string; // group name
  element: HTMLElement; // hotkey target element. By default is document.
  trigger: 'keydown' | 'keyup'; // type of event. It's keydown by default.
  description: string; // hotkey description
  showInHelpMenu: boolean; // included in the shortcut list to be display in the help dialog.
  preventDefault: boolean; // whether to prevent the default behavior of the event. It's true by default.
}
```
* `onShortcut` - listen to any register hotkey
This method takes a callback function with three inputs and returns void. Internally we are subscribing to a list of callbacks, so we need to unsubscribe once we're done.
```ts
const unsubscribe = this.hotkeys.onShortcut(
  (event, key, target) => console.log('callback', key)
  );
// when you're done listening, unsubscribe
// unsubscribe(); 
```

* `registerHelpModal` - display a help dialog listing all visible hotkeys

```ts
const helpFcn: () => void = () => {
  this.dialog.open(HotkeysHelpComponent, { width: '500px' });
};
this.hotkeys.registerHelpModal(helpFcn);
```
It accepts a second input that allows to define the "show help" hotkey.

* `getHotkeys` - retrieve all defined hotkeys application-wide.

It returns a list of `Hotkey` elements with all the defined hotkeys.
```ts
interface Hotkey {
  key: string; // shortcut key
  group: string; // group name
  element: HTMLElement; // target element
  trigger: 'keydown' | 'keyup'; // type of event
  description: string; // shortcut description
  showInHelpMenu: boolean; // hotkey shows in help menu
  preventDefault: boolean; // prevent default event behavior or not
}

```
* `getShortcuts` - retrieve the shortcuts to be displayed in a help screen.

It returns a conveniently formatted list to show in the help dialog.

```ts
{ 
  group: string; // group name
  hotkeys: { keys: string; description: string}[] // hotkey list for this group
}
```

## Hotkeys Shortcut Pipe
There is also available a @Pipe, `hotkeysShortcut`, to help you format your shortcuts when presenting them in a custom help screen.

```html
<div class="help-dialog-shortcut-key">
  <kbd [innerHTML]="hotkey.keys | hotkeysShortcut"></kbd>
</div>
```

## Hotkeys Help Component
There is also available a @Component, `HotkeysHelpComponent`, which offers a help view of the collection of visible shortcuts. It can be easily used with Angular Material Dialog.

```ts
this.dialog.open(HotkeysHelpComponent, { width: '500px' });
```

That's all for now! Make sure to check out the playground inside the src folder!

## FAQ

**Can I define duplicated hotkeys?**

No. It's not possible to define a hotkey multiple times. Each hotkey has a description and a group, so it doesn't make sense assigning a hotkey to different actions.

**Why am I not receiving any event?**

If you've added a hotkey to a particular element of your DOM, make sure it's focusable. Otherwise, hotkeys cannot capture any keyboard event.


## How to ...

**Listening to the same shortcut in different places.**

You can always use `onShortcut`. This method allows listening to all registered hotkeys without affecting the original definition.

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<!-- markdownlint-enable -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
