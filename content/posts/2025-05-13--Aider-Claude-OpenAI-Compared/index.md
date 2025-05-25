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
* extra tooling on top of CLI: 2-3hrs work building supporting system
* time to implement: 5 mins
* cost: $0.05
* prompts used: 2 prompts; 71 and 85 words

claude code:
* extra tooling on top of CLI: none
* time to implement: 1-2 mins
* cost: $2.16
* prompts used: 2 prompts; 71 and 85 words

openai codex:
* extra tooling on top of CLI: none
* time to implement: 15-20 mins
* cost: $0.94
* prompts used: 4 prompts; 22, 43, 34 and 51

## Key Takeaways

### What works well

### What to watch out for

# Tools Comparison Details

## Aider

* What is it: [Aider-chat](https://aider.chat) is an open source and free command line tool allowing you to write / edit code using natural language prompts, inferencing with [many of the popular](https://aider.chat/docs/llms.html) LLMs available. It maps the structure of your repo, makes the requests to the LLM, and edits your source code providing a summary of the changes through the terminal.
* At time of writing, to run like an agent moving around the codebase changing multiple files and reviewing examples, I had to build a system of scripts around the product to load files and direct the code changes
* The cost was very cheap to run the inferencing itself, but required a lot of up-front work to facilitate behaviour that is now out of the box for other tools

### Prompts used for aider:
```text:wrap
Following the example of ExampleMoviesAPI, using http://localhost:8001/openapi.json for endpoint details add new cases to CafeProductsAPI. /products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse Only decode the values required by the domain model.
```
```text:wrap
Following the example of NetworkMoviesAPIClient, using CafeProductsAPI for endpoint details add new methods to CafeProductsClient. /products will be decoded to Array of DecodableCafeProduct then the Result success block will map to Array of CafeProduct. /products/{product_id} -> GET will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. /products/{product_id} PUT will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. This is a first draft, keep the solution simple. IMPORTANT: implement the solution without asking any questions
```

## Claude Code:

* What is it: [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) is an agentic coding tool from Anthropic that runs in your terminal, understands your codebase, and uses natural language commands to implement code changes.
* Was able to handle a large prompt that fully encapsulated the requirements for the changes
* Searched in the repo for the relevant example files without these needing to be explicitly directed

## Open-AI Codex:


## Prompt used for claude code:
```text:wrap
Following the example of ExampleMoviesAPI in /AIGenerator/Sources/Package_NetworkingAsyncEndpointDescriptor/NetworkingExample/NetworkingExample/ExampleMoviesAPI.swift, using http://localhost:8001/openapi.json for endpoint details add new cases to CafeProductsAPI in /AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Repositories/CafeProductsAPI.swift. /products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse Only decode the values required by the domain model.
```
```text:wrap
Following the example of NetworkMoviesAPIClient, using CafeProductsAPI for endpoint details add new methods to CafeProductsClient. /products will be decoded to Array of DecodableCafeProduct then the Result success block will map to Array of CafeProduct. /products/{product_id} -> GET will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. /products/{product_id} PUT will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. This is a first draft, keep the solution simple. IMPORTANT: implement the solution without asking any questions
```

**Total Cost: $2.16** (whole session)

## Prompt used for openai codex:
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


```text:wrap
ExampleMoviesAPI.swift, using http://localhost:8001/openapi.json for endpoint details add new cases to CafeProductsAPI in /AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Repositories/CafeProductsAPI.swift.

Thinking...

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
Open AI did not create a new `DecodableCafeProduct` that could be encoded to JSON (Claude did make this).

```text:wrap
/products will map to Array of DecodableCafeProduct and /products/{product_id} -> GET will map to DecodableCafeProduct and /products/{product_id} PUT will map to DecodableCafeProduct. For each endpoint, all responses should be mapped to a decodable response struct separate to the domain model like ExamplePopularMoviesResponse
```

Single prompt used by other tools split for open-ai codex
```text:wrap
Following the example of NetworkMoviesAPIClient, using CafeProductsAPI for endpoint details add new methods to CafeProductsClient. /products will be decoded to Array of DecodableCafeProduct then the Result success block will map to Array of CafeProduct.
```
```text:wrap
/products/{product_id} -> GET will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. /products/{product_id} PUT will be decoded to DecodableCafeProduct then the Result success block will map to CafeProduct. This is a first draft, keep the solution simple. IMPORTANT: implement the solution without asking any questions
```

**Cost: TODO**