---
title: Noid
layout: default
nav_order: 5
---

# Noid

[Main Documentation](https://metacpan.org/dist/Noid/view/noid)

## Setting up the database on a new env:

`noid dbcreate gmgs.reeeeek long 77981 "University of Wisconsin-Milwaukee Libraries" gmgs`

`gmgs.reeeeek`

This is the template for the identifiers you're minting. Here’s how it breaks down:

`gmgs.` is a fixed prefix for our identifiers (in this case, a "shoulder" that represents a sub-namespace within the larger namespace).
    
`r` stands for a random character (can be any letter or digit).
    
`eeee` stands for exact characters (usually letters or digits).
    
`k` stands for a check digit (ensures integrity and helps prevent typographical errors).

`long`

This is the term that defines how many identifiers the system can mint and how they behave: long means a very large identifier space, typically meaning the system won’t run out of identifiers for a long time. Other options include short or medium, where short means identifiers can be reused when the space is exhausted.

`77981`

This is your NAAN (Name Assigning Authority Number), which identifies your organization in the global ARK namespace. In this case, it's the registered NAAN for the University of Wisconsin-Milwaukee Libraries.

`"University of Wisconsin-Milwaukee Libraries"`

This is the NAA or Name Assigning Authority in a human-readable format, which corresponds to the NAAN (77981). It represents the entity responsible for assigning and managing the identifiers.

`gmgs`

This is the SubNAA or shoulder, which is a namespace within your organization (in this case, gmgs). It allows for further subdivision of the identifier space. You use this shoulder to create identifiers like ark:/77981/

## URLS:

Production: https://digilib-admin.uwm.edu/noidu_gmgs

Development: https://digilib-dev.uwm.edu/noidu_gmgs

## Noid Commands:

Append the command after the URL above like this:

https://digilib-dev.uwm.edu/noidu_gmgs?mint+1

**noid mint N** [ Element Value ]

Generate N identifiers. If other arguments are specified, for each generated noid, add the given Element and bind it to the given Value. [Element-Value binding upon minting is not implemented yet.]

There is no "unmint" command. Once an identifier has been circulated in the outside world, it may be hard to withdraw because external users and systems will have bound it with their own assertions. Even within the minting organization, removing all of the identifier's supporting bindings could entail actions such as file deletion that are outside the scope of the minter. While there is no command capable of withdrawing a circulated identifier, it is nonetheless easy to queue an identifier for reminting and to hold it against the possibility of minting at all. Identifiers that are long term should be treated as non-renewable resources except when you are absolutely sure about recycling them.

**noid peppermint N** [ Element Value ]

[This command is not implemented yet.] Generate N "peppered" identifiers. A peppered identifier is a regular identifier concatenated with a "!" character and a randomly generated cookie -- the pepper -- which serves as a kind of per-identifier password. (Salt is a technical term for some extra data that makes it harder to crack encrypted values; we use pepper for some extra data that makes it harder to crack unencrypted values.) To provide an extra level of database security, the base identifier, which is everything up to the "!", should be used in all public communication, but the complete peppered identifier is required for all noid operations that would change values in the database.

As with the mint command, if other arguments are specified, for each generated noid, add the given Element and bind it to the given Value.

**noid bind How Id Element Value**

For the given Id, bind the Element to Value according to How. The Element and Value may be arbitrary strings. There are two reserved Element names allowing Values to be entered that are too large or syntactically inconvenient (depending on the calling environment's quoting restrictions) to pass in as command-line tokens.

If the Element is ":" and no Value is present, lines are read from the standard input up to a blank line; they will contain Element-colon-Value pairs in essentially email header format, with long values continued on indented lines. If the Element is ":-" and no Value is present, lines are read from the standard input up to end-of-file; the first non-comment, non-blank line must have an Element-colon to specify an Element name, and all the remaining input (up to EOF) is taken as its corresponding Value. Lines beginning with "#" are considered "comment" lines and are skipped.

The How argument specifies one of the following kinds of binding. Of these, the set, add, insert, and purge kinds "don't care" if there is no current binding.

*new*

Only if Element does not exist, create a new binding.

*replace*

Only if Element exists, undo any old bindings and create a new binding.

*set*

Means new or, failing that, replace.

*append*

Only if Element exists, place Value at the end of the old binding.

*add*

Means new or, failing that, append.

*prepend*

Only if Element exists, place Value at the beginning of the old binding.

*insert*

Means new or, failing that, prepend.

*delete*

Remove any trace of Element, returning an error if it did not exist to begin with.

*purge*

Remove any trace of Element, returning success whether or not it existed to begin with.

*mint*

Means new, but ignore the Id argument (actually, confirm that it was given as new) and mint a new Id first.

*peppermint*

[This kind of binding is not implemented yet.] Means new, but ignore the Id argument (new) and peppermint a new Id first.

**noid fetch Id** [ Element ... ]

For the noid, Id, print with labels all bindings for the given Elements. If no Element is given, find and print all bindings for the given Id. This is the verbose version of the get command, in that it prints headers and labels for everything it finds.

**noid get Id** [ Element ... ]

For the noid, Id, print without labels all bindings for the given Elements. If no Element is given, find and print all bindings for the given Id. This is the quiet version of the fetch command, in that it suppresses all headers and labels. Between each Element requested, the output will be separated by a blank line.
