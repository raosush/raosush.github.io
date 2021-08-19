---
layout: post
title: Ruby GSoC '21 Final Report
date: 2021-08-19 10:00:04 +0530
author: 'Sushanth Sathesh Rao'
type: 'Work Product Submission'
excerpt: Final report regarding my work completed while working on a project with Ruby during the period of GSoC '21
tags: gsoc rbs ruby
asset_img: '/assets/img/gsoc_ruby_final_report/main.png'
---

## Background
[RBS](https://github.com/ruby/rbs) is a newly developed syntax language for describing structure of Ruby programs, released with **Ruby 3.0.0**. RBS is a very useful addition to implement __static__/__runtime__ type checking for Ruby, which is a dynamically typed language.

## Project
My GSoC [project](https://summerofcode.withgoogle.com/projects/#5569460552859648) with Ruby aimed at providing signatures for standard libraries & third party gems, and improving/fixing the existing RBS toolchain. Providing high quality signatures for various libraries in Ruby would mean an expansion of the userbase of RBS. A feature rich & fullproof toolchain would encourage more users to reap the benefits of RBS.

#### Specifications:
* __Project__: [Writing of RBS files](https://summerofcode.withgoogle.com/projects/#5569460552859648)
* __Mentor__: [Soutaro Matsumoto](https://github.com/soutaro)
* __Organisation__: [Ruby](https://summerofcode.withgoogle.com/organizations/4697800446574592/)

## Descriptive Goal

* This project is split into two components:
  * [Provding RBS writeup for standard & third party libraries](#RBS-Writeup)
  * [Fixing/improving the current RBS toolchain](#Improving-RBS)

* ### RBS Writeup
  * This project was intended to start off by me working on providing RBS writeup of standard & third party libraries
  * This would help me to understand the functionality provided by RBS & it's many use cases.
  * It would also help me find a few rough edges/scope for improvement in the RBS codebase

* ### Improving RBS
  * After having gained sufficient exposure & knowledge with RBS, I would be picking a few issues & start working on them.
  * Based on my capabilities & understanding of RBS codebase, I would be working on a variety of issues which would include bug fixes, features & enhancements.

## Work
| Pull Request | Description | Status |
| -------- | -------- | -------- |
| [RBS for chunky_png gem](https://github.com/ruby/gem_rbs_collection/pull/22)     | Provides RBS writeup of [Chunky PNG](https://github.com/wvanbergen/chunky_png) gem, v1.4.0     | Merged     |
| [RBS for net-http gem](https://github.com/ruby/rbs/pull/686) | Provided RBS writeup of [Net::HTTP](https://github.com/ruby/net-http) gem, v0 | Merged |
| [RBS for HTTParty gem](https://github.com/ruby/gem_rbs_collection/pull/31) | Provided RBS writeup of [HTTParty](https://github.com/jnunemaker/httparty) gem, v0.18.1 | Merged |
| [RBS for sidekiq](https://github.com/ruby/gem_rbs_collection/pull/34) | Provided RBS writeup of [Sidekiq](https://github.com/mperham/sidekiq) gem, v6.2 | Merged |
| [Generate nested declarations](https://github.com/ruby/rbs/pull/700) | Supports generation of nested declarations while prototyping RBS from Ruby runtime APIs | Merged |
| [Remove documentation about `super`](https://github.com/ruby/rbs/pull/716) | Removed inconsistent documentation about `super` type | Merged |
| [Removed documentation about runtime testing](https://github.com/ruby/gem_rbs_collection/pull/41) | Remove inconsistent documentation about support for `runtime test` in RBS | Merged |
| [Add Recusrive type alias validation](https://github.com/ruby/rbs/pull/719) | Added validation for detecting & pointing out recursive type alias definitions | Merged |
| [Generate included modules with complete name](https://github.com/ruby/rbs/pull/731) | Generates complete name for included/prepended/extended modules, i.e, with their namespace | Merged |
| [Generate RBS from JSON Schema](https://github.com/ruby/rbs/pull/730) | Generates RBS type definitions from a JSON Schema | Approved |

### Repositories

* [RBS](https://github.com/ruby/rbs)
* [gem_rbs_collection](https://github.com/ruby/gem_rbs_collection)
* [PRs](https://github.com/ruby/rbs/pulls?q=is%3Apr+author%3Araosush+)
* [PRs](https://github.com/ruby/gem_rbs_collection/pulls?q=is%3Apr+author%3Araosush+)

## Completed Work

### Part 01 - RBS Writeup

* Provided RBS writeup of Chunky PNG gem, which included signatures of the APIs provided by the gem & tests. Chunky PNG is a pure Ruby based PNG modifier which is a useful & famous gem.
* Provided RBS writeup of `net-http` gem, which included signatures of the APIs provided by the gem & tests to check for compliance of signatures provided with application code. `net-http` gem is a standard library which is an extremely useful & famous gem for working with programmatic HTTP requests.
* Provided RBS writeup of `HTTParty` gem, which included signatures of the APIs provided by the gem & tests. `HTTParty` is an extremely prominent & useful gem which encapsulates the functionality of `net-http` gem & provides ability to implement methods in a custom class.
* Provided RBS writeup of `Sidekiq` gem, which included signatures of the APIs provided by the gem & tests. `Sidekiq` is a really famous & useful gem which provides support for Ruby based applications to efficiently handle background processing.

### Part 02 - Improving RBS Toolchain
* Worked on generating nested declarations while prototyping RBS from Ruby runtime APIs. This was a significant improvement since compact declarations had a few downsides.
  * ![](/assets/img/gsoc_ruby_final_report/nested_runtime_prototype.png)
* Worked on adding validations for recursive type aliases. This was a good feature that also helped me to gain a lot more understanding of RBS codebase, good coding conventions & data structures.
  * ![](/assets/img/gsoc_ruby_final_report/recursive_type_alias_validation.png)
* Worked on generating RBS from JSON Schema documents. This was a great feature to work on as it helped me learn a lot about JSON schema & it's usecases. JSON Schema documents are far more expressive & extensible than RBS and is used for a variety of applications. Hence, support for translating types from JSON schema makes RBS even better.
  * ![](/assets/img/gsoc_ruby_final_report/rbs_json_schema.png)

## Learning Outcomes

* One of the best learning outcomes of this project was a practical experience in coding, following SOLID coding principles.
* I learnt a lot about type systems, type checking, etc. My mentor provided me with resources to get started off with understanding the basics of types & type systems I always had a knack for learning about the underlying & abstract implementations of programming languages. This project exposed me to a variety of concepts & techniques which helped me understand the working of a programming language.
* I also got to learn about writing tests for a variety of use cases & the use cases of writing tests while maintaining a large codebase.
* After having worked on this project, I got to learn a lot about the open source culture, such as planning, brainstorming about ideas & approaches, structured way of coding, involving with the community & contributing to projects.
* I also got to learn about designing & implementing a clean, user friendly interface while building command line based utilities.

## Acknowledgements

I would like to sincerely thank my mentor [Soutaro Matsumoto](https://github.com/soutaro) sir for having guided me throughout the course of this project. He was really very helpful & always provided timely constructive feedback regarding my work & ideas. As GSoC mentions in one of it's guides, mentor is the most important asset for a student in GSoC, which was true in my case. My mentor was truly an asset for me during the course of this program. Once again I would like to thank my mentor for having assisted me throughout the program. I would also like to thank the Ruby community for their assistance & guidance. I wish to continue contributing to this wonderful community of Ruby developers to the best of my capacity

I would also like to thank [Mohit Tahilani](https://github.com/mohittahiliani) sir, [Govind Jeevan](https://github.com/govindjeevan), [Pavan Vachhanih](https://github.com/vachhanihpavan), [Abhishek Kumar](https://github.com/abhishekkumar2718), [Salman Shah](https://github.com/mohittahiliani) & [Anumeha Agarwal](https://github.com/anumehaagrawal) for motivating & guiding me in applying to this program.

I would like to sincerely thank Google for organising such a program which helps several students like me get started with their open source journey & become long standing open source contributors. It is an extra ordinary feeling to look at several developers use tools built or coded by you for various purposes. I sincerely wish to continue contributing to open source.