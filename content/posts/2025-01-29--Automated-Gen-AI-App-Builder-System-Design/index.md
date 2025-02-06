---
title: Generative AI App Builder - Views and Navigation
date: "2025-02-06"
template: "post"
draft: false
slug: "/posts/gen_ai_app_builder_views_and_navigation"
category: "Software Development Practice"
tags:
  - "Software"
  - "AI"
  - "Automated Generative AI"
description: "2. Creating a view and navigation structure using the Generative AI App Builder"
socialImage: "./media/More_AI_Tools_Including_Aider.png"
---

# About

In this post I describe how the Generative AI App Builder tool outlined in the [introduction](../../posts/gen_ai_app_builder_introduction/) can be used to build a set of views using MVVM as a pattern for the view - domain model structure, and using an environment based Router with NavigationStack to perform the navigation between 2 of the views.

To check out the source code used for these experiments in automated generation the repository is [here](https://github.com/MBaldo83/StarteriOSMVVM).

# Key Takeaways

* Is it working?
  * Yes. So far, the small steps taken to script against an AI coding tool have worked, although the code written is simple and small, the system does work as expected.
* Is it worth the effort?
  * Not yet known. So far in this project I've put in a lot of effort to generate views that could have been hand-written in a fraction of the time. However, a lot of time has been spent establishing the AI Generator structure to build upon going forwards. At this point all that is clear is that there is a lot of up-front investment needed build a system like this, and at the moment it's still quite far from building full apps, but the small pieces are working. The question of 'is it worth it' cant be answered  until we have a wider set of features to use and we test the system building some real-world app architectures.
  * The good thing about effort put into generative software is that the effort is 'banked', once created the system becomes useful with minimum effort to operate.
* How close is this to building real-world app projects?
  * At the moment this is still a way off. We would need to cover items like networking integration, persistent storage, deep-linking, identity management, as well as more complex state management like shared state passed between views.

# Generating View & Navigation Code from Feature Specs

## Collection View - *Saved Decks*

To generate views we distinguish between collections / single item models.
We have to define the name, where to create the files and the domain models used by the view.

```swift
static func savedDecksViewFeatureSpec() -> MVVM.ViewSpecification {
        .init(
            viewName: "SavedDecksView",
            viewFolderPath: "\(AiderControl.Constants.appModuleRoot)Views/",
            models: [
                .init(
                    variableName: "savedDecks",
                    modelType: "LocalDeck",
                    modelPath: "\(AiderControl.Constants.appModuleRoot)Domain/LocalDeck.swift",
                    isCollection: true
                )
            ]
        )
    }
```

## Details View *Deck Details*

```swift
static func deckDetailViewSpecification() -> MVVM.ViewSpecification {
        .init(
            viewName: "DeckDetailsView",
            viewFolderPath: "\(AiderControl.Constants.appModuleRoot)Views/",
            models: [
                .init(
                    variableName: "deck",
                    modelType: "LocalDeck",
                    modelPath: "\(AiderControl.Constants.appModuleRoot)Domain/LocalDeck.swift",
                    isCollection: false
                )
            ]
        )
    }
```

## Navigation Spec

To generate navigation links we need to know where the navigation is from and to, we describe what triggers the navigation. One of the advantages of using LLMs to generate code is that unlike traditional code generation, we have a lot more readable descriptions of how a product behaves in our specification files.

```swift
static func savedDecksToDeckDetailNavigationSpec() -> EnvironmentRouterNavigation.NavigationLink {
        .init(
            from: .init(
                // mapping from mvvm views to router navigation spec.
                // These 2 packages are de-coupled to allow flexibilty to choose other architectures
                mvvmView: .savedDecksViewFeatureSpec()
            ),
            to: .init(mvvmView: .deckDetailViewSpecification()),
            navigateWhen: "a deck is selected from the list of decks",
            dataMappings: [
                .init(use: "savedDecks items in collection", toCreate: "deck")
            ],
            routes: .init(
                filePath: "\(AiderControl.Constants.appModuleRoot)Navigation/Route.swift",
                name: "Router.Routes"
            ),
            viewBuilder: .init(
                filePath: "\(AiderControl.Constants.appModuleRoot)Navigation/SwiftUIRouteViewBuilder.swift",
                name: "SwiftUIRouteViewBuilder"
            )
        )
    }
```

The full code produced can be seen here:
* [Saved Decks](https://github.com/MBaldo83/StarteriOSMVVM/blob/0fef4c5a915576e2563df6356bbe1355e50ba477/AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Views/SavedDecksView.swift)
* [Deck Details](https://github.com/MBaldo83/StarteriOSMVVM/blob/0fef4c5a915576e2563df6356bbe1355e50ba477/AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Views/DeckDetailsView.swift)
* [Deck Generator](https://github.com/MBaldo83/StarteriOSMVVM/blob/0fef4c5a915576e2563df6356bbe1355e50ba477/AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Views/DeckGeneratorView.swift)
* [Route](https://github.com/MBaldo83/StarteriOSMVVM/blob/0fef4c5a915576e2563df6356bbe1355e50ba477/AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Navigation/Route.swift)
* [View Builder](https://github.com/MBaldo83/StarteriOSMVVM/blob/0fef4c5a915576e2563df6356bbe1355e50ba477/AppGenAISwiftUIStarter/AppGenAISwiftUIStarter/Navigation/SwiftUIRouteViewBuilder.swift)

## Sample Output: Collection View - *Saved Decks*

This is the view, view model and navigation for the collection of *Saved Decks* defined above. It's simple but functional, and demonstrates that the system does produce working output. 

```swift
struct SavedDecksView: View {
    
    @State var viewModel: ViewModel
    @Environment(Router.self) private var router: Router
    
    var body: some View {
        SavedDecksContentView(
            decks: viewModel.decks,
            viewActionOne: viewModel.viewActionOne,
            navigateToDeckDetails: { deck in
                router.navigateTo(.deckDetails(deck: deck))
            }
        )
    }
}

struct SavedDecksContentView: View {
    let decks: [LocalDeck]
    let viewActionOne: () -> Void
    let navigateToDeckDetails: (LocalDeck) -> Void

    var body: some View {
        VStack {
            Button(action: viewActionOne) {
                Text("View Action 1")
            }
            ForEach(decks) { deck in
                Button(action: {
                    navigateToDeckDetails(deck)
                }) {
                    Text(deck.name)
                }
            }
        }
    }
}

extension SavedDecksView {
    @Observable
    class ViewModel {
        var decks: [LocalDeck]
        
        init(decks: [LocalDeck]) {
            self.decks = decks
        }
        
        func viewActionOne() {
            // Placeholder for view action.
            if let firstDeck = decks.first {
                decks[0] = LocalDeck(id: firstDeck.id, name: firstDeck.name + "!", questions: []) // Question is not defined, so I'm using an empty array here.
            }
        }
    }
}
```
