---
title: forwardRef
translators: [이승효]
---

<Intro>

`forwardRef` lets your component expose a DOM node to parent component with a [ref.](/learn/manipulating-the-dom-with-refs)
<Trans>`forwardRef`를 사용하면 컴포넌트가 [ref](/learn/manipulating-the-dom-with-refs)를 사용하여 부모 컴포넌트에 DOM 노드를 노출할 수 있습니다.</Trans>

```js
const SomeComponent = forwardRef(render)
```

</Intro>

<InlineToc />

---

## Reference<Trans>참조</Trans> {/*reference*/}

### `forwardRef(render)` {/*forwardref*/}

Call `forwardRef()` to let your component receive a ref and forward it to a child component:
<Trans>컴포넌트가 ref를 받아 하위 컴포넌트로 전달하도록 하려면 forwardRef()를 호출하세요:</Trans>

```js
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  // ...
});
```

[See more examples below.](#usage)
<Trans>[아래에 더 많은 예제가 있습니다.](#usage)</Trans>

#### Parameters<Trans>파라미터</Trans> {/*parameters*/}

* `render`: The render function for your component. React calls this function with the props and `ref` that your component received from its parent. The JSX you return will be the output of your component.
<Trans>
* `render`: 컴포넌트의 렌더링 함수입니다. React는 컴포넌트가 부모로부터 받은 props와 `ref`를 가지고 이 함수를 호출합니다. 반환하는 JSX는 컴포넌트의 출력이 됩니다.
</Trans>

#### Returns<Trans>반환</Trans> {/*returns*/}

`forwardRef` returns a React component that you can render in JSX. Unlike React components defined as plain functions, a component returned by `forwardRef` is also able to receive a `ref` prop.
<Trans>`forwardRef`는 JSX에서 렌더링할 수 있는 React 컴포넌트를 반환합니다. 일반 함수로 정의된 React 컴포넌트와 달리, `forwardRef`가 반환하는 컴포넌트는 `ref` prop를 받을 수도 있습니다.</Trans>

#### Caveats<Trans>경고</Trans> {/*caveats*/}

* In Strict Mode, React will **call your render function twice** in order to [help you find accidental impurities.](#my-initializer-or-updater-function-runs-twice) This is development-only behavior and does not affect production. If your render function is pure (as it should be), this should not affect the logic of your component. The result from one of the calls will be ignored.
<Trans>
* Strict Mode에서 React는 [실수로 발생한 불순물](#my-initializer-or-updater-function-runs-twice)을 찾기 위해 **렌더링 함수를 두 번 호출**합니다. 이는 개발전용 동작이며 프로덕션에는 영향을 미치지 않습니다. 렌더링 함수가 순수하다면(그래야만 하는 것처럼) 컴포넌트의 로직에 영향을 미치지 않을 것입니다. 호출 중 하나의 결과는 무시됩니다.
</Trans>

---

### `render` function<Trans>`render` 함수</Trans> {/*render-function*/}

`forwardRef` accepts a render function as an argument. React calls this function with `props` and `ref`:
<Trans>`forwardRef`는 render 함수를 인자로 받습니다. React는 이 함수를 `props`, `ref`와 함께 호출합니다:</Trans>

```js
const MyInput = forwardRef(function MyInput(props, ref) {
  return (
    <label>
      {props.label}
      <input ref={ref} />
    </label>
  );
});
```

#### Parameters<Trans>파라미터</Trans> {/*render-parameters*/}

* `props`: The props passed by the parent component.

* `ref`:  The `ref` attribute passed by the parent component. The `ref` can be an object or a function. If the parent component has not passed a ref, it will be `null`. You should either pass the `ref` you receive to another component, or pass it to [`useImperativeHandle`.](/reference/react/useImperativeHandle)
<TransBlock>
* `props`: 부모 컴포넌트에서 전달된 props 입니다.

* `ref`:  부모 컴포넌트에서 전달된 `ref` 속성입니다. `ref`는 객체나 함수일 수 있습니다. 부모 컴포넌트가 `ref`를 전달하지 않았다면 `null`이 됩니다. 받은 `ref`를 다른 컴포넌트에 전달하거나 [`useImperativeHandle`](/reference/react/useImperativeHandle)에 전달해야 합니다.
</TransBlock>

#### Returns<Trans>반환</Trans> {/*render-returns*/}

`forwardRef` returns a React component that you can render in JSX. Unlike React components defined as plain functions, the component returned by `forwardRef` is able to take a `ref` prop.
<Trans>`forwardRef`는 JSX에서 렌더링할 수 있는 React 컴포넌트를 반환합니다. 일반 함수로 정의된 React 컴포넌트와 달리, `forwardRef`가 반환하는 컴포넌트는 `ref` prop를 받을 수도 있습니다.</Trans>

---

## Usage {/*usage*/}

### Exposing a DOM node to the parent component {/*exposing-a-dom-node-to-the-parent-component*/}

By default, each component's DOM nodes are private. However, sometimes it's useful to expose a DOM node to the parent--for example, to allow focusing it. To opt in, wrap your component definition into `forwardRef()`:

```js {3,11}
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} />
    </label>
  );
});
```

You will receive a <CodeStep step={1}>ref</CodeStep> as the second argument after props. Pass it to the DOM node that you want to expose:

```js {8} [[1, 3, "ref"], [1, 8, "ref", 30]]
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});
```

This lets the parent `Form` component access the <CodeStep step={2}>`<input>` DOM node</CodeStep> exposed by `MyInput`:

```js [[1, 2, "ref"], [1, 10, "ref", 41], [2, 5, "ref.current"]]
function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

This `Form` component [passes a ref](/reference/useref#manipulating-the-dom-with-a-ref) to `MyInput`. The `MyInput` component *forwards* that ref to the `<input>` browser tag. As a result, the `Form` component can access that `<input>` DOM node and call [`focus()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus) on it.

Keep in mind that exposing a ref to the DOM node inside your component makes it harder to change your component's internals later. You will typically expose DOM nodes from reusable low-level components like buttons or text inputs, but you won't do it for application-level components like an avatar or a comment.

<Recipes title="Examples of forwarding a ref">

#### Focusing a text input {/*focusing-a-text-input*/}

Clicking the button will focus the input. The `Form` component defines a ref and passes it to the `MyInput` component. The `MyInput` component forwards that ref to the browser `<input>`. This lets the `Form` component focus the `<input>`.

<Sandpack>

```js
import { useRef } from 'react';
import MyInput from './MyInput.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

```js MyInput.js
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});

export default MyInput;
```

```css
input {
  margin: 5px;
}
```

</Sandpack>

<Solution />

#### Playing and pausing a video {/*playing-and-pausing-a-video*/}

Clicking the button will call [`play()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/play) and [`pause()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/pause) on a `<video>` DOM node. The `App` component defines a ref and passes it to the `MyVideoPlayer` component. The `MyVideoPlayer` component forwards that ref to the browser `<video>` node. This lets the `App` component play and pause the `<video>`.

<Sandpack>

```js
import { useRef } from 'react';
import MyVideoPlayer from './MyVideoPlayer.js';

export default function App() {
  const ref = useRef(null);
  return (
    <>
      <button onClick={() => ref.current.play()}>
        Play
      </button>
      <button onClick={() => ref.current.pause()}>
        Pause
      </button>
      <br />
      <MyVideoPlayer
        ref={ref}
        src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
        type="video/mp4"
        width="250"
      />
    </>
  );
}
```

```js MyVideoPlayer.js
import { forwardRef } from 'react';

const VideoPlayer = forwardRef(function VideoPlayer({ src, type, width }, ref) {
  return (
    <video width={width} ref={ref}>
      <source
        src={src}
        type={type}
      />
    </video>
  );
});

export default VideoPlayer;
```

```css
button { margin-bottom: 10px; margin-right: 10px; }
```

</Sandpack>

<Solution />

</Recipes>

---

### Forwarding a ref through multiple components {/*forwarding-a-ref-through-multiple-components*/}

Instead of forwarding a `ref` to a DOM node, you can forward it to your own component like `MyInput`:

```js {1,5}
const FormField = forwardRef(function FormField(props, ref) {
  // ...
  return (
    <>
      <MyInput ref={ref} />
      ...
    </>
  );
});
```

If that `MyInput` component forwards a ref to its `<input>`, a ref to `FormField` will give you that `<input>`:

```js {2,5,10}
function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <FormField label="Enter your name:" ref={ref} isRequired={true} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

The `Form` component defines a ref and passes it to `FormField`. The `FormField` component forwards that ref to `MyInput`, which forwards it to a browser `<input>` DOM node. This is how `Form` accesses that DOM node.


<Sandpack>

```js
import { useRef } from 'react';
import FormField from './FormField.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <FormField label="Enter your name:" ref={ref} isRequired={true} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

```js FormField.js
import { forwardRef, useState } from 'react';
import MyInput from './MyInput.js';

const FormField = forwardRef(function FormField({ label, isRequired }, ref) {
  const [value, setValue] = useState('');
  return (
    <>
      <MyInput
        ref={ref}
        label={label}
        value={value}
        onChange={e => setValue(e.target.value)} 
      />
      {(isRequired && value === '') &&
        <i>Required</i>
      }
    </>
  );
});

export default FormField;
```


```js MyInput.js
import { forwardRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});

