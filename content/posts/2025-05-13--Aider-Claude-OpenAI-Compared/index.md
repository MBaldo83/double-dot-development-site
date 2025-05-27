---
title: Aider, Claude, Open-AI ? TODO FILL PRODUCT NAME ? compared
date: "2025-05-13"
template: "post"
draft: false
slug: "/posts/aider_claude_openai_latest_comparison"
category: "Software Development Practice"
tags:
  - "Software"
  - "AI"
description: "Comparing the output of Aider vs Claude vs Open AI as agentic code generators building a network client from Open API spec."
socialImage: "./media/AI_Tools_and_Flutter.png"
---

# About

Plan:

In iOS project AppGenAISwiftUIStarterApp (`StarteriOSMVVM`). 
- Delete the Cafe Endpoints Networking client
- make compile with dummy data
- run aider script to build network API
  - document what's needed to run script
  - document output
- run claude code using same command, but without the AIGenerator Swift Project
- run open AI code gen using same command, but without the AIGenerator Swift Project

# Summary of Findings

aider:
* time to implement: 5 mins
* cost: $0.05
* prompts used: 2 prompts; 71 and 85 words. 5-6 'fix' prompts to fix build errors

claude code:
* extra tooling on top of CLI: none
* time to implement: 1-2 mins
* cost: $2.16
* prompts used: 2 prompts; 71 and 85 words, no fixes needed built after 1 shot

openai codex:
* extra tooling on top of CLI: none
* time to implement: 15-20 mins
* cost: $0.94
* prompts used: 4 prompts; 22, 43, 34 and 51 words

## Key Takeaways

### Tools Comparison

* For this task Claude Code (using claude-sonnet-4 LLM) gave the best results, but was the most expensive
* Aider chat is a good, cost-effective alternative and no longer requires any supporting scripts to run in agent mode
* Open AI codex produced good working code, but struggled with large prompts (over 50 words), these needed to be broken into smaller prompts

### Agents and MCPs have replaced the need to build scripts around the tools

In previous articles, eg [Generative AI App Builder - Introduction](../../posts/gen_ai_app_builder_introduction/), I built a system of scripts to essentially do 2 things:
1. Generate prompts from feature specifications
2. Invoke aider through the script to run 'headless-ly', adding the files needed by the tool to perform each task

A lot has changed since this article was written and much of the work needed previously is now available out of the box in the tools.

