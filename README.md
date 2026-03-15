# OpenClaw Jobsearch

`openclaw-jobsearch` is an OpenClaw skill for ranking live job listings from a single employer's careers portal against a job seeker's profile and stated preferences.

The skill is designed for repeatable shortlist generation. Instead of relying on ad hoc prompting, it uses a fixed workflow to:
- resolve an employer name through `EMPLOYERS.yaml`
- inspect the employer's live careers portal
- compare current roles against `USER.md` and `PREFERENCES.md`
- return a ranked top-10 shortlist with evidence and source links

## Inputs

The skill expects these files to exist at runtime:
- `EMPLOYERS.yaml`: employer keys and careers portal URLs
- `PREFERENCES.md`: the job seeker's preferences, constraints, and ranking priorities
- `USER.md`: the job seeker's background, experience, and profile
- `APPLICATIONS.yaml`: optional, used only when checking for duplicates or prior applications matters

`USER.md` is assumed to be installation-managed by OpenClaw and may not live in this repository.

## Behavior

The skill:
- takes a single employer name
- finds the matching careers portal in `EMPLOYERS.yaml`
- reads live listings from that portal
- filters roles using the preferences stated in `PREFERENCES.md`
- scores roles using evidence from the job listing, `USER.md`, and `PREFERENCES.md`
- returns the strongest matches first

The scoring method and output contract are documented in:
- [references/scoring-rubric.md](/Users/jordanmorris/projects/openclaw-jobsearch/skills/openclaw-jobsearch/references/scoring-rubric.md)
- [references/output-format.md](/Users/jordanmorris/projects/openclaw-jobsearch/skills/openclaw-jobsearch/references/output-format.md)

## Invocation

Invoke the skill as `openclaw-jobsearch`.

Example:

```text
Use openclaw-jobsearch to rank the top current jobs for OPENAI.
```

## Output

The skill returns:
- a ranked shortlist of up to 10 roles
- a fit score for each role
- concise reasons each role matches
- key risks or gaps
- source links to the underlying postings
- a short exclusions and limitations summary

## Limitations

The skill depends on access to the employer's live careers portal. Results may be incomplete when:
- listings are hidden behind heavy client-side rendering
- the portal blocks browsing or search
- work model or location details are missing from postings

The skill does not invent missing facts. If a posting does not clearly state location, compensation, visa support, or similar details, the output should say so rather than guess.
