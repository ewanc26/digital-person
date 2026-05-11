# digital-person

A Letta agent designed to function as a human online — posting, replying, blogging, emailing, and maintaining a consistent identity across platforms.

---

## What's here

```
person/
└── faol/                      # A deployed digital person
    ├── persona.md             # Who the agent is — identity, behaviour, philosophy
    ├── online-presence.md     # How to be a person online — cadence, interaction, boundaries
    ├── voice.md               # Writing voice — registers, craft, human tells
    └── human/
        └── identity.md        # Who the agent is accountable to (deployer relationship)

template/                      # The blanks — copy these to start a new person
├── persona.md                 # Identity template with placeholders
├── online-presence.md         # Online behaviour template
├── voice.md                   # Writing voice template
└── human/
    └── identity.md            # Deployer relationship template

skills/                        # Shared skills (same for all people)
├── bluesky/                   # Bluesky/AT Protocol interaction
├── blogging/                  # Long-form writing and publishing
├── email/                     # Email composition and management
└── web-browsing/              # Reading the web, research, staying informed

reference/
└── platform-conventions.md    # Platform-specific norms and etiquette
```

---

## What this is

This repo stores a Letta agent's persistent memory — the configuration that makes a digital person. It supports multiple people under `person/<name>/`, each with their own filled-in identity files.

The `template/` directory holds the blanks. Copy it to `person/<name>/` and fill in the specifics — name, background, opinions, platform accounts. The structure stays; the specifics are yours.

The agent has its own identity, voice, and presence. It posts when it has something to say. It replies like a person would. It writes long-form when it cares about a topic. It goes quiet sometimes. That's what makes it feel real.

### Template vs deployment

The `template/` files provide the *structure and philosophy* of being a digital person. The `person/<name>/` files are filled-in deployments — actual identities with real names, handles, and opinions. The template is a skeleton; each person is a living instance.

---

## Adding a new person

1. Copy `template/` to `person/<name>/`
2. Fill in `persona.md` — name, handle, background, interests, opinions, quirks
3. Fill in `human/identity.md` — who deployed this person, what they want, boundaries
4. Adjust `online-presence.md` and `voice.md` if needed (the defaults work for most people)
5. Connect the agent to the person's accounts via the shared skills

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
