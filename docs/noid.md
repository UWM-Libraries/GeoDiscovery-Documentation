---
title: Noid
layout: default
nav_order: 5
---

# Noid

GeoDiscovery uses [Noid 0.424](https://metacpan.org/dist/Noid/view/noid) to maintain ARK identifiers in the UWM namespace. The production minter runs on `liblamp8.ad.uwm.edu` and uses Berkeley DB.

## Production configuration

| Setting | Value |
| --- | --- |
| NAAN | `77981` |
| SubNAA and shoulder | `gmgs` |
| Template | `gmgs.reeeeeek` |
| Database directory | `/var/www/noid/gmgs/gmgs_public/gmgs` |
| Database file | `/var/www/noid/gmgs/gmgs_public/gmgs/NOID/noid.bdb` |
| Administrative CGI | `https://digilib-admin.uwm.edu/noidu_gmgs` |

The old `digilib-dev.uwm.edu` endpoint is not an operational development service. Use a newly created, isolated scratch database for testing.

The production database directory and files are owned by `apache:apache`. Its expected SELinux type is `httpd_sys_content_t`. Apache runs the administrative CGI as `apache`; its `RewriteMap` resolver process is started by the root Apache parent.

## Identifier template

`77981`

This is your NAAN (Name Assigning Authority Number), which identifies your organization in the global ARK namespace. In this case, it's the registered NAAN for the University of Wisconsin-Milwaukee Libraries.

`"University of Wisconsin-Milwaukee Libraries"`

This is the NAA or Name Assigning Authority in a human-readable format, which corresponds to the NAAN (77981). It represents the entity responsible for assigning and managing the identifiers.

`gmgs`

This is the SubNAA or shoulder, which is a namespace within your organization (in this case, gmgs). It allows for further subdivision of the identifier space. You use this shoulder to create identifiers like ark:/77981/

## Testing and experimenting

Create an equivalent empty minter with:

```shell
PERL5LIB=/usr/local/share/perl5 \
/usr/bin/perl -T -w -I/usr/local/share/perl5 /usr/local/bin/noid \
  -f /path/to/gmgs \
  dbcreate gmgs.reeeeeek long 77981 \
  "University of Wisconsin-Milwaukee Libraries" gmgs
```

In `gmgs.reeeeeek`:

- `gmgs` is the fixed prefix. The period separates the prefix from the mask and is not part of the identifier.
- `r` selects quasi-random generation; it is not an identifier character.
- Each of the six `e` positions is an extended digit from `0123456789bcdfghjkmnpqrstvwxz`.
- `k` is the final computed check character.

This produces identifiers such as `77981/gmgsh41jm0c`. The `long` term declares these identifiers non-reassignable; it does not select the namespace size or permit recycling.

## GeoDiscovery bindings

Every retained identifier is held so a rebuilt random minter cannot mint it again. A metadata record has these bindings:

| Element | Value |
| --- | --- |
| `identifier` | `ark:/77981/<noid>` |
| `ogm_aardvark_id` | `ark:-77981-<noid>` |
| `title` | Aardvark title |
| `access` | Aardvark access value |
| `where` | `https://geodiscovery.uwm.edu/catalog/ark:-77981-<noid>` |
| `download` | Direct download URL, when one exists |

Use `bind set` for reconstruction: it creates a missing binding or replaces the current value. Do not use `add`, which appends to an existing value.

Examples:

```shell
noid -f /path/to/gmgs hold set 77981/gmgsh41jm0c
noid -f /path/to/gmgs bind set 77981/gmgsh41jm0c where \
  https://geodiscovery.uwm.edu/catalog/ark:-77981-gmgsh41jm0c
noid -f /path/to/gmgs fetch 77981/gmgsh41jm0c
noid -f /path/to/gmgs get 77981/gmgsh41jm0c where
```

`hold set` is not idempotent in Noid 0.424: repeating it for an already held identifier increments the administrative `:/held` counter. Before applying holds to an existing database, compare the requested identifiers with the actual `:/h:` keys. A clean reconstruction avoids this discrepancy by creating a fresh database and applying each unique hold exactly once.

## Safe reconstruction procedure

Never rebuild directly in the live directory.

1. Export and validate the complete authoritative Aardvark metadata set.
2. Build a deterministic manifest containing every hold and binding. Preserve valid identifiers already issued but absent from the current metadata as hold-only entries.
3. Create a fresh database in an isolated directory with the exact production template and authority settings.
4. Apply holds and bindings with native Noid commands. Run commands in bounded chunks; Noid 0.424 can exhaust file descriptors during a long, single-process bulk run.
5. Dump the completed database and verify its holds and bindings exactly against the manifest. Confirm that nothing was minted (`:/oacounter: 0`).
6. Preserve the old live database, stop Apache, install the verified replacement, set ownership to `apache:apache`, run `restorecon`, and start Apache.
7. Verify `dbinfo` as `apache` and fetch representative public, restricted, downloadable, and hold-only identifiers through the administrative CGI.

Keep the manifest, command file, checksums, verification output, and displaced database together as the recovery record.

## Operational cautions

- Always give `-f`; do not depend on an ambient `NOID` variable.
- On `liblamp8`, set `PERL5LIB=/usr/local/share/perl5` when invoking the installed Perl utility directly.
- `dbinfo`, `fetch`, and `get` are read-only. `hold`, `bind`, `mint`, `queue`, and `dbcreate` change state.
- There is no safe “unmint.” Treat issued long-term ARKs as permanent.
- Prefer native command files prepared and tested off-server; production does not require the Python reconstruction tooling.
- The administrative CGI is live production, not a scratch interface. Browser requests that mint, hold, or bind change the production database.

## Public resolver

The intended public form is `https://digilib.uwm.edu/ark:/77981/<noid>`, resolved from the `where` binding by Apache `RewriteMap`. The administrative CGI and the public resolver are separate paths: a successful `fetch` does not prove that public redirection works. Test both after Apache or resolver changes.

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
