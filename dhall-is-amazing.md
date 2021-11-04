---
slug: dhall-is-amazing
title: Dhall is Amazing
date: 2021-08-25
---

Dhall is a programmable configuration language that you can think of as: JSON + functions + types + imports.

My first introduction to Dhall was in a team I was rotating through, where they used it to write their yaml files, such as Buildkite pipelines, Kubernetes definitions and AWS cloudformation templates.

I had previously worked with all of these, except I wrote them in yaml, and experienced literally every problem that Dhall solves. Indentation errors, invalid schema, incorrect types, small typos... Learning Dhall was challenging at first, but made me much more productive and quick when I got used to it.

It is SO beneficial to have the language server throw errors immediately, rather than write some yaml, check it in, wait however long for the pipeline only for it to fail, and repeat.

To talk to Dhall's description as 'JSON + functions + types + imports':

* JSON is json, you can still write regular json and it will compile to json/yaml fine.

* Functions are really nice because they help to DRY the config, and we can have dynamic templates depending on a parameter, which could be for example, the hosting environment.

* Types are great because it forces you to pass in the correct type. We can define our own types so types can require other types. No more indentation errors or missing fields!

* Imports are also really nice because it allows us to separate out our config into more workable files. For example, A cloudformation template could define 10 resources in one file, and end up as one huge file; Or we can define each resource in their own file and import them into a 'main' one, resulting in 11 more maintainable files.

I even enjoyed it so much I hosted a workshop for my graduate cohort to go through a demo.

The Dhall documentation is really amazing, and the examples on the [official website](https://dhall-lang.org/) aren't contrived at all. The only pain point I found is that dhall is not installed on most CI tools, so you will need to install it first. The team used `nix`, but I guess you could use `brew install dhall`.

These are the open source packages we used to define:

* [Buildkite Pipelines](https://github.com/myob-oss/buildkite.dhall)
* [Cloudformation](https://github.com/jcouyang/dhall-aws-cloudformation)
* [Kubernetes](https://github.com/dhall-lang/dhall-kubernetes)
