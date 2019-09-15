# Learning ReasonML

## Q & A

#### What is _t_ in `ReactEvent.Keyboard.t`?

> @lessp — "the main type of the module. it's t by convention"

- [ReactEvent source/docs](https://github.com/reasonml/reason-react/blob/master/src/ReactEvent.rei)

#### Why do we have to parse an event to use it? -> `ReactEvent.Keyboard.key(e)`

> @mulmus - "the most special thing is the [@bs.get] external part on [this line in the binding's source](https://github.com/reasonml/reason-react/blob/master/src/ReactEvent.re#L84). it's basically a special instruction to the compiler that tells it how the JS should end up looking in the binding"

- [DOM KeyBoardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent)
- [ReactEvent.Keyboard.key source](https://github.com/reasonml/reason-react/blob/627a8a3a4d4e800f225c5757f66d0619b5fa8569/src/ReactEvent.rei#L137)

#### How can I get the index when mapping over an Array?

> Use the indexed function, mapi.
> `mapi: ((int, 'a) => 'b, array('a)) => array('b)`

- [Array API](https://reasonml.github.io/api/Array.html)

## Resources

- [XState -> REState](https://sketch.sh/s/IaSD7pEAjjt0SqYiE9chXs/)
- [ReasonML Cheatsheet](https://github.com/arecvlohe/reasonml-cheat-sheet)
