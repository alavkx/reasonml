# Learning ReasonML

## Q & A

#### What is _t_ in `ReactEvent.Keyboard.t`?

> @lessp — "the main type of the module. it's t by convention"

- [ReactEvent source/docs](https://github.com/reasonml/reason-react/blob/master/src/ReactEvent.rei)

<hr />

#### Why do we have to parse an event to use it? -> `ReactEvent.Keyboard.key(e)`

> @mulmus - "the most special thing is the [@bs.get] external part on [this line in the binding's source](https://github.com/reasonml/reason-react/blob/master/src/ReactEvent.re#L84). it's basically a special instruction to the compiler that tells it how the JS should end up looking in the binding"

- [DOM KeyBoardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent)
- [ReactEvent.Keyboard.key source](https://github.com/reasonml/reason-react/blob/627a8a3a4d4e800f225c5757f66d0619b5fa8569/src/ReactEvent.rei#L137)

<hr />

#### How can I get the index when mapping over an Array?

> Use the indexed function, mapi.

```
mapi: ((int, 'a) => 'b, array('a)) => array('b)
```

- [Array API](https://reasonml.github.io/api/Array.html)

<hr />

#### Does ReactDOMRe support html5 audio controls?

> Yes. Unlike JS, you need to explicity pass boolean values.

```
<audio controls> // Wrong
<audio controls=true> // Right
```

- [ReactDOMRe docs](https://github.com/reasonml/reason-react/blob/master/src/ReactDOMRe.re)

<hr />

#### What is the config needed to require css in a similar fashion to the reason-react-hacker-news example project? ie: [`requireCSS("src/CommentList.css");`](https://github.com/reasonml-community/reason-react-hacker-news/blob/8ba4b7855e7c40cd5f2d575beb9a381c8f8ff7e5/src/CommentList.re#L5)

> that function is just [a binding to require()](https://github.com/reasonml-community/reason-react-hacker-news/blob/8ba4b7855e7c40cd5f2d575beb9a381c8f8ff7e5/src/Utils.re#L1-L2), which appears to be webpack in this case with a css loader

- [Bucklescript — Intro to external](https://bucklescript.github.io/docs/en/intro-to-external)

<hr />

#### Any idea what Error: Unbound type constructor App.track could mean?

> do the two modules depend on each other in a circular way? I’ve accidentally done that after splitting files up. Circular dependencies are illegal, and the compiler will just not “see” one of them
> E.g., if App.re uses anything from Player.re then that may be the problem.

`App.re`

```
...
type track = {
  name: string,
  artist: string,
};
...
<> <Library tracks /> <Player tracks /> </>;
```

`Library.re`

```
let make = (~tracks: option(array(App.track)), ~playTrack: int => unit) => {
```

<hr />

## Resources

- [XState -> REState](https://sketch.sh/s/IaSD7pEAjjt0SqYiE9chXs/)
- [ReasonML Cheatsheet](https://github.com/arecvlohe/reasonml-cheat-sheet)
