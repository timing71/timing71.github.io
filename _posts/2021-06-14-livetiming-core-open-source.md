---
layout: post
title:  "livetiming-core is now open-source"
---

I'm pleased to announce that `livetiming-core`, the backend library used by Timing71,
is now [available on GitHub under the AGPL license](https://github.com/timing71/livetiming-core)!

## What is livetiming-core?

`livetiming-core` is the backend library used server-side on Timing71. It
contains the necessary code to write your own timing services for the Timing71
Desktop Client or other purposes.

With `livetiming-core`, you can:

- Write your own service plugin to transform data for use in the Desktop Client
- Manipulate recording ZIP files
- Generate analysis files from recording ZIP files

`livetiming-core` is written in Python.

## What is livetiming-core _not_?

`livetiming-core` does **not** include any code that interfaces with any
specific timing data provider. In order to protect and honour the work of those
companies whose data makes Timing71 possible, and to avoid any possible legal
challenges, there is no code in this repo relating to directly extracting
data from any other source.

It also doesn't include any of the frontend code for the website or desktop
client.

If you're interested in writing your own timing service, you can see an example
in the [livetiming-plugin-example repo](https://github.com/timing71/livetiming-plugin-example)
also on GitHub.

## Getting started

Head over to the [livetiming-core repo]((https://github.com/timing71/livetiming-core))
to get started.
