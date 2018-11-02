---
title: "Amethyst: The Gem in the Rust"
date: 2018-10-28T13:00:00-04:00
draft: false
---

# Amethyst: The Gem in the Rust

__I originally wrote this all up in preparation for the talk I gave at the Rust NYC October Meetup in 2018. The slides are available 
[here][slides]. This is just a translation of what I said but in blog form_

[slides]: https://www.slideshare.net/LucioFranco/amethyst-the-gem-in-the-rust-120999299

## What is a Game Engine?

A game engine is usually just a collection of libraries that are glued together to provide easy utilities to build a game. This includes things like asset loading (think PNG files and 3D models), render utilities (think of things that allow you to draw stuff without thinking of the lower level code), input capturing, and UI Components. Game engines usually become very large code bases. They can grow very quickly which makes it hard to maintain a uniforn API and verify consitency across the whole project. Engine components are usually very tightly coupled, meaning that the dependency graph can get _CRAZY_. This part relies on that part which relies back on that part. It gets hectic! Because of this coupling it creates issues with paralleization and memory saftey.

## What is Amethyst?

Amethyst is, you guessed right, a game engine! It is built up of many small modular components (crates) that can be used indepdently from each other or can be used together glued by the `amethyst` crate. The backbone of the project is an Entity Component System or ECS. An Entity Component System is a method to describe the game state. It follows the pattern of composition over inheritance. This pattern fits rusts trait system extremely well. The ECS setup allows the engine to be data oriented and data driven. Both of these combined allows the engine to have parallelism at its core. Using composition also allows the user to focus on reusability and clean interfaces.

## History

The project was originally created as a hobby game engine by [Eyal][eyal] in early 2016. It soons started to grow and required a movement to an organization. From this organization and its members, [Specs][specs] was born. Specs is the ECS library that amethyst is built off of. From this libraries such as [Shred][shred] were born to provide utilities on top of specs to take advantage of the parallel and data oriented nature.

Now, Amethyst is growing as a top choice for an all purpose rust game engine. There are other similar libraries but none of them attempt to achieve what Amethyst is trying to do. Currently, the repo sits at 35.7K (as of October 2018) lines of code. Not that this means too much but more a measure of scale of the project. This size is only the engine and does not include its external dependencies. We have a book that is growing day by day. It includes almost fully featured pong tutorial and other useful insights. We have 22 examples from hello world to advanced gLTF loading. Amethyst, currently fully supports all three main operating systems with a future goal of supporting iOS, Android and WASM.

The list of features is HUGE, we have modular rendering system built from gfx, which is a multi backend graphics api. Meaning, it can support OpenGL, Metal, Vulkan, and DirectX based apis. There is support for game pads and controllers. Multiplayer, User interfaces, animations, ECS, Inputs, Configuration loading via Rust object notation, think JSON but for Rust. Asset loading and an advanced statemanager.

The future holds a lot for the engine, we are currently, working on a new renderer that will take advantage of the new modern rendering apis such as vulkan and metal. Better networking, this is the area that I focus on, there is work going into a semi reliable UDP protocol for game networking. Editors, scripting, future support for WASM and WebGL, (write your game in rust that runs anywhere a browser can!). Mobile support and a REPL.

## How does Rust Help?

Now, why use rust for this project, and why is this the future of game development. Rust supports awesome parallization primatives in the standard library. Such as RwLocks etc. It also has great libraries for this stuff including crossbeam and rayon. This allows specs and the ECS stystem to be really fast. Not only does it help with speed but it also lets us take advantage of the new rendering apis that are no longer single threaded based. Meaing that we can dispatch render calls from any thread. Easy to build and support rendering optimzations such as framegraphs. More writing code and less tracing data races and segfaults, but Im sure you all have heard this enough.

The compiler is your best friend in rust, after you learn to properly use the borrow checker. The type saftey and checking provides an ease of use when it comes to producing high quality code that WORKS. Trait composition fits very nicely into the ECS model. Type inference allows you to not spend time on trying to type out one type 8 times, looking at you java. Bugs, the compiler just stops you from producing a whole class of bugs. This is a sanity check for game developers who want to release a game and sleep at night.

Cargo, is my favorite package manger, its simple, easy to use and does a great job. It allows us to build, run and test our whole project incredibly easily. We can run benchmarks to find regressions or just see how fast our code is. Built in documentation allows us to provide a documenation first perspective. Instead of approaching new people with "get good" we can approach it all by saying, we should add this to the docs. This promotes a great community atomsphere but also improves the docs for future users. Cargo also does a great job at dependency managment and making sure our code doesnt get broken. These are just a few of the improvments a rust game engine has over say a C++ one.

## Getting started!

Checkout the [Amethyst][amethyst] website. Come join us on discord! 

[eyal]: https://github.com/ebkalderon
[soecs]: https://github.com/slide-rs/specs
[shred]: https://github.com/slide-rs/shred
[amethyst]: https://amethyst.rs
