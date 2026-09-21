# Telep IO — Muse Connectors

Open-source [Muse](https://muse.ai) connectors built by [Telep IO](https://telep.io).
Each connector lets an AI agent prepare a real-world action; a human reviews the
exact details and pays before anything irreversible happens.

## How the model works

1. **Agent prepares** — the agent drafts the action (a letter, a fax, a call…) and
   returns a review link. The agent can never complete the action itself.
2. **Human reviews & pays** — you see the exact content, destination, and price,
   then confirm and pay (Stripe) in one step.
3. **Provider performs** — a third-party fulfillment provider executes the
   confirmed action. Status is tracked honestly: `sent` means the provider
   accepted it, not that it arrived.

Every connector repo is public and secret-free — read the code before you trust it.

## The connectors

| Connector | What it does | Price | Status |
|---|---|---|---|
| [paper-send](https://github.com/Telep-IO/paper-send) | PDF → printed & mailed physical letter (US) | $4.99 + $0.25/page | In review — launching after provider authorization |
| [sign-send](https://github.com/Telep-IO/sign-send) | E-signature envelopes: upload PDF, collect signatures | $2.99 / envelope | Scaffold — provider integration pending |
| [fax-send](https://github.com/Telep-IO/fax-send) | Send faxes from a PDF, with optional cover page | $0.99 / page | Scaffold — provider integration pending |
| [call-send](https://github.com/Telep-IO/call-send) | Agent-drafted phone calls with a human-approved verbatim script | $0.99 / call | Scaffold — provider integration pending |
| [ink-send](https://github.com/Telep-IO/ink-send) | Robot-handwritten letters & cards, mailed for you | $3.99 / letter | Scaffold — provider integration pending |
| [domain-send](https://github.com/Telep-IO/domain-send) | Register a domain name (WHOIS privacy included) | $14.99 / yr (.com) | Scaffold — provider integration pending |

## Trust & safety design (shared by all)

- Agents may create **drafts** and read **status** only.
- The review page shows the exact content, destination, options, and price.
- Human confirmation + payment gate the irreversible provider action.
- Approval cryptographically binds content hash, destination, price, and policy versions.
- Provider calls use idempotency keys; failures queue refunds.
- Statuses are honest: drafts, checkouts, and queue entries are never described as done.

## Repo layout (each connector)

- `src/` — API server (Node.js)
- `public/` — human review page
- `docs/connector.md` — the agent-facing contract (this is what a Muse connector listing points at)
- `policies/` — terms & privacy served from the review flow
- `TERMS-DILIGENCE.md` — provider terms status: what is verified, what is not

## Status honesty

Anything marked **Scaffold** runs end-to-end in demo mode but its real
fulfillment-provider integration is stubbed. Nothing here sends a real letter,
fax, call, or registration until provider terms are confirmed in writing and the
integration is wired. The `TERMS-DILIGENCE.md` file in each repo says exactly
where things stand.
