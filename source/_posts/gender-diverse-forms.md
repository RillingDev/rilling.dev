---
title: "Gender-diverse Forms"
icon: thumbs-up
date: 2017/2/2
updated: 2019/2/2
tags:
    - HTML
    - LGBTQ+
    - Forms
---

With the rise of [societies acceptance of trans\* and non-binary gender identities](https://twitter.com/NatGeo/status/809800514791677952),
It's time to think about how to handle the traditional "Male", "Female" and "Other" radio buttons of forms, both on paper and in applications and web sites.

<!-- more -->

## How it's been so far:

Most forms currently have a structure similar to this:

![Old Gender Form](form_old_1.png)

or this:

![Old Gender Form with Radio buttons](form_old_2.png)

As you already might have guessed: You can't possible include all gender labels into a list like this.

## A possible solution

So here is an idea how to solve this:
Instead of asking for an explicit gender, simply asking for the preferred pronoun is way easier - most of the time that is the only thing a gender selection input is used for anyways. While this is obviously quite a bit of a change, I feel like this would be a relatively minor change with quite an improvement on inclusivity.
Here is an example on how this could look:

![Form with pronoun-sentence](form_new_1.png)

In this case we use a sentence where we ask for the users pronouns in the subjective and possessive form,
allowing us to interact with the user using the correct pronouns.

## Names and legal names

In cases where you need the users legal name, for example when sending receipts,
including an optional text-field for the legal name is a good idea:

![Form with pronoun-sentence and optional legal name](form_new_2.png)
