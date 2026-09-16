# DECISIONS.md

Fictional example decisions for Weather CLI. These are durable choices, not a status log.

## Use a 3-second HTTP timeout

- Decision: every upstream weather request times out after 3 seconds and then surfaces a readable error.
- Why: a hung network call makes the CLI feel broken, and weather data is not worth an indefinite wait.
- Background or constraints: the tool is interactive. Users would rather retry than stare at a blank terminal.
- Date confirmed: example scenario, 2026-03-01

## Reject malformed JSON instead of guessing

- Decision: if the response is not JSON, or if required fields are missing, the CLI stops and reports a decode error. It does not invent a temperature.
- Why: a partial or HTML error page is worse when rendered as if it were weather.
- Background or constraints: public APIs sometimes return error documents with a 200-looking wrapper or a truncated body.
- Date confirmed: example scenario, 2026-03-01

## Cache successful responses for 10 minutes

- Decision: cache only successful, validated payloads, keyed by normalized city name, with a 10-minute TTL.
- Why: repeated queries for the same city during development and daily use should not hit the network every time.
- Background or constraints: stale weather is acceptable for a few minutes; expired cache must be fetched again rather than served forever.
- Date confirmed: example scenario, 2026-03-02
