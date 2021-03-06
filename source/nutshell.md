---
title: Vulcan in a Nutshell
---

The best way to understand how Vulcan works is to consider its three main aspects: the role of the schema, how Vulcan reads data, and how Vulcan writes data.

## Overview

[![/images/how-vulcan-works.svg](/images/how-vulcan-works.svg)](/images/how-vulcan-works.svg)

## The Schema

At its core, a Vulcan schema is just a JavaScript object containing a list of fields such as `name`, `_id`, `createdAt`, `description`, etc. describing a **type of document** (a movie, a post, a photo, a review, and so on).

The schema is what defines how a [Collection](/schemas.html) (a.k.a. a Model) behaves, and it fulfills many important functions:

1. It's used to generate your GraphQL schema, which in turn controls your app's GraphQL API.
2. It's used to [control permissions](http://docs.vulcanjs.org/groups-permissions.html).
3. It's used to [generate forms](http://docs.vulcanjs.org/forms.html).

## Reading Data

Reading data basically means getting data from your database all the way to the user's browser.

Let's assume you have a `Movies.jsx` component ready to take a `results` prop and display its contents as a list of movies.

In order to pass that prop, you'll wrap your component with the [`withList` higher-order component](/resolvers.html#withList). You just need to specify the appropriate [Collection](/schemas.html), and optionally also specify a [fragment](/fragments.html) to define which document fields to load.

The `withList` HoC will then query your GraphQL endpoint's corresponding [`MoviesList` resolver](/resolvers.html#List-Resolver), which can be generated automatically using Vulcan's [default resolvers](/resolvers.html#Default-Resolvers).

The `MoviesList` resolver will optionally check the query's `terms` argument and [feed it through a set of callbacks](/terms-parameters.html) in order to generate a valid MongoDB query, whose result it will return to `withList` and from there back to your `Movies` component.

## Writing Data

Now let's consider the opposite operation: writing data, such as editing a movie's description.

First, you should know that the movie edit form can be [automatically generated from your schema](/forms.html), meaning you don't actually need to code it or worry about hooking it up to your GraphQL API.

That form is wrapped with the [`withEdit` HoC](/mutations.html#Higher-Order-Components), which in turn will call the `EditMovies` mutation on the server (which again can be [automatically generated from default mutations](/mutations.html#Default-Mutations)).

`EditMovies` will then call a [boilerplate mutator](/mutations.html#Boilerplate-Mutations) which will perform **validation** based on your schema, and finally modify the document inside your database.
