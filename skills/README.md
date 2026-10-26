# skills/

Agent-agnostic **procedural memory**. Each skill is a folder with a `SKILL.md`
describing when to use it and the procedure to follow — portable across runtimes
(Hermes, OpenClaw). Listed in `../06-skills-catalog.md`.

Ships with two worked-example skills: `daily-review/` (a stand-up over the
project registry) and `distill-skill/` (the skill factory — turn a successful
task into a new skill). Add your own the same way and list them in the catalog.

## Convention

```
skills/
  <skill-name>/
    SKILL.md      # name, when-to-use, procedure — no runtime-specific calls
```