export default MyInput;
```

```css
input, button {
  margin: 5px;
}
```

</Sandpack>

---

### Exposing an imperative handle instead of a DOM node {/*exposing-an-imperative-handle-instead-of-a-dom-node*/}

Instead of exposing an entire DOM node, you can expose a custom object, called an *imperative handle,* with a more constrained set of methods. To do this, you'd need to define a separate ref to hold the DOM node:

```js {2,6}
const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  // ...

  return <input {...props} ref={inputRef} />;
});
```

Pass the `ref` you received to [`useImperativeHandle`](/reference/react/useImperativeHandle) and specify the value you want to expose to the `ref`:

```js {6-15}
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});
```

If some component gets a ref to `MyInput`, it will only receive your `{ focus, scrollIntoView }` object instead of the DOM node. This lets you limit the information you expose about your DOM node to the minimum.

<Sandpack>

```js
import { useRef } from 'react';
import MyInput from './MyInput.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
    // This won't work because the DOM node isn't exposed:
    // ref.current.style.opacity = 0.5;
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

```js MyInput.js
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});

export default MyInput;
```

```css
input {
  margin: 5px;
}
```

</Sandpack>

[Read more about using imperative handles.](/reference/react/useImperativeHandle)

<Pitfall>

**Do not overuse refs.** You should only use refs for *imperative* behaviors that you can't express as props: for example, scrolling to a node, focusing a node, triggering an animation, selecting text, and so on.

