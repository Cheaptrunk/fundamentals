<p align="center">
  <img src="assets/banner.png" alt="fundamentals" width="100%">
</p>

# fundamentals

A free Claude skill that tells you what you need to know before you read a text.

Give it a paper, an RFC, a contract or an article. It returns the concepts the text takes for granted, in the order you should learn them, each with a short definition and a link to the right Wikipedia article. It's a starting map for the reading, not a summary of it.

## What it does

When you read outside your field, the text itself is rarely the problem. The problem is the terms it assumes you already know, and finding out which of them you're missing usually means opening ten tabs halfway through the second paragraph.

When you invoke it, the skill:

1. Reads the whole text. If you give it a file or a URL, it opens it first.
2. Lists every concept the text assumes, and drops the ones the text explains on its own.
3. Adds what those concepts depend on, about two levels down, stopping where an educated non-specialist already is.
4. Orders the list so that each entry only uses terms defined above it, and groups it by domain when the text crosses more than one field.
5. Expands acronyms, writes a one or two line definition for each entry, and links the precise English Wikipedia article. When Wikipedia has nothing adequate, it falls back to the RFC, standard or statute itself, official documentation, or a peer-reviewed source.

A few rules it holds to:

- **Coverage first.** Missing a prerequisite is worse than including a borderline one.
- **No invented links.** If a link can't be checked, it's marked as unverified instead of guessed.
- **More than 20 entries means stop.** It tells you the count per domain and asks whether you want the full list, the ten that matter most, or one domain at a time.
- **No summary, no review.** It doesn't tell you what the text says or whether it's any good.

## Install

**Claude Code.** Put the skill in a folder named `fundamentals` inside your skills folder.

For all your projects:

```bash
git clone https://github.com/Cheaptrunk/fundamentals ~/.claude/skills/fundamentals
```

For one project only, run this from the project folder:

```bash
git clone https://github.com/Cheaptrunk/fundamentals .claude/skills/fundamentals
```

The skill is one file, so copying `SKILL.md` into `~/.claude/skills/fundamentals/` works too. On Windows, `~/.claude` is the `.claude` folder inside your user folder.

**claude.ai.** Zip `SKILL.md` and `LICENSE` in a folder named `fundamentals`, then upload it under Skills in your claude.ai settings.

## Use it

Type `/fundamentals` followed by the text, a file or a URL:

```text
/fundamentals https://www.rfc-editor.org/rfc/rfc9110#section-9.2.2
```

You can also ask in plain words, like "what do I need to know before reading this?" or "which terms should I look up first?". It answers in the language you write in and keeps technical terms in their original form, with a translation in parentheses where one exists.

## Example

You type:

```text
/fundamentals When a browser opens an HTTPS page, TLS secures the connection first:
the server sends its certificate, the browser checks it, and from then on every HTTP
request travels encrypted.
```

You get (from a real test run, trimmed):

```text
11 fundamentals, two domains: networking and cryptography.

Networking — the text describes two machines exchanging messages over a network:
nothing else in it holds without this layer.

- Network: a set of connected devices that exchange data following shared rules.
  → https://en.wikipedia.org/wiki/Computer_network
- Client-server model: one program, the client, asks for resources and another, the
  server, provides them. → https://en.wikipedia.org/wiki/Client%E2%80%93server_model
- Protocol: the rules that fix the format and order of messages between two parties.
  → https://en.wikipedia.org/wiki/Communication_protocol
- HTTP (HyperText Transfer Protocol): the protocol a browser uses to request pages from
  a web server. → https://en.wikipedia.org/wiki/HTTP

Cryptography — the "S" in HTTPS lives here.

- Encryption: turning data into a form only the holder of the right key can read.
  → https://en.wikipedia.org/wiki/Encryption
- Public-key cryptography: …
- Digital signature: …
- Digital certificate: …
- CA (Certificate Authority): …
- TLS (Transport Layer Security): …
- HTTPS (HTTP Secure): …
```

The text never mentions signatures or certificate authorities, but "the browser checks it" doesn't make sense without them, so they're on the list.

## What it needs

Web access, to fetch URLs and to check the links it isn't sure about. Without it the skill still works on pasted text and files, and marks any link it couldn't verify.

It works best on texts with a dense vocabulary: specs, papers, legal and financial documents. On an introductory text it returns the three or four entries that exist and says so, rather than padding the list.

## License

MIT. See [LICENSE](LICENSE).

Made by [Enrico M](https://cheaptrunk.dev)
