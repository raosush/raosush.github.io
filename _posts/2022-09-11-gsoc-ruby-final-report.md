---
title: Ruby GSoC '22 Final Report
description: Final report regarding my work completed while working on a project with Ruby during the period of GSoC '22
date: 2022-09-11 10:54:04 +0530
author: 'Sushanth Sathesh Rao'
type: 'Work Product Submission'
excerpt: Final report regarding my work completed while working on a project with Ruby during the period of GSoC '22
tags: gsoc rbs ruby
asset_img: '/assets/img/gsoc_ruby_final_report/main.png'
---
# Ruby - GSoC '22 Final Report

## Background
[RBS](https://github.com/ruby/rbs) is a newly developed syntax language for describing structure of Ruby programs, released with **Ruby 3.0.0**. RBS is a very useful addition to implement __static__/__runtime__ type checking for Ruby, which is a dynamically typed language.

## Project
My GSoC [project](https://summerofcode.withgoogle.com/programs/2022/projects/1wSHVbTn) with Ruby aimed at implementing a plugin for RDoc to generate documentation from RBS source code and upstreaming changes to RDoc to support RBS specific data structures such as Interfaces, Type Aliases, Type Parameters(generics), etc.

#### Specifications:
* __Project__: [RDoc Generation from RBS](https://summerofcode.withgoogle.com/programs/2022/projects/1wSHVbTn)
* __Mentor__: [Soutaro Matsumoto](https://github.com/soutaro)
* __Organisation__: [Ruby](https://summerofcode.withgoogle.com/programs/2022/organizations/ruby)

## Descriptive Goal

* This project is split into two components:
  * [Developing a RDoc plugin to support generating documentation from RBS source code](#RDoc-Plugin)
  * [Upstreaming features to RDoc to support RBS specific data structures](#Improving-RDoc)

* ### RDoc Plugin
  * This project was intended to start off by me working on providing an RDoc plugin to generate documentation from RBS source code
  * This would help me to understand the functionality provided by RDoc, it's many use cases and possible limitations with respect to RBS specific data structures.

* ### Improving RDoc
  * After having gained sufficient exposure & knowledge with RDoc, I would then start working on supporting RBS specific data structures such as Interfaces, Type Aliases, Generics, etc.

## Work
|---
| Pull Request | Description | Status
|:-:|:-:|:-:
| [RDoc Plugin for RBS](https://github.com/ruby/rbs/pull/1048)     | RDoc Plugin for generating documentation from RBS source code     | Merged
| [Support RBS generics in RDoc](https://github.com/ruby/rdoc/pull/925) | Add support in RDoc for RBS specific data structure, type parameters(generics) | In Review
| [Enhance RDoc plugin for RBS with generics](https://github.com/ruby/rbs/pull/1105) | Update RDoc plugin for RBS to support type parameters(generics) | In Review
|---

### Repositories

* [RBS](https://github.com/ruby/rbs)
* [RDoc](https://github.com/ruby/rdoc)

## Completed Work

### Part 01 - RDoc Plugin

* [Implemented a plugin for RDoc](https://github.com/ruby/rbs/pull/1048) which parses RBS source code and generates RDoc based documentation from it.
    * As part of the initial release, the plugin supports only the data structures common to Ruby and RBS, such as `class`, `module`, `method`, `attr_*`, etc.
    * RBS specific data structures such as `Type Aliases`, etc are not supported yet.
    * The plugin has been released in a pre-release version, [`2.7.0.pre.1`](https://rubygems.org/gems/rbs/versions/2.7.0.pre.1)

### Part 02 - Upstreaming changes to support RBS specific data structures
* Upstreamed changes to RDoc to support RBS specific data structure, Type Parameters(generics).
    * Also introduced a new syntax in RDoc markdown format to support the type parameters feature in Ruby & C source as well.
    * The above was done to ensure consistent documentation across RDoc, irrespective of the origin of the documentation.
    * The syntax introduced a new key: `:type-parameters:` and follows the same [syntax as that of RBS](https://github.com/ruby/rbs/blob/master/docs/syntax.md#generics) for defining generics:
    * ```ruby
        #
        # :type-parameters:
        # in Y < String
        # unchecked out Elem < Integer
        # X
        class Test
        end
        # Will result in a documentation as follows
        # class Test [in Y < String, unchecked out Elem < Integer, X]
      ```
      ![](https://i.imgur.com/0DFPybh.png)


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
