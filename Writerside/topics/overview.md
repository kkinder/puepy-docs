# PuePy: Overview

PuePy is a frontend web framework that builds on Python and Webassembly using [PyScript](https://pyscript.net). PuePy is
*truly* a Python-first development environment. There is no transpiling to JavaScript; no Yarn, no NPM, no webpack, no
Vite or Parcel. Python runs directly in your browser. PuePy is inspired by Vue.js, but is entirely from scratch in
Python.

## Features

- **Reactivity**: As components' state changes, redraws happen automatically
- **Component-Based Design**: Encapsulate data, logic, and presentation in reusable components
- **Single-Class Components**: Based vaguely on Vue's "single file components", each component and each page is a class.
- **Events, Slots, and Props**: Events, slots, and props are all inspired by Vue and work similarly in PuePy
- **Minimal, Python**: PuePy is built to use Pythonic conventions whenever possible, and eschews verbosity in the name
  of opinion.

## Runtimes

PuePy, by way of PyScript, supports two runtime environments to varying degrees:

- PyScript's [Pyodide](http://pyodide.org) runtime is a nearly complete CPython development environment. It is
  comprehensive, but has a hefty runtime of about 11MB. It is well-suited to fast environments with caching.
- PyScript's [MicroPython](https://micropython.org) runtime is far, far smaller but not nearly as complete. It is ideal
  for situations where webpage response time is important.

See [PyScript's explanation for more information about the differing runtimes](https://docs.pyscript.net/2024.5.2/user-guide/architecture/).


<warning>
HTML5 history mode / client-side routing is <emphasis>not</emphasis> supported with MicroPython. If you use MicroPython, you need to handle routing on the server-side and render each web page in a traditional way. This is probably better anyway, as MicroPython's implementation lacks the memory management features built into the Pyodide runtime.
</warning>

