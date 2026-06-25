## Contributing

SAILL is an early-stage open standard. The goal right now is simple: get it into people's hands so real usage can reveal where it works, where it breaks, and what's missing. That stress-testing is more valuable than any single code contribution at this point.

This is also my first public open-source project. I've worked with Git inside organizations, but this is my first time putting something in community space and genuinely asking for input. I'm open to it, and I'll do my best to engage.

Here's where contribution is most useful right now:

---

### 1. Submit a well-crafted example loop

The single highest-value contribution: a real, tested SAILL team definition that solves a workflow you actually use.

Good examples are the fastest way for new users to understand and adopt the language. They also stress-test the primitives in ways I haven't thought of.

**How to submit one:**

1. Fork the repo
2. Add your loop to `documentation/04 - Example Loops/`
3. Follow the format in the existing examples: team name, the human prompt it encodes, the SAILL expression, and a one-line invoke example
4. Open a PR with a short description of the workflow and why you built it

The bar is: does it use current primitives correctly, does it represent a genuinely useful workflow, and is it something someone else could pick up and adapt?

---

### 2. Propose a new primitive or flag

SAILL is deliberately minimal. The primitives are meant to stay small — these definitions get read into context on every session, so token cost is a real constraint. A new flag needs to carry its weight.

The question I'll ask about any proposal: **does this fit the primitives-first, low-context-overhead philosophy, or does it add complexity without adding expressive power that can't already be covered by combining existing primitives?**

If you think you've found something that genuinely can't be expressed today and belongs in the core language, I want to hear it. Use the **[Suggest New Primitive](https://github.com/HorizonBrute/Standard_AI_Looping_Language--SAILL/issues/new?template=suggest_primitive.md)** issue template — it walks through the full submission package.

**Full how-to:** [documentation/13 - Proposing a New Primitive/proposing_primitive.md](documentation/13%20-%20Proposing%20a%20New%20Primitive/proposing_primitive.md)

The required submission package (no exceptions):
- The natural-language prompt that surfaced the need
- The proposed primitive definition (one-sentence, terse)
- A team definition that uses the primitive
- Test evidence — output from `/test-agent-teams` showing the primitive ran and behaved as described
- Before/after prompt comparison showing the compression
- A brief statement of why no existing primitive covers this

---

### What I'm not looking for right now

SAILL is intentionally a primitive layer. I'd rather keep it tight and let the community build on top of it than try to cover every case in the spec itself. If you're looking to take SAILL in a new direction — fork it, improve it, build on it. That's the point of making it open.

---

Questions, ideas, or just want to share how you're using it — open an issue. I'm learning how this works too.
