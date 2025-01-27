---
title: Automated Generative AI Mobile App Builder 
date: "2025-01-27"
template: "post"
draft: true
slug: "/posts/endpoint_path"
category: "Software Development Practice"
tags:
  - "Software"
  - "AI"
  - "Automated Generative AI"
description: "Automating the generation and the deployment of generative AI prompts"
socialImage: "./media/AI_Tools_and_Flutter.png"
---

# NOTES

# About

I am introducing an approach to writing software that uses 'prompt libraries' plus generative AI tools to write source code from a set of product requirements. In this approach, the prompts used by the AI are not written each time by the developer, but are saved in libraries that are pre-configured to solve common problems using a particular pattern. For example you might choose to use a library that implements a set of views using the MVVM pattern or you could choose The Composable Architecture (TCA). Using this system the same requirements could be used to generate the underlying structure for either approach. What this results in is a development environment where you write software by taking *feature specifications* encoding a description of your product requirements, and *prompt generators* that encode a description of how to write the software.

## But Why?

Before diving into how I've started building this sort of a system. I think it's worth expanding a little bit on why. 

* Generative AI still has so much unrealised potential
  * Generative AI (gen-AI) for software development has developed rapidly over the past few years. It's changed the way many developers work on the *micro* level of writing code where it's already commonplace to write simple functions using gen-AI assistants. However, for big projects there's currently no way to scale this to ask the Gen-AI to keep iterating and writing code in the way we want it to. This approach attempts to join these small simple tasks together to build a more complete set of code from a set of feature requirements. This expands the time-saving potential for gen-AI coding assistants from helping to write a function to writing tens or hundreds of files that all work together as a set of source code.
* Encoding requirements is already useful
  * One major human 'input' of this system involves writing a set of requirements to describe how features should behave that is independent of how they are built. Even without a tool to automate writing the software, these encoded descriptions of features are already very useful when working with LLMs. For example they can be used to check that requirements are covered by tests, or in some situations that the code fulfills the requirements.
* We save time by removing repetitive prompt writing.
  * Saving time by removing repetition is a very common aim in software development, it even has it's own acronym DRY. The system I'm exploring here explores whether we can remove the repetition of prompt writing across any number of projects that are a good fit for the tool. Eg, if this tool can be used to write boilerplate MVVM structure for all the views in one product, and then across hundreds of similar products, the time-savings globally will be impactful.
* Generalising the approach to writing software leads to more predictable code
  * What this approach forces us to do is to define as a set of descriptive prompts for *how* to implement requirements as code within a chosen pattern. Essentially this is already what 1000+ blog posts on MVVM or TCA or whatever network architecture you choose already do. And again privately what 1000s of teams do with code-principles Wiki-pages. What I'm exploring here is using these descriptions of how to write code in a way that actually allows you to automate the process of writing code. By encoding the knowledge and experience of many engineers into the system used to write the code, we can improve the code produced, avoiding bugs and pitfalls that have been learnt through experience.
* If I give more responsibility to AI, I want to guide it
  * As teams start using gen-AI tools more and more, having a structure in place to ensure coding standards are maintained will become ever more useful. Part of this research is to help position developers in a good place to be ready for that change.


# Summary of Findings

## Key Takeaways

### What works well

### What to watch out for

### Future Speculation!