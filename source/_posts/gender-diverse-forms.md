---
title: "Gender-Diverse Forms"
date: 2017-02-02
updated: 2021-12-14
tags:
    - HTML
    - LGBTQIA+
    - Forms
---

At some point in your life you probably had to fill out a form that asked for your gender. Most forms
tend to have very limited options for specifying gender, ranging from a dropdown with 'female', 'male', and
sometimes 'other' to things like two radio buttons to pick either 'female' or 'male'. This can be quite frustrating for people which do not fit into either of these boxes and even if an option for 'other' is available, this groups together many wildly different gender identities into one box.
Other issues can appear when someone aware of these problems tries to fix them by offering options for a wider range of commonly used genders: Dropdowns can end up having _too_ many options and finding the best match gets hard, while still omitting the options some of your users might want to pick. This approach also often ends up including several genders which are usually treated as identical (why are you offering both the options 'Woman' as well as 'Trans Woman'?).

Let's look at how we can design forms to account for these issues and avoid them.

<!-- more -->

## You Aren’t Gonna Need It

Even though ["You aren't gonna need it" (YAGNI)](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) is a concept focused on programming, the same can also be applied in other cases such as designing forms. Often the gender selected by the user is only actually used in very few places, such as the greeting in mails sent to the user. In such cases, often it is easiest to simply use a gender-neutral greeting such as 'Dear \<username\>' or 'Dear customer' instead. This not only avoids the need of having to know the user's gender but also reduces the amount of logic for building the text. A positive side effect of this is that in terms of privacy, you need to store less personal information about your users.

## Ask for Pronouns Instead

Another solution to this is only asking for the user's pronouns rather than their gender. This way you can still have more 'personalized' messages without having to know the user's gender.
A dropdown with three options like 'he/him', 'she/her' and 'they/them' (With examples of how these would later appear, e.g. 'Wish `him` a happy birthday!') would probably already be sufficient for most use cases. If you want to improve this even more you could include text fields to allow the user to specify [pronouns outside of these options](https://en.wikipedia.org/wiki/Neopronoun), such as 'ze/hir' or others!
A similar approach can be used for addressing the recipient in a mail: Instead of 'Dear Mister/Miss Doe', a dropdown could allow the user to choose their preferred way of being addressed, which also might allow choosing between formal or informal versions.

## Additional Resources

-   [Designing forms for gender diversity and inclusion](https://uxdesign.cc/designing-forms-for-gender-diversity-and-inclusion-d8194cf1f51)
