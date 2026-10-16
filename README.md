# co-design

[![skills.sh](https://skills.sh/b/StefKors/co-design-skill)](https://skills.sh/StefKors/co-design-skill)

An agent skill for co-designing a product feature with Claude in four quick phases:

1. **Problem statement**: root cause via 5 Whys
2. **Insight statements** about the existing feature
3. **Process map** of the ideal customer flow
4. **Prototype directions**: clickable single-file HTML, built on your design system and iterated with Agentation feedback

Claude does the analysis and asks a few multiple-choice questions per phase, so you steer instead of writing paragraphs.

## Install

```bash
npx skills add StefKors/co-design-skill
```

## Layout

```
skills/co-design/
  SKILL.md
  references/   # per-phase guidance
  assets/       # prototype template
```

## License

MIT