**If you can express something as a prop, you should not use a ref.** For example, instead of exposing an imperative handle like `{ open, close }` from a `Modal` component, it is better to take `isOpen` as a prop like `<Modal isOpen={isOpen} />`. [Effects](/learn/synchronizing-with-effects) can help you expose imperative behaviors via props.

</Pitfall>

---

## Troubleshooting {/*troubleshooting*/}

### My component is wrapped in `forwardRef`, but the `ref` to it is always `null` {/*my-component-is-wrapped-in-forwardref-but-the-ref-to-it-is-always-null*/}

This usually means that you forgot to actually use the `ref` that you received.

For example, this component doesn't do anything with its `ref`:

```js {1}
const MyInput = forwardRef(function MyInput({ label }, ref) {
  return (
    <label>
      {label}
      <input />
    </label>
  );
});
```

To fix it, pass the `ref` down to a DOM node or another component that can accept a ref:

```js {1,5}
const MyInput = forwardRef(function MyInput({ label }, ref) {
  return (
    <label>
      {label}
      <input ref={ref} />
    </label>
  );
});
```

The `ref` to `MyInput` could also be `null` if some of the logic is conditional:

```js {1,5}
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      {showInput && <input ref={ref} />}
    </label>
  );
});
```

If `showInput` is `false`, then the ref won't be forwarded to any node, and a ref to `MyInput` will remain empty. This is particularly easy to miss if the condition is hidden inside another component, like `Panel` in this example:

```js {5,7}
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      <Panel isExpanded={showInput}>
        <input ref={ref} />
      </Panel>
    </label>
  );
});
```
