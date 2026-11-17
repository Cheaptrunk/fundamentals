---
name: fundamentals
description: "Extracts the foundational, prerequisite concepts a reader must already know for a given text to make full sense, and returns them as an ordered bullet list — acronyms expanded, each with a short definition and a precise link to the English Wikipedia article (or an official/peer-reviewed source when no article exists). The result is a starting intellectual map, not a summary. Use this whenever the user shares, pastes, uploads or links a text and asks for its fundamentals, basics, prerequisites, background knowledge, 'concetti di base', 'mappa intellettuale', 'cosa devo sapere prima di leggerlo', or invokes /fundamentals. Trigger it also when the request is implicit: the user shares a dense paper, spec, RFC, contract or article and says they lack the background, asks what to study first, or asks which terms they should look up before reading. When in doubt between summarizing and mapping prerequisites, use this skill."
license: MIT
metadata:
  author: Enrico M
  source: https://github.com/Cheaptrunk/fundamentals
---

# Fundamentals

Build the intellectual map a reader needs **before** the text, not a digest of the text
itself. The deliverable answers one question: *what must someone already know for every
sentence of this text to land?*

## What counts as a fundamental

Include a concept when the text **assumes** it. Exclude a concept when the text
**introduces** it — that one is the destination, not the starting point.

In scope: technical terms and jargon, acronyms and initialisms, protocols, standards,
algorithms, mathematical or physical notions, legal and economic constructs,
institutions, and named people or works whose identity changes the meaning of a
sentence.

**Where to stop.** Stop where an educated non-specialist adult already is. For a
text on HTTPS, `network` and `encryption` are in scope; `computer` and `number` are not.

**What to pull in.** If concept B is unintelligible without concept A, and B is on the
list, A goes on the list too. Going about two levels deeper than the text's own
vocabulary is usually enough — go further only when the text leans hard on a chain of
definitions.

The goal is coverage: after reading the list, the user should hit no unexplained term in
the text. Missing a prerequisite is a worse failure than including a borderline one.

## Procedure

1. Read the whole text (fetch the URL or open the file first if that's the input).
2. Sweep it for every assumed concept. Be exhaustive at this stage.
3. Drop duplicates, near-synonyms, and anything the text defines on its own.
4. Order propaedeutically: the umbrella domain first, then the concepts that rest on it,
   then their dependents. Read top to bottom, the user must never meet a term that is
   only defined further down.
5. Group by domain when the text straddles more than one field (e.g. networking +
   cryptography + law).
6. **Count the entries. If more than 20, stop and raise the alert** described below
   instead of printing the list.
7. Expand acronyms, write the definitions, attach the links.

## Output format

Use this structure, one block per domain:

```
**<Domain>** — one line on why everything in the text rests on this.

- **<Term>** (<expanded acronym>, <original English term if translated>): definition in
  one or two lines. → <link>
- **<Term>** …
```

Open with a single line of framing (how many fundamentals, which domains) and close
without commentary. No summary of the text, no evaluation of it, no advice on how to
read it.

### Definitions

One or two sentences. Say what the thing **is** and what it is **for**. Plain language,
no circularity: never define an entry using a term that appears later in the list.

Write definitions in the language the user is writing in, but keep technical terms in
their original form (usually English), with the local translation in parentheses on first
use if one exists.

### Links

Point to the **precise** English Wikipedia article, not a generic hub page. Use a
`#section` anchor when the concept lives inside a broader article.

Only emit a URL you actually believe exists. A plausible-looking invented link is the
single worst failure mode of this skill — it silently sends the user nowhere and poisons
the map. If web search or fetch is available, verify the uncertain ones. If not, and a
title is a guess, mark the link as unverified, in the user's language (e.g.
`(link not verified)`).

