---
layout: post
title: "Considerations on React"
date: 2017-09-25 00:02:00
categories: Programming
---
For the past few years there has been a sharp increase in front-end
tools. From Bower to Webpack to Grunt to Gulp, making it harder for
interested people to jump in and start learning front-end web
development.  But there's no space more crowded than that of
JavaScript frameworks.

For newcomers in this space, it's very easy to get overwhelmed by all
the different frameworks and libraries used today. Do you use Angular?
Or maybe you only need jQuery for your particular problem. Should you
learn React? What the hell is Flux? These are all questions someone
today may ask themselves. In a month or so expect some new framework
to have made its way to that list.

Chances are, you don't need any of them. Or maybe you only need
jQuery. There's nothing wrong with using plain old JavaScript in a
simple `.js` file. In fact, most of the time it will be the best
course of action. It's simple, you don't have to learn a new library
and there's no fear of the framework you relied on will be abandoned.

But a lot of the frameworks out there today exist for a good
reason. Each one is trying to solve a problem or a series of problems
and some of these solution overlap. A good front-end developer should
be able to recognize what each framework is trying to solve and use
them accordingly.

## React

The [official React website](https://facebook.github.io/react/) states
that React is 
> A JavaScript library for building user interfaces.

Admittedly, I am stretching the definition of framework here a
bit. React is not as powerful or with as many features as Angular or
Ember. It doesn't handle the logic of the application, just the
presentation. It can be described as the V in MVC. But it's widely
used and liked a lot nowadays, so I decided to include it in this
list.

React is an open-source library maintained by engineers working on
Facebook, Instagram and a sizable community of individual developers
and companies. It's being used by giants such as Netflix, Imgur,
Airbnb and others. Including, of course, Facebook and Instagram.

### A bit of history

React was created by Jordan Walke, a software engineer at Facebook. He
took inspiration from
[XHP](https://www.facebook.com/notes/facebook-engineering/xhp-a-new-way-to-write-php/294003943919/),
a PHP extension which allows XML syntax to be used for the creation of
custom and reusable HTML elements. React saw its first real-world use
when deployed on Facebook's newsfeed in 2011, quickly followed by
being deployed on Instagram one year later. In 2013, at a JSConf US it
was finally open sourced.

### Core concepts

#### One-way data flow

One-way data flow states that a component cannot directly modify any
values stored as properties passed to it, but it can be passed
callback functions which can in turn modify those values. 

In other words, values are stored as HTML properties inside
components. These values cannot be updated directly by the HTML,
instead, the component needs to call a method to do so. This can also
be easily illustrated with an example:

``` 
render() { 
    return <input 
        value={this.state.inputValue} 
        onChange={this.changedValue}></input> 
}
changedValue(e) {
    this.setState({inputValue: e.target.value}); 
} 
``` 

In this example, `<input>` does not control the value stored inside
it. The value can only be updated by calling the `changedValue`
method, which in turn sets the new value using `setState`.

#### Virtual DOM

React was probably what made the whole concept of a Virtual DOM
popular. The DOM was not built with a dynamic UI in mind. As such, it
was never optimized to be used in one. Sure, you can use plain
JavaScript or jQuery to manipulate the it, but in today's day and age
you have to do a lot of DOM manipulation, sometimes across tens of
thousands of nodes which is a huge problem because of how slow and
inefficient it is.

**Enter React's Virtual DOM**

React abstracts the DOM into an in-memory data structure cache which
can easily and _efficiently_ be updated. Once the changes are made,
React automatically finds the differences with the real DOM and
re-renders only what's needed.

#### JSX 

Probably the most controversial concept introduced with React. JSX is
a JavaScript extension syntax which allows the developer to use HTML
tags inside their JavaScript. Now admittedly it may sound crazy for
someone not used to it and it seemed very ugly when I first started to
use it. 

Some of you might think that you are mixing data management with
business logic and presentation. Which you are, but contrary to
people's belief, it's not as bad as you have been led to believe.

React argues that you should only worry about presentation. If you are
doing things right that is. React does not deal with logic or data. It
only cares about how that data is presented. It knows what needs to be
rendered, when it needs to be rendered and how to render it
efficiently (the Virtual DOM). What it doesn't know is what the
application is actually doing with that data.

### What problem is React trying to solve

So far we have established that React is an great tool. But before
jumping headfirst and starting to use it in your latest project, you
should try to understand what problem it's trying to solve.

**Basically, any application that needs to change its state a lot will
benefit from React.**

One concept we did not discuss earlier, but from which React gains a
lot is Functional Reactive Programming. Functional Reactive
Programming (FRP for short) is a programming paradigm which uses
building blocks of functional programming (map, reduce, filter) in
reactive programming. This paradigm is used in robotics, music and
graphical user interfaces.

In layman terms, FRP allows you to keep your app state constrained to
a single place, and the app itself is mostly just a bunch of stateless
functions which react to state changes and send their results to React
which in turn re-renders the updated DOM with the new data.

This is an improvement to the "old way" of doing things through
imperative programming where there's a single state made up of objects
which is constantly changed by a bunch of methods and functions. This
may very easily lead to bugs and issues related to the application
state becoming unpredictable and breaking things.

This also ties to the Virtual DOM. In order to update the data in an
application with lots of state changes you would need a lot of
processing power to update the DOM since as previously established,
it's not very efficient. This is why the Virtual DOM is so important
to React. Without it, React applications would be slow and very
inefficient.

Hopefully it should be quite clear by now what React is supposed to
do. If you are planning to build a web application with lots of state
changes, the FRP offered by React will help ensure as few bugs as
possible and will eliminate the chance for the app state to reach any
unexpected outcome, while keeping everything as fast and efficient as
possible by using the Virtual DOM.
