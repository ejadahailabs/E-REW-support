# Support

E-REW is built by **Ejadah AI Labs**, a startup. You will be talking to the people who
wrote the code.

## Where to go

| Need | Where |
|---|---|
| How do I…? | [User Guide](USER_GUIDE.md) |
| Run the analysis in CI | [User Guide §9.6](USER_GUIDE.md) — the CLI, the gate, a copy-paste workflow |
| Something is wrong | [Open a bug](https://github.com/ejadahailabs/E-REW-support/issues/new?template=bug_report.yml) |
| I need a capability | [Request a feature](https://github.com/ejadahailabs/E-REW-support/issues/new?template=feature_request.yml) |
| Security vulnerability | [SECURITY.md](SECURITY.md) — **never** a public issue |

## Turnaround targets

| Kind | Target |
|---|---|
| **Bug fix** | **72 hours** |
| **Complex feature or issue** | **about one week** |

These are targets we hold ourselves to, not contractual guarantees, and they mean *a
substantive response* — a fix, a workaround, or a clear statement of what we found and
when it will land. They do not mean an automated acknowledgement.

If something is blocking safety-critical work, say so in the first line of the issue. It
changes our ordering.

## What makes a report fast to fix

E-REW is an evidence tool, so **a wrong or missing finding is a bug even when nothing
crashed**. The most useful report names:

1. the finding you got, or the one you expected and did not get;
2. the requirement id and the rule id;
3. the **template** — E-REW defines no metadata schema, so your templates decide what
   can be analyzed at all.

A large share of "this check is missing" reports turn out to be an **undeclared role**.
That is deliberate: a repository that declares no `hazard` role gets no safety findings,
because a confidently wrong number on a safety dashboard is the worst output this product
could produce. The User Guide explains how to declare one.

## What we will not do

- Send your requirements anywhere. Analysis is local and deterministic; AI features are
  opt-in and clearly marked.
- Edit a file you did not open. Ever — it is an architectural rule, not a setting.