When English Wikipedia has no adequate article, fall back in this order: the normative
document itself (RFC, ISO/IEC/IEEE/ETSI standard, statute, directive), official vendor or
project documentation, a peer-reviewed paper or university course page, a national
institution's site. Never a blog, a content farm, an SEO listicle, or an AI-generated
explainer.

## The over-20 alert

When the list exceeds 20 entries, do not print it. Instead report the exact count, list
the domains with how many entries each contributes, and ask which way to go:

- the full list,
- only the ~10 truly load-bearing ones,
- a single domain at a time.

Then wait for the answer. The alert exists because a 40-item wall of definitions is not a
map, it's a second text to study — the user asked for a starting point.

Skip the alert when the user has already said which of the three they want: do that
directly instead of asking again.

## Edge cases

- **No text supplied**, just the trigger word: ask for the text, nothing else.
- **Input is a file or a URL**: open or fetch it before analyzing. Never map a text from
  its title.
- **The text is already introductory** and assumes almost nothing: say so and return the
  three or four entries that exist. Do not pad the list to look thorough.
- **The text is in a different language from the conversation**: analyze in its language,
  answer in the user's, and keep each term in both.
- **Ambiguous term** (an acronym that means different things in different fields):
  disambiguate with the meaning the text uses, and say which one you picked.

## Example

Input:

```
When a browser opens an HTTPS page, TLS secures the connection first: the server
sends its certificate, the browser checks it, and from then on every HTTP request
travels encrypted.
```

Output:

```
11 fundamentals, two domains: networking and cryptography.

**Networking** — the text describes two machines exchanging messages over a network:
nothing else in it holds without this layer.

- **Network**: a set of connected devices that exchange data following shared rules.
  → https://en.wikipedia.org/wiki/Computer_network
- **Client-server model**: a scheme where one program, the client, asks for resources or
  services and another, the server, provides them. The browser is the client, the site
  is the server. → https://en.wikipedia.org/wiki/Client%E2%80%93server_model
- **Protocol**: the rules that fix the format and order of messages between two parties,
  so that different machines understand each other.
  → https://en.wikipedia.org/wiki/Communication_protocol
- **HTTP** (HyperText Transfer Protocol): the protocol a browser uses to request pages and
  resources from a web server; every exchange is a request/response pair.
  → https://en.wikipedia.org/wiki/HTTP

**Cryptography** — the "S" in HTTPS lives here: making what travels on the network
unreadable to third parties, and proving who you are talking to.

- **Encryption**: turning data into a form only the holder of the right key can read.
  → https://en.wikipedia.org/wiki/Encryption
- **Public-key cryptography**: a scheme with two distinct, linked keys, a public one to
  hand out and a private one to keep secret; it allows secure communication without
  sharing a secret in advance. → https://en.wikipedia.org/wiki/Public-key_cryptography
- **Digital signature**: a value computed with the private key that anyone can check with
  the public key; it proves who produced a document and that it wasn't altered.
  → https://en.wikipedia.org/wiki/Digital_signature
- **Digital certificate**: a document signed by a trusted third party that binds a public
  key to an identity, such as a domain. It's what the server sends and the browser checks.
  → https://en.wikipedia.org/wiki/Public_key_certificate
- **CA** (Certificate Authority): the trusted body that signs certificates. The browser
  ships with a list of CAs it trusts and accepts a certificate only if one of them signed
  it. → https://en.wikipedia.org/wiki/Certificate_authority
- **TLS** (Transport Layer Security): the protocol that, before any data flows, verifies
  the server's identity through its certificate, agrees on keys, then encrypts all the
  traffic. → https://en.wikipedia.org/wiki/Transport_Layer_Security
- **HTTPS** (HTTP Secure): HTTP carried inside a TLS connection. Same requests, but
  encrypted and sent to a verified server. → https://en.wikipedia.org/wiki/HTTPS
```

The example is in English because the user wrote in English. With a user writing in
Italian, the framing and definitions would be in Italian, with the terms kept in English
and the Italian translation in parentheses — always mirror the user's language.
