# Learning ReasonML

## Talks and notes 🤔💭

### [Sean Grove - Finding joy in programming - Øredev 2017](https://vimeo.com/242081961)

<details>
  <summary style="font-weight: 600;">
    Sean enumerates common headaches and how we can avoid them
  </summary>
  <p>When is programming intrinsically fun, and when is it laborious? What drives some developers towards the rapidly developing JavaScript ecosystem, while others flock to less-developed and more esoteric languages and ecosystems? What keeps us using metaphors and abstractions developed so many decades before, when we've spent so much effort designing clean-slate systems?

We'll look at this discussion through the prism of ReasonML, a new syntax and set of tooling around OCaml.

OCaml itself is more than 20 years old and is wildly popular amongst academics, but is largely unknown in the industry. We'll see how Reason straddles the spectrum of the questions above to bring all of that horsepower to bear on industry problems - through a syntax and developer experience appealing to pragmatists, an emphasis on FP and a type system meant to draw in purists, and a focus on incremental migration that means we can use it in the systems of yesteryear while moving towards the abstractions of tomorrow.

#### Key takeaways

  <ul>
    <li></li>
  </ul>
</details>

<hr />

<a id="toc"></a>

## Questions and answers 📈

- [What is _t_ in `ReactEvent.Keyboard.t`?](#qa1)
- [Why do we have to parse an event to use it?](#qa2)
- [Why do we have to parse an event to use it?](#qa1)
- [How can I get the index when mapping over an Array?](#qa2)
- [Does ReactDOMRe support html5 audio controls?](#qa3)
- [What is the config needed to require css in a similar fashion to the reason-react-hacker-news example project?](#qa4)
- [Any idea what Error: Unbound type constructor App.track could mean?](#qa5)
- [Has anyone messed around with xstate concepts in reason?](#qa6)

- [When defining a binding to a JS component, are we supposed to include children?](#qa7)
- [Why does Js.Promise expect a generic here?](#qa8)
- [I’ve been trying to write a binding for a hook in react-use, and failing pretty bad.](#qa9)

<a id="qa1"></a>

### What is _t_ in `ReactEvent.Keyboard.t`? [⬆](#toc)

> @lessp — "the main type of the module. it's t by convention"

- [ReactEvent source/docs](https://github.com/reasonml/reason-react/blob/master/src/ReactEvent.rei)

<a name="qa2"></a>

### Why do we have to parse an event to use it? [⬆](#toc)

```ocaml
ReactEvent.Keyboard.key(e)
```

> @mulmus - "the most special thing is the [@bs.get] external part on [this line in the binding's source](https://github.com/reasonml/reason-react/blob/master/src/ReactEvent.re#L84). it's basically a special instruction to the compiler that tells it how the JS should end up looking in the binding"

- [DOM KeyBoardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent)
- [ReactEvent.Keyboard.key source](https://github.com/reasonml/reason-react/blob/627a8a3a4d4e800f225c5757f66d0619b5fa8569/src/ReactEvent.rei#L137)

<a name="qa3"></a>

### How can I get the index when mapping over an Array? [⬆](#toc)

> Use the indexed function, mapi.

```
mapi: ((int, 'a) => 'b, array('a)) => array('b)
```

- [Array API](https://reasonml.github.io/api/Array.html)

<a name="qa4"></a>

### Does ReactDOMRe support html5 audio controls? [⬆](#toc)

> Yes. Unlike JS, you need to explicity pass boolean values.

```html
<audio controls></audio> // Wrong

<audio controls="true"></audio> // Right
```

- [ReactDOMRe docs](https://github.com/reasonml/reason-react/blob/master/src/ReactDOMRe.re)

<a name="qa5"></a>

### What is the config needed to require css in a similar fashion to the [⬆](#toc)reason-react-hacker-news example project?

ie: [`requireCSS("src/CommentList.css");`](https://github.com/reasonml-community/reason-react-hacker-news/blob/8ba4b7855e7c40cd5f2d575beb9a381c8f8ff7e5/src/CommentList.re#L5)

> @mulmus - "that function is just [a binding to require()](https://github.com/reasonml-community/reason-react-hacker-news/blob/8ba4b7855e7c40cd5f2d575beb9a381c8f8ff7e5/src/Utils.re#L1-L2), which appears to be webpack in this case with a css loader"

- [Bucklescript — Intro to external](https://bucklescript.github.io/docs/en/intro-to-external)

<a name="qa6"></a>

### Any idea what Error: Unbound type constructor App.track could mean? [⬆](#toc)

> @johnridesabike — "do the two modules depend on each other in a circular way? I’ve accidentally done that after splitting files up. Circular dependencies are illegal, and the compiler will just not “see” one of them. E.g., if `App.re` uses anything from `Player.re` then that may be the problem."

```ocaml
/* App.re */
...
type track = {
  name: string,
  artist: string,
};
...
<> <Library tracks /> <Player tracks /> </>;
```

```ocaml
/* Library.re */
let make = (~tracks: option(array(App.track)), ~playTrack: int => unit) => {
  ...
}
```

<a name="qa7"></a>

### Has anyone messed around with xstate concepts in reason? [⬆](#toc)

> @gabrielrabreu — "I have thrown [some ideas in this sketch](https://sketch.sh/s/IaSD7pEAjjt0SqYiE9chXs/) but did not finished it yet"

- [XState -> REState](https://sketch.sh/s/IaSD7pEAjjt0SqYiE9chXs/)

<a name="qa8"></a>

### When defining a binding to a JS component, are we supposed to include children? [⬆](#toc)

```ocaml
module Button = {
  [@bs.module "components/base/Button.js"] [@react.component]
  external make:
    (~onClick: unit => unit, ~className: string, ~children: React.element) =>
...
```

If I take it out I get an error —

```ocaml
  163 ┆ }>
  164 ┆ <div>
  165 ┆   <Button
  166 ┆     className=Styles.templateOption
  167 ┆     onClick={_ =>

  The function applied to this argument has type
    (~key: string=?) => {. "className": string, "onClick": unit => unit}
This argument cannot be applied with label ~children
```

If I include it, I get an error in a different usage of `Button` —

```ocaml
  284 ┆   ])}>
  285 ┆   <div>
  286 ┆     <Button onClick={_ => ()} className=Styles.templateOption />
  287 ┆   </div>
  288 ┆ </div>}

  This call is missing an argument of type (~children: React.element)
```

> @yawaramin - "Try ~children: React.element=?"

```ocaml
(~onClick: unit => unit, ~className: string, ~children: React.element=?) =>
```

<a name="qa8"></a>

### Why does Js.Promise expect a generic here? [⬆](#toc)

```ocaml
let getTemplates = (~accountId: string, ~domain: string) => {
  FlowBuilderApi.getFlowBuilderApiClient()##get(
    "/accounts/" ++ accountId ++ "/step-templates?domain=" ++ domain,
  )
  ->Js.Promise.then_(res => res##data##data##templates);
};
```

```ocaml
This has type:
  Js.Promise.t(FlowBuilderApi.response) (defined as
    Js.Promise.t(FlowBuilderApi.response))

But somewhere wanted:
    'a => Js.Promise.t('b)
```

> @yawaramin - "use |> instead of -> here. Js.Promise.then\_ is written in t-last argument order"

<a name="qa9"></a>

### I’ve been trying to write a binding for a hook in react-use, and failing pretty bad. [⬆](#toc)

I did read all the bucklescript docs. It seems like there’s a bunch of ways to do things..bs val, external, send, pipe.

I have a bunch of questions..

- Can I ignore any of the these concepts for my application?
- Do i get to chose the API that i’ll be dealing with?
- Is there any best practices with choosing the structures for your api?
- What role does type t play in the binding?
- What role does make play?
- What’s the difference between module Make, let make, and let \_make? I’ve seen all three in bindings.

Forgive the wall of text 😰
I’m pretty confused

> @lmao bindings can be a fairly overwhelming thing to jump into, but the good news is, you can ignore quite a bit of it, and it's fairly easy to try things out, look at the compiled JS, and then try something different if it didn't work

> i can't say i've written bindings to any react hooks (hopefully someone else around here can help you with the specifics), but a lot of bindings follow the pattern:

```ocaml
[@bs.module "module-name"]
external nameYouChoose: string => int = "nameOfExternalFunctionToCall";
```

`@bs.module`

tells it which JS module to require/import (could be local or in node_modules)

`external`

works exactly like let, except it's a hint that this thing isn't going to be implemented in Reason

`nameYouChoose`

is the name you want to use to call the function. make is a convention for constructor functions, but it's not required

`: string => int`

is any valid function signature

`= "nameOfExternalFunctionToCall"`

is the "implementation" of the function, telling the compiler what JS function should be called

it's often common to want to construct JS objects with the new keyword, in which case you can prefix your external with `[@bs.new]`. in those cases, it might be helpful to define an "abstract type" (one with no implementation details visible) often named t by convention:

```ocaml
type t; // no implementation, no constructors
[@bs.new] external make: int => t = "Foo"; // the only way to make a value of type `t`
```

make(3) will turn into new Foo(3) in that example

## Resources

- [ReasonML Cheatsheet](https://github.com/arecvlohe/reasonml-cheat-sheet)
