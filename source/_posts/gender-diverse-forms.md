---
title: "Gender-Diverse Forms"
date: 2017-02-02
updated: 2023-08-27
tags:
    - HTML
    - LGBTQIA+
    - Forms
description: "Most forms tend to have limited options for specifying gender. This article discusses how we can avoid these issues."
---

At some point in your life, you probably had to fill out a form that asked for your gender. Most forms
tend to have limited options for specifying gender, ranging from a dropdown with 'female', 'male', and
sometimes 'other' to things like two radio buttons to pick either 'female' or 'male'.
This can be frustrating for people who don't fit into either of these categories. Even if an option for 'other' is available, this crams together many wildly different gender identities into one category.

This article discusses how we can avoid these issues.

<!-- more -->

## Offering More Options

An obvious solution would be just offering more options. Replace those radio buttons with a dropdown with all the needed options and you're good, right? Sadly, this approach also has problems: You can end up with _too_ many options which makes finding a fitting option hard, while still possibly omitting the options some of your users might want to pick. Because of this, simply adding more options is unlikely to solve the problem.

## You Aren’t Gonna Need It

Even though the concept ["You aren't gonna need it" (YAGNI)](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) focuses on programming, you can also apply it here. Often the gender selected by the user in forms is only used in very few places, such as greetings in emails sent to the user. In such cases, it is easiest to simply use a gender-neutral greeting such as "Dear \<username\>" or "Dear customer" instead.
This not only avoids the need of having to ask for the user's gender, it also reduces the complexity of the logic for constructing the text. A positive side effect of this is that regarding privacy, you need to store less personal information about your users.

## Ask for Pronouns Instead

Another solution is only asking for the user's pronouns rather than their gender. This way you can still have more 'personalized' messages without having to know the user's gender.
A dropdown with three options like 'he/him', 'she/her', and 'they/them' (possibly with examples of how these would later appear, such as 'Wish _him_ a happy birthday!') would already be enough for most use cases. If you want to improve this even more, you could include text fields allowing the user to specify [pronouns outside of these options](https://en.wikipedia.org/wiki/Neopronoun), such as 'ze/hir' and more.
A variant of this is asking the user how they want to be addressed, which allows the user to choose between formal and informal greetings which may or may not use pronouns. As an example, the choices available could include "Dear Mr. \<last_name\>" as well as a simple "Hi \<first_name\>".

## Additional Resources

- [Gender or sex – GOV.UK Design System](https://design-system.service.gov.uk/patterns/gender-or-sex/)
- [Designing forms for gender diversity and inclusion](https://uxdesign.cc/designing-forms-for-gender-diversity-and-inclusion-d8194cf1f51)
