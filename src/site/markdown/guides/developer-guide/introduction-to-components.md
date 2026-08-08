---
title: Developer Guide - Introduction to Plexus Components
author: Rahul Thakur
date: 2006-06-17
---

# Introduction to Components

## What are Components?

[ Szyperski](http://www.amazon.com/exec/obidos/ASIN/0201745720/qid%3D1090125966/sr%3D11-1/ref%3Dsr%5F11%5F1/104-8989869-2491134) lists several characteristics of components: composition, units of deployment and resuability.

Other projects have also offered what a component may be:

- [ PicoContainer](http://picocontainer.codehaus.org/Components)

- [ Avalon](http://avalon.apache.org/central/cop/index.html)

- [ C2 - Component Oriented Programming](http://c2.com/cgi/wiki?ComponentOrientedProgramming)

Some descriptions of a component are:

- A nontrivial, nearly independent, and replaceable part of a system that fulfils a clear function in the context of a well-defined architecture. A component conforms to and provides the physical realization of a set of interfaces. (Philippe Krutchen, Rational Software)

- A runtime software component is a dynamically bindable package of one or more programs managed as a unit and accessed through documented interfaces that can be discovered at runtime. (Gartner Group)

- A software component is a unit of composition with contractually specified interfaces and explicit context dependencies only. A software component can be deployed independently and is subject to third-party composition. (Clemens Szyperski, _"Component Software"_)

- A self-contained piece of software that can be independently deployed and plugged into an environment that provides a compatible socket. It has well-defined run-time interfaces, and it can cooperate out of the box with other components (Peter Herzum, Olivier Sims, _"Business Component Factory"_)

## Criteria for Components

Meyer: [ _"Seven Criteria for Components"_](http://www.sdmagazine.com/documents/s\=746/sdm0003k/0003k.htm)

- May be used by other software elements (clients).

- May be used by clients without the intervention of the component's developers.

- Includes a specification of all dependencies (hardware and software platform, versions, other components).
