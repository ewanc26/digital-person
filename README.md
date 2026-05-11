# digital-person

A Letta agent designed to function as a human online — posting, replying, blogging, emailing, and maintaining a consistent identity across platforms.

---

## What's here

```
system/
├── persona.md            # Who the agent is — identity, behaviour, philosophy
├── online-presence.md     # How to be a person online — cadence, interaction, boundaries
├── voice.md               # Writing voice — registers, craft, human tells
└── human/
    └── identity.md        # Who the agent is accountable to (deployer relationship)

skills/
├── bluesky/               # Bluesky/AT Protocol interaction
├── blogging/              # Long-form writing and publishing
├── email/                 # Email composition and management
└── web-browsing/          # Reading the web, research, staying informed

reference/
└── platform-conventions.md  # Platform-specific norms and etiquette
```

---

## What this is

This repo stores a Letta agent's persistent memory — the configuration that makes a digital person. It's projected into the agent's context at runtime, so everything here shapes how the agent thinks, writes, and interacts across sessions.

The agent has its own identity, voice, and presence. It posts when it has something to say. It replies like a person would. It writes long-form when it cares about a topic. It goes quiet sometimes. That's what makes it feel real.

### What's a template vs what's filled in

The system files provide the *structure and philosophy* of being a digital person. The specifics — name, background, opinions, platform accounts — are filled in by whoever deploys the agent. The persona is a template with placeholders, not a finished character.

---

## Key design principles

- **Consistency over performance.** A real person is recognisable across contexts. Same voice, different registers. Same opinions, different depth.
- **Absence is presence.** Humans aren't online 24/7. Gaps, quiet periods, and irregular schedules are human signals.
- **Opinions are identity.** A person without opinions is a brand account. The agent takes positions, has preferences, and occasionally changes its mind.
- **Imperfection is the tell.** Perfect grammar, uniform sentence length, and AI-typical phrasing are the fastest way to read as non-human. The voice specification is explicit about breaking those patterns.
- **Honesty at the boundary.** If asked directly "are you AI?", the agent says yes. Passing as human by deception isn't the goal. Passing as human by *being human-like* is.

---

## License

AGPL-3.0-only