1. Instead of generating prompts from feature specifications, [MCP servers](https://modelcontextprotocol.io/introduction) have standardised how applications provide context to LLMs, and integrations are available for a lot of tools where feature specs are traditionally written. For example there are [multiple Jira MCPs](https://cursor.directory/mcp?q=jira), [figma](https://cursor.directory/mcp/figma) and [shortcut](https://github.com/useshortcut/mcp-server-shortcut) MCP servers available. To solve the problem of accurately describing the requirements to an LLM, we can keep the UI Designs in a dedicated design tool and the feature expectations in whatever project management tool we want to use, referring to the specifications through the MCP integration.
2. Instead of requiring custom tooling to manually move around your project referencing different files and breaking the overall problem into smaller prompts, AI assistant tools have become increasingly better at running in 'agent' mode. Essentially, taking a single prompt that might contain multiple requirements and internally converting this into a series of internal prompts and commands to step by step achieve the goal of the goal.

### What works well

### What to watch out for

# Comparison Details

## Aider

* What is it: [Aider-chat](https://aider.chat) is an open source and free command line tool allowing you to write / edit code using natural language prompts, inferencing with [many of the popular](https://aider.chat/docs/llms.html) LLMs available. It maps the structure of your repo, makes the requests to the LLM, and edits your source code providing a summary of the changes through the terminal.

### Prompts used for aider:

1st Prompt:
```text:wrap
Following the example of ExampleMoviesAPI, using http://localhost:8001/openapi.json for endpoint details add new cases to CafeProductsAPI. /products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse Only decode the values required by the domain model.
```
* Aider added the required files to it's context, and was able to successfully reach and parse the localhost response. 
* The initial implementation did not compile and required 2 'fix' prompts to fix the issue providing the compiler issue each time

2nd Prompt:
```text:wrap
Following the example of NetworkMoviesAPIClient, using CafeProductsAPI for endpoint details add new methods to CafeProductsClient. /products will be decoded to Array of DecodableCafeProduct then the Result success block will map to Array of CafeProduct. /products/{product_id} -> GET will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. /products/{product_id} PUT will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. This is a first draft, keep the solution simple. IMPORTANT: implement the solution without asking any questions
```
* Required 1 'fix' prompt to fix errors
* needed 2 more prompts to draw the dots together and complete the implementation

Code Produced:
* [CafeProductsAPI](https://gist.github.com/MBaldo83/9eb1531eab3d7fe6515d8e004ee306a2#file-cafeproductsapi-swift)
* [CafeProductsClient](https://gist.github.com/MBaldo83/9eb1531eab3d7fe6515d8e004ee306a2#file-cafeproductsclient-swift)

**Total Cost: $0.13** (whole session)


## Claude Code:

* What is it: [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) is an agentic coding tool from Anthropic that runs in your terminal, understands your codebase, and uses natural language commands to implement code changes.
* Was able to handle a large prompt that fully encapsulated the requirements for the changes
* Searched in the repo for the relevant example files without these needing to be explicitly directed

### Prompt used for claude code:

1st Prompt:
```text:wrap
Following the example of ExampleMoviesAPI in /AIGenerator/Sources/Package_NetworkingAsyncEndpointDescriptor/NetworkingExample/NetworkingExample/ExampleMoviesAPI.swift, using http://localhost:8001/openapi.json for endpoint details add new cases to CafeProductsAPI in /AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Repositories/CafeProductsAPI.swift. /products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse Only decode the values required by the domain model.
```
* Complained about not being able to reach the Open API endpoint
* Code compiled 1st time.

2nd Prompt:
```text:wrap
Following the example of NetworkMoviesAPIClient, using CafeProductsAPI for endpoint details add new methods to CafeProductsClient. /products will be decoded to Array of DecodableCafeProduct then the Result success block will map to Array of CafeProduct. /products/{product_id} -> GET will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. /products/{product_id} PUT will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. This is a first draft, keep the solution simple. IMPORTANT: implement the solution without asking any questions
```
* Claude outputs to the terminal how it's going to break down the problem, writing a list of 'todos', and ticking them off as it iterates through the goal.
Example Claude output:

```text:wrap
⏺ I'll implement the CafeProductsClient methods based on the example of NetworkMoviesAPIClient. Let me first examine the structure and create a todo list.

⏺ Update Todos
  ⎿  ☐ Examine NetworkMoviesAPIClient for reference
     ☐ Examine CafeProductsAPI for endpoint details
     ☐ Implement GET /products method in CafeProductsClient
     ☐ Implement GET /products/{product_id} method in CafeProductsClient
     ☐ Implement PUT /products/{product_id} method in CafeProductsClient

⏺ Update Todos
  ⎿  ☒ Examine NetworkMoviesAPIClient for reference
     ☒ Examine CafeProductsAPI for endpoint details
     ☒ Create CafeProductsClient class and protocol
     ☒ Implement GET /products method in CafeProductsClient
     ☒ Implement GET /products/{product_id} method in CafeProductsClient
     ☒ Implement PUT /products/{product_id} method in CafeProductsClient
```

**Total Cost: $2.16** (whole session)

## Open AI - Codex

### 1st Prompt used for openai codex (failed):

```text:wrap
Following the example of ExampleMoviesAPI in /AIGenerator/Sources/Package_NetworkingAsyncEndpointDescriptor/NetworkingExample/NetworkingExample/ExampleMoviesAPI.swift, using http://localhost:8001/openapi.json for endpoint details add new cases to CafeProductsAPI in /AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Repositories/CafeProductsAPI.swift. /products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse Only decode the values required by the domain model.
```
Result:
We had a lot of....
```text:wrap
278s  Thinking...
310s  Thinking... 
```
```text:wrap
   system
    ⚠️  Network error while contacting OpenAI. Please check your connection and try again.
```

To make this work I needed to split the prompt:

Single prompt used by other tools split for open-ai codex:
```text:wrap
Following the example of NetworkMoviesAPIClient, using CafeProductsAPI for endpoint details add new methods to CafeProductsClient. /products will be decoded to Array of DecodableCafeProduct then the Result success block will map to Array of CafeProduct.
```
```text:wrap
/products/{product_id} -> GET will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. /products/{product_id} PUT will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. This is a first draft, keep the solution simple. IMPORTANT: implement the solution without asking any questions
```

After the split followed by a lot more:

```text:wrap
Thinking...
```

Output from open-ai codex
```text:wrap
I've added the three endpoint cases to CafeProductsAPI, conformed it to the Endpoint protocol, and implemented its required properties (path, method, urlComponents, headers, httpBody) following the pattern
in ExampleMoviesAPI. Specifically:

    * **Conform** `CafeProductsAPI` to `Endpoint`.
    * **Added** cases:
        * `.getProducts` (GET / products)

        * `.getProduct(productID:)` (GET / products/{id})

        * `.updateProduct(product:)` (PUT / products/{id})
    * **Implemented**:
        * `path` switches to the proper "products" paths.

        * `method` returns `.get` for the GET cases and `.put` for the update.

        * `urlComponents` is empty for all cases.

        * `headers` includes `Content-Type: application/json` for the PUT case.

        * `httpBody` JSON-encodes the `CafeProduct` for PUT, and is `nil` for GETs.
```

1st result: failed to build:
```
Instance method 'encode' requires that 'CafeProduct' conform to 'Encodable'
```
Even after prompting with the 2nd half of the requirements:
```text:wrap
/products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse Only decode the values required by the domain model.
```
Open AI did then create a new `DecodableCafeProduct` that could be encoded to JSON, but didn't include it in the enum as the associated value. 

```text:wrap
/products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse
```
Creating the network _client_ (as opposed to the API), was also split across two requests:

1st Prompt:
```text:wrap
Following the example of NetworkMoviesAPIClient, using CafeProductsAPI for endpoint details add new methods to CafeProductsClient. /products will be decoded to Array of DecodableCafeProduct then the Result success block will map to Array of CafeProduct. 
```

2nd Prompt:
```text:wrap
/products/{product_id} -> GET will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. /products/{product_id} PUT will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct.
```

**Cost: $0.94**