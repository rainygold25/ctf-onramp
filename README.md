# CTF On-Ramp

**Five beginner capture-the-flag challenges that run in Google Colab. No install, no server, no prior security background.**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rainygold25/wece-ctf-onramp/blob/main/onramp.ipynb)

---

## Why this exists

UIUC has one of the strongest student CTF communities in the country. That is exactly the problem: when the visible entry point is a competition run by people who do this seriously, a first attempt feels out of reach, and most people never make one.

This is the missing step before that. Five challenges, each solvable in under an hour, each with a written walkthrough explaining *why* it was breakable rather than just what the answer was. It assumes you can write a `for` loop and nothing else.

The design constraint was that it must run with zero setup. Web-exploitation and binary-exploitation challenges need hosted infrastructure, a scoreboard server, and someone on call when it falls over at 2am — none of which a first-timer should have to wait on, and none of which a student committee should have to maintain. Cryptography and encoding challenges need a Python interpreter. That is the whole dependency list, and Colab provides it in a browser tab.

## The challenges

| # | Name | Category | Pts | What it teaches |
|---|------|----------|-----|-----------------|
| 1 | Warm Up | Encoding | 50 | Base64 and hex are transport formats, not protection |
| 2 | Shifty | Classical crypto | 75 | A key space you can exhaust by hand is not a key space |
| 3 | One Byte | XOR | 100 | Brute force is a legitimate first move when the key space is small |
| 4 | Reused | Password hashing | 125 | Why unsalted MD5 fails, and why bcrypt and Argon2 are deliberately slow |
| 5 | Small Primes | RSA | 150 | RSA rests entirely on factoring being hard — the algorithm was fine, the parameter was not |

They ramp on purpose. Challenge 1 is solvable by someone who has never seen a flag before; challenge 5 has them factoring a modulus and inverting an exponent.

## How to play

Click the Colab badge above, or open `onramp.ipynb` in Colab yourself. Then:

```python
show(1)                        # print the challenge
hint(1)                        # stuck for ten minutes? no penalty
check(1, "wece{your_answer}")  # tells you if you are right, and why it worked
scoreboard([1, 2, 3])          # what you have solved so far
```

Flags look like `wece{something_here}`. Brute force counts. Searching the internet counts. Reading the hint counts. The first challenge is the only one that is genuinely hard, because it is the one where you do not yet believe you can do this.

When you are done, open `solutions.ipynb` and read every walkthrough — including for the ones you solved. The flag was never the point.

**Where to go next:** [picoCTF](https://picoctf.org) has hundreds more at this difficulty and the same shape.

## Running this as an event

For a workshop or a hackathon side-track:

1. Fork the repo and run `python generate.py` to mint a fresh set of flags. This matters — it means last year's answers do not circulate.
2. Share the Colab link. Nobody installs anything.
3. Budget 60–75 minutes. Most people finish 1–3 in the first twenty minutes and spend the rest on 4 and 5.
4. Do not release `solutions.ipynb` until the end, then walk through all five on a projector. The walkthroughs are the actual content; the challenges are just what makes people pay attention to them.

## Repository layout

```
onramp.ipynb      participant notebook - challenges, hints, flag checker
solutions.ipynb   walkthroughs, one per challenge, with the reasoning
generate.py       regenerates everything with fresh flags
challenges.json   generated artifacts + SHA-256 flag hashes (safe to commit)
flags.txt         generated plaintext flags (gitignored - never commit)
```

## Design notes

**Flags are stored as SHA-256 hashes, never plaintext.** The checker hashes your guess and compares digests, so the repository can be public without giving the answers away by reading the source.

**Everything is generated, not hand-written.** `generate.py` produces the artifacts, the hashes and both notebooks from one definition of each challenge. Fresh flags every year is one command, not an afternoon of editing strings.

**The notebook is self-contained.** Challenge data is embedded rather than fetched, so it works on conference wifi and keeps working if this repository moves.

**Every challenge carries a `teaches` field, and the checker prints it on success.** The security lesson arrives at the moment the flag is accepted, which is the one moment the solver is guaranteed to be paying attention.

## Adding a sixth challenge

Append a dict to `make_challenges()` in `generate.py` with `name`, `category`, `points`, `blurb`, `artifact`, `hint`, `teaches` and `flag`, add its walkthrough to `SOLUTION_CODE`, and re-run the generator. Both notebooks rebuild.

## Scope

These are self-contained puzzles: encodings to reverse, small key spaces to enumerate, and cryptographic parameters chosen badly on purpose. Nothing here targets real software or real systems, and nothing here is an exploit — the whole point is to show *why* weak parameters and fast hashes fail, so that the people who go on to build things choose the strong ones.

## License

MIT.
