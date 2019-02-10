---
id: accessibility
title: Accessibility
permalink: docs/accessibility.html
---

## 접근성을 지원해야 하는 이유? {#why-accessibility}

Web accessibility (also referred to as a11y) is the design and creation of websites that can be used by everyone. Accessibility support is necessary to allow assistive technology to interpret web pages.
웹 접근성 ([**a11y**](https://en.wiktionary.org/wiki/a11y)라고도 함)은 모든 사람이 사용할 수있는 웹 사이트의 디자인과 생성입니다. 보조 기술로 웹 페이지를 해석 할 수 있도록 접근성 지원이 필요합니다.

React는 표준 HTML 기술을 사용하여 접근성있는 웹 사이트를 완벽하게 지원합니다.

## 표준 및 가이드라인 {#standards-and-guidelines}

### WCAG {#wcag}

[웹 문서 접근성 지침(Web Content Accessibility Guidelines)](https://www.w3.org/WAI/intro/wcag)은 접근 가능한 웹 사이트를 만드는 가이드라인을 제공합니다.

아래 나오는 WCAG 체크 리스트는 개요를 제공합니다.

- [Wuhcag의 WCAG 체크 리스트](https://www.wuhcag.com/wcag-checklist/)
- [WebAIM의 WCAG 체크 리스트](http://webaim.org/standards/wcag/checklist)
- [A11Y 프로젝트의 체크 리스트](http://a11yproject.com/checklist.html)

### WAI-ARIA {#wai-aria}

[Web Accessibility Initiative - Accessible Rich Internet Applications](https://www.w3.org/WAI/intro/aria) 문서에는 완벽하게 접근 가능한 JavaScript 위젯을 만드는 기술이 포함되어 있습니다.

모든 `aria- *` HTML 어트리뷰트는 JSX에서 완벽하게 지원됩니다. React에서 대부분의 DOM 프로퍼티와 어트리뷰트는 카멜 케이스(camelCase)이지만, 이런 어트리뷰트는 일반 HTML과 마찬가지로 하이픈 케이스(kebab-case또는 lisp-case 등이라고 함)를 사용해야 합니다.

```javascript{3,4}
<input
  type="text"
  aria-label={labelText}
  aria-required="true"
  onChange={onchangeHandler}
  value={inputValue}
  name="name"
/>
```

## 시맨틱 HTML {#semantic-html}
Semantic HTML is the foundation of accessibility in a web application. Using the various HTML elements to reinforce the meaning of information
in our websites will often give us accessibility for free.
시맨틱 HTML은 웹 애플리케이션에서 접근성의 토대입니다. 다양한 HTML 엘리먼트를 사용하여 웹 사이트에 있는 정보의 의미를 강화하면 특별한 조치 없이 접근성이 충족되는 경우도 많습니다.

- [MDN HTML 엘리먼트 참조](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)

React 코드가 동작하도록 하기 위해 JSX에 `<div>` 엘리먼트를 추가 할 때가 있습니다. 특히 목록(`<ol>`, `<ul>` 또는 `<dl>`) 및 HTML `<table>` 엘리먼트를 작업하는 경우 종종 HTML 시맨틱을 어기게 됩니다. 이러한 경우에는 [React Fragment](/docs/fragments.html)를 사용하여 여러 엘리먼트를 그룹화하는 것이 좋습니다.

예를 들면,

```javascript{1,5,8}
import React, { Fragment } from 'react';

function ListItem({ item }) {
  return (
    <Fragment>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </Fragment>
  );
}

function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        <ListItem item={item} key={item.id} />
      ))}
    </dl>
  );
}
```

You can map a collection of items to an array of fragments as you would any other type of element as well:
다른 유형의 엘리먼트와 마찬가지로 항목 모음을 Fragment 배열에 매핑 할 수 있습니다.

```javascript{6,9}
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // 콜렉션을 매핑하는 경우 Fragment에 `key` prop이 있어야 함
        <Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </Fragment>
      ))}
    </dl>
  );
}
```

Fragment 태그에 props가 필요하지 않은 경우에는 다음과 같이 [짧은 문법](/docs/fragments.html#short-syntax) (사용하고 있는 도구에서 이 기능을 제공한다면)을 사용할 수도 있습니다.

```javascript{3,6}
function ListItem({ item }) {
  return (
    <>
      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </>
  );
}
```

자세한 내용은 [Fragment 문서](/docs/fragments.html)를 참조하십시오.

## 접근 가능한 폼 {#accessible-forms}

### 라벨링 {#labeling}

`<input>`과 `<textarea>` 같은 모든 HTML 엘리먼트는 접근 가능하도록 라벨을 지정해야 합니다. 스크린 리더에도 노출되는 설명이 포함된 라벨을 제공해야합니다.

아래의 목록은 이를 위한 방법을 설명합니다.

- [W3C의 엘리먼트에 라벨을 지정하는 방법에 대한 설명](https://www.w3.org/WAI/tutorials/forms/labels/)
- [WebAIM의 엘리먼트에 라벨을 지정하는 방법에 대한 설명](http://webaim.org/techniques/forms/controls)
- [Paciello Group 접근 가능한 이름에 대한 설명](https://www.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/)

이러한 표준 HTML 어트리뷰트는 React에서 바로 사용할 수 있지만, `for` 어트리뷰트는 JSX에서 `htmlFor`로 작성해야 합니다.

```javascript{1}
<label htmlFor="namedInput">Name:</label>
<input id="namedInput" type="text" name="name"/>
```

### Notifying the user of errors {#notifying-the-user-of-errors}

Error situations need to be understood by all users. The following link shows us how to expose error texts to screen readers as well:

- [The W3C demonstrates user notifications](https://www.w3.org/WAI/tutorials/forms/notifications/)
- [WebAIM looks at form validation](http://webaim.org/techniques/formvalidation/)

## Focus Control {#focus-control}

Ensure that your web application can be fully operated with the keyboard only:

- [WebAIM talks about keyboard accessibility](http://webaim.org/techniques/keyboard/)

### Keyboard focus and focus outline {#keyboard-focus-and-focus-outline}

Keyboard focus refers to the current element in the DOM that is selected to accept input from the keyboard. We see it everywhere as a focus outline similar to that shown in the following image:

<img src="../images/docs/keyboard-focus.png" alt="Blue keyboard focus outline around a selected link." />

Only ever use CSS that removes this outline, for example by setting `outline: 0`, if you are replacing it with another focus outline implementation.

### Mechanisms to skip to desired content {#mechanisms-to-skip-to-desired-content}

Provide a mechanism to allow users to skip past navigation sections in your application as this assists and speeds up keyboard navigation.

Skiplinks or Skip Navigation Links are hidden navigation links that only become visible when keyboard users interact with the page. They are very easy to implement with
internal page anchors and some styling:

- [WebAIM - Skip Navigation Links](http://webaim.org/techniques/skipnav/)

Also use landmark elements and roles, such as `<main>` and `<aside>`, to demarcate page regions as assistive technology allow the user to quickly navigate to these sections.

Read more about the use of these elements to enhance accessibility here:

- [Accessible Landmarks](http://www.scottohara.me/blog/2018/03/03/landmarks.html)

### Programmatically managing focus {#programmatically-managing-focus}

Our React applications continuously modify the HTML DOM during runtime, sometimes leading to keyboard focus being lost or set to an unexpected element. In order to repair this,
we need to programmatically nudge the keyboard focus in the right direction. For example, by resetting keyboard focus to a button that opened a modal window after that modal window is closed.

MDN Web Docs takes a look at this and describes how we can build [keyboard-navigable JavaScript widgets](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets).

To set focus in React, we can use [Refs to DOM elements](/docs/refs-and-the-dom.html).

Using this, we first create a ref to an element in the JSX of a component class:

```javascript{4-5,8-9,13}
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // Create a ref to store the textInput DOM element
    this.textInput = React.createRef();
  }
  render() {
  // Use the `ref` callback to store a reference to the text input DOM
  // element in an instance field (for example, this.textInput).
    return (
      <input
        type="text"
        ref={this.textInput}
      />
    );
  }
}
```

Then we can focus it elsewhere in our component when needed:

 ```javascript
 focus() {
   // Explicitly focus the text input using the raw DOM API
   // Note: we're accessing "current" to get the DOM node
   this.textInput.current.focus();
 }
 ```

Sometimes a parent component needs to set focus to an element in a child component. We can do this by [exposing DOM refs to parent components](/docs/refs-and-the-dom.html#exposing-dom-refs-to-parent-components)
through a special prop on the child component that forwards the parent's ref to the child's DOM node.

```javascript{4,12,16}
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.inputElement = React.createRef();
  }
  render() {
    return (
      <CustomTextInput inputRef={this.inputElement} />
    );
  }
}

// Now you can set focus when required.
this.inputElement.current.focus();
```

When using a HOC to extend components, it is recommended to [forward the ref](/docs/forwarding-refs.html) to the wrapped component using the `forwardRef` function of React. If a third party HOC
does not implement ref forwarding, the above pattern can still be used as a fallback.

A great focus management example is the [react-aria-modal](https://github.com/davidtheclark/react-aria-modal). This is a relatively rare example of a fully accessible modal window. Not only does it set initial focus on
the cancel button (preventing the keyboard user from accidentally activating the success action) and trap keyboard focus inside the modal, it also resets focus back to the element that
initially triggered the modal.

>Note:
>
>While this is a very important accessibility feature, it is also a technique that should be used judiciously. Use it to repair the keyboard focus flow when it is disturbed, not to try and anticipate how
>users want to use applications.

## Mouse and pointer events {#mouse-and-pointer-events}

Ensure that all functionality exposed through a mouse or pointer event can also be accessed using the keyboard alone. Depending only on the pointer device will lead to many cases where
keyboard users cannot use your application.

To illustrate this, let's look at a prolific example of broken accessibility caused by click events. This is the outside click pattern, where a user can disable an opened popover by clicking outside the element.

<img src="../images/docs/outerclick-with-mouse.gif" alt="A toggle button opening a popover list implemented with the click outside pattern and operated with a mouse showing that the close action works." />

This is typically implemented by attaching a `click` event to the `window` object that closes the popover:

```javascript{12-14,26-30}
class OuterClickExample extends React.Component {
constructor(props) {
    super(props);

    this.state = { isOpen: false };
    this.toggleContainer = React.createRef();

    this.onClickHandler = this.onClickHandler.bind(this);
    this.onClickOutsideHandler = this.onClickOutsideHandler.bind(this);
  }

  componentDidMount() {
    window.addEventListener('click', this.onClickOutsideHandler);
  }

  componentWillUnmount() {
    window.removeEventListener('click', this.onClickOutsideHandler);
  }

  onClickHandler() {
    this.setState(currentState => ({
      isOpen: !currentState.isOpen
    }));
  }

  onClickOutsideHandler(event) {
    if (this.state.isOpen && !this.toggleContainer.current.contains(event.target)) {
      this.setState({ isOpen: false });
    }
  }

  render() {
    return (
      <div ref={this.toggleContainer}>
        <button onClick={this.onClickHandler}>Select an option</button>
        {this.state.isOpen ? (
          <ul>
            <li>Option 1</li>
            <li>Option 2</li>
            <li>Option 3</li>
          </ul>
        ) : null}
      </div>
    );
  }
}
```

This may work fine for users with pointer devices, such as a mouse, but operating this with the keyboard alone leads to broken functionality when tabbing to the next element
as the `window` object never receives a `click` event. This can lead to obscured functionality which blocks users from using your application.

<img src="../images/docs/outerclick-with-keyboard.gif" alt="A toggle button opening a popover list implemented with the click outside pattern and operated with the keyboard showing the popover not being closed on blur and it obscuring other screen elements." />

The same functionality can be achieved by using an appropriate event handlers instead, such as `onBlur` and `onFocus`:

```javascript{19-29,31-34,37-38,40-41}
class BlurExample extends React.Component {
  constructor(props) {
    super(props);

    this.state = { isOpen: false };
    this.timeOutId = null;

    this.onClickHandler = this.onClickHandler.bind(this);
    this.onBlurHandler = this.onBlurHandler.bind(this);
    this.onFocusHandler = this.onFocusHandler.bind(this);
  }

  onClickHandler() {
    this.setState(currentState => ({
      isOpen: !currentState.isOpen
    }));
  }

  // We close the popover on the next tick by using setTimeout.
  // This is necessary because we need to first check if
  // another child of the element has received focus as
  // the blur event fires prior to the new focus event.
  onBlurHandler() {
    this.timeOutId = setTimeout(() => {
      this.setState({
        isOpen: false
      });
    });
  }

  // If a child receives focus, do not close the popover.
  onFocusHandler() {
    clearTimeout(this.timeOutId);
  }

  render() {
    // React assists us by bubbling the blur and
    // focus events to the parent.
    return (
      <div onBlur={this.onBlurHandler}
           onFocus={this.onFocusHandler}>
        <button onClick={this.onClickHandler}
                aria-haspopup="true"
                aria-expanded={this.state.isOpen}>
          Select an option
        </button>
        {this.state.isOpen ? (
          <ul>
            <li>Option 1</li>
            <li>Option 2</li>
            <li>Option 3</li>
          </ul>
        ) : null}
      </div>
    );
  }
}
```

This code exposes the functionality to both pointer device and keyboard users. Also note the added `aria-*` props to support screen-reader users. For simplicity's sake
the keyboard events to enable `arrow key` interaction of the popover options have not been implemented.

<img src="../images/docs/blur-popover-close.gif" alt="A popover list correctly closing for both mouse and keyboard users." />

This is one example of many cases where depending on only pointer and mouse events will break functionality for keyboard users. Always testing with the keyboard will immediately
highlight the problem areas which can then be fixed by using keyboard aware event handlers.

## More Complex Widgets {#more-complex-widgets}

A more complex user experience should not mean a less accessible one. Whereas accessibility is most easily achieved by coding as close to HTML as possible,
even the most complex widget can be coded accessibly.

Here we require knowledge of [ARIA Roles](https://www.w3.org/TR/wai-aria/#roles) as well as [ARIA States and Properties](https://www.w3.org/TR/wai-aria/#states_and_properties).
These are toolboxes filled with HTML attributes that are fully supported in JSX and enable us to construct fully accessible, highly functional React components.

Each type of widget has a specific design pattern and is expected to function in a certain way by users and user agents alike:

- [WAI-ARIA Authoring Practices - Design Patterns and Widgets](https://www.w3.org/TR/wai-aria-practices/#aria_ex)
- [Heydon Pickering - ARIA Examples](http://heydonworks.com/practical_aria_examples/)
- [Inclusive Components](https://inclusive-components.design/)

## Other Points for Consideration {#other-points-for-consideration}

### Setting the language {#setting-the-language}

Indicate the human language of page texts as screen reader software uses this to select the correct voice settings:

- [WebAIM - Document Language](http://webaim.org/techniques/screenreader/#language)

### Setting the document title {#setting-the-document-title}

Set the document `<title>` to correctly describe the current page content as this ensures that the user remains aware of the current page context:

- [WCAG - Understanding the Document Title Requirement](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-title.html)

We can set this in React using the [React Document Title Component](https://github.com/gaearon/react-document-title).

### Color contrast {#color-contrast}

Ensure that all readable text on your website has sufficient color contrast to remain maximally readable by users with low vision:

- [WCAG - Understanding the Color Contrast Requirement](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html)
- [Everything About Color Contrast And Why You Should Rethink It](https://www.smashingmagazine.com/2014/10/color-contrast-tips-and-tools-for-accessibility/)
- [A11yProject - What is Color Contrast](http://a11yproject.com/posts/what-is-color-contrast/)

It can be tedious to manually calculate the proper color combinations for all cases in your website so instead, you can [calculate an entire accessible color palette with Colorable](http://jxnblk.com/colorable/).

Both the aXe and WAVE tools mentioned below also include color contrast tests and will report on contrast errors.

If you want to extend your contrast testing abilities you can use these tools:

- [WebAIM - Color Contrast Checker](http://webaim.org/resources/contrastchecker/)
- [The Paciello Group - Color Contrast Analyzer](https://www.paciellogroup.com/resources/contrastanalyser/)

## Development and Testing Tools {#development-and-testing-tools}

There are a number of tools we can use to assist in the creation of accessible web applications.

### The keyboard {#the-keyboard}

By far the easiest and also one of the most important checks is to test if your entire website can be reached and used with the keyboard alone. Do this by:

1. Plugging out your mouse.
1. Using `Tab` and `Shift+Tab` to browse.
1. Using `Enter` to activate elements.
1. Where required, using your keyboard arrow keys to interact with some elements, such as menus and dropdowns.

### Development assistance {#development-assistance}

We can check some accessibility features directly in our JSX code. Often intellisense checks are already provided in JSX aware IDE's for the ARIA roles, states and properties. We also
have access to the following tool:

#### eslint-plugin-jsx-a11y {#eslint-plugin-jsx-a11y}

The [eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y) plugin for ESLint provides AST linting feedback regarding accessibility issues in your JSX. Many
IDE's allow you to integrate these findings directly into code analysis and source code windows.

[Create React App](https://github.com/facebookincubator/create-react-app) has this plugin with a subset of rules activated. If you want to enable even more accessibility rules,
you can create an `.eslintrc` file in the root of your project with this content:

  ```json
  {
    "extends": ["react-app", "plugin:jsx-a11y/recommended"],
    "plugins": ["jsx-a11y"]
  }
  ```

### Testing accessibility in the browser {#testing-accessibility-in-the-browser}

A number of tools exist that can run accessibility audits on web pages in your browser. Please use them in combination with other accessibility checks mentioned here as they can only
test the technical accessibility of your HTML.

#### aXe, aXe-core and react-axe {#axe-axe-core-and-react-axe}

Deque Systems offers [aXe-core](https://github.com/dequelabs/axe-core) for automated and end-to-end accessibility tests of your applications. This module includes integrations for Selenium.

[The Accessibility Engine](https://www.deque.com/products/axe/) or aXe, is an accessibility inspector browser extension built on `aXe-core`.

You can also use the [react-axe](https://github.com/dylanb/react-axe) module to report these accessibility findings directly to the console while developing and debugging.

#### WebAIM WAVE {#webaim-wave}

The [Web Accessibility Evaluation Tool](http://wave.webaim.org/extension/) is another accessibility browser extension.

#### Accessibility inspectors and the Accessibility Tree {#accessibility-inspectors-and-the-accessibility-tree}

[The Accessibility Tree](https://www.paciellogroup.com/blog/2015/01/the-browser-accessibility-tree/) is a subset of the DOM tree that contains accessible objects for every DOM element that should be exposed
to assistive technology, such as screen readers.

In some browsers we can easily view the accessibility information for each element in the accessibility tree:

- [Using the Accessibility Inspector in Firefox](https://developer.mozilla.org/en-US/docs/Tools/Accessibility_inspector)
- [Activate the Accessibility Inspector in Chrome](https://gist.github.com/marcysutton/0a42f815878c159517a55e6652e3b23a)
- [Using the Accessibility Inspector in OS X Safari](https://developer.apple.com/library/content/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)

### Screen readers {#screen-readers}

Testing with a screen reader should form part of your accessibility tests.

Please note that browser / screen reader combinations matter. It is recommended that you test your application in the browser best suited to your screen reader of choice.

### Commonly Used Screen Readers {#commonly-used-screen-readers}

#### NVDA in Firefox {#nvda-in-firefox}

[NonVisual Desktop Access](https://www.nvaccess.org/) or NVDA is an open source Windows screen reader that is widely used.

Refer to the following guides on how to best use NVDA:

- [WebAIM - Using NVDA to Evaluate Web Accessibility](http://webaim.org/articles/nvda/)
- [Deque - NVDA Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/nvda-keyboard-shortcuts)

#### VoiceOver in Safari {#voiceover-in-safari}

VoiceOver is an integrated screen reader on Apple devices.

Refer to the following guides on how activate and use VoiceOver:

- [WebAIM - Using VoiceOver to Evaluate Web Accessibility](http://webaim.org/articles/voiceover/)
- [Deque - VoiceOver for OS X Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/voiceover-keyboard-shortcuts)
- [Deque - VoiceOver for iOS Shortcuts](https://dequeuniversity.com/screenreaders/voiceover-ios-shortcuts)

#### JAWS in Internet Explorer {#jaws-in-internet-explorer}

[Job Access With Speech](http://www.freedomscientific.com/Products/Blindness/JAWS) or JAWS, is a prolifically used screen reader on Windows.

Refer to the following guides on how to best use JAWS:

- [WebAIM - Using JAWS to Evaluate Web Accessibility](http://webaim.org/articles/jaws/)
- [Deque - JAWS Keyboard Shortcuts](https://dequeuniversity.com/screenreaders/jaws-keyboard-shortcuts)

### Other Screen Readers {#other-screen-readers}

#### ChromeVox in Google Chrome {#chromevox-in-google-chrome}

[ChromeVox](http://www.chromevox.com/) is an integrated screen reader on Chromebooks and is available [as an extension](https://chrome.google.com/webstore/detail/chromevox/kgejglhpjiefppelpmljglcjbhoiplfn?hl=en) for Google Chrome.

Refer to the following guides on how best to use ChromeVox:

- [Google Chromebook Help - Use the Built-in Screen Reader](https://support.google.com/chromebook/answer/7031755?hl=en)
- [ChromeVox Classic Keyboard Shortcuts Reference](http://www.chromevox.com/keyboard_shortcuts.html)
