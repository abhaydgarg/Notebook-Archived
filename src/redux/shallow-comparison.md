# Shallow comparison in React & Redux context

Shallow comparison in **React and Redux context** means that, if say we have some object. And we have two states of the object. One is `prevSate` and another is `nextState`. React and redux will iterate over each key of `nextState` and compare the value of key with the `prevSate` key value. This is shallow comaprison in React and Redux.

If any value is changed then React/Redux update the component.

Whereas, **Strict Comparison**, in React/Redux world means comapring the references of `prevSate` and `nextState` and does not iterate over keys.

!!! note "Shallow Comparison in Javascript Context in General"
    When we talk about shallow comaprison in context of Javascript & immutability then it means comparing the references of two variables.
