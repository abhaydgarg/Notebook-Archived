# Rules when writing component

## Rules

1. There must only be a single “source of truth” for any data (props and state). [Read this](/react/getDerivedStateFromProps/)
2. Data must always flow from top to bottom.
3. props must be immutable.
4. One Component -> One Responsibility
5. One Component -> One File

## What is single source of truth

A common pattern is to create several **stateless** components that just render data, and have a single stateful component above them in the hierarchy that passes its state to its children via props. The stateful component encapsulates all of the interaction logic, while the stateless components take care of rendering data in a declarative way.

**In simple words:** Only one component, for example, parent component should only **fetch** data from external source, **validate** it and **update** it. Children components are only there for receiving data from parent as props. Child component may trigger logic, like updating state, but only by means of functions passed down from the parent component.

### Lifting state up

There should be a single “source of truth” for any data that changes in a React application. Usually, the state is first added to the component that needs it for rendering. Then, if other components also need it, you can lift it up to their closest common ancestor. Instead of trying to sync the state between different components, you should rely on the top-down data flow.

**Lifting state involves writing more “boilerplate” code than two-way binding approaches, but as a benefit, it takes less work to find and isolate bugs. Since any state “lives” in some component and that component alone can change it, the surface area for bugs is greatly reduced.**

## Summary

If lifting state up, to support single source of truth rule, is considered then extra expicit type check and value check should be avoided in children component. You can do it if required in certain circumstances.

Child components should always get data from top parent in the form of props and can be used its own state for its internal logic for example adding and removing placehoder in Image component based on image loading events, because if the state you are going to use for child comp is entirely for child's internal use and has no connection with parent comp then it is sometimes not a good idea to let the parent handle the state of child comp by means of functions passed down from the parent component.

Initialize state with props. If child component is using prop to set its own internal state value then you must be doing something wrong, therefore you should read [this](/react/getDerivedStateFromProps/)
