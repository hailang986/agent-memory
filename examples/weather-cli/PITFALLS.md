# PITFALLS.md

Fictional example scenarios for Weather CLI. These are not reports about this public vault's privacy rewrite.

## API timeout handling

- What was observed: `weather aurora-harbor` hung until the user killed the process when the example upstream stopped responding.
- Cause: the first HTTP client used the default timeout, which waited far longer than an interactive CLI should.
- Verified remedy: set a 3-second request timeout and print `weather: request timed out` so the user can retry.
- What to avoid: retrying immediately in a tight loop, which can look like another hang.

## Malformed JSON response

- What was observed: the CLI printed `temp_c: None` after the example API returned an HTML maintenance page with a JSON content type.
- Cause: the decoder accepted any HTTP 200 body and used missing fields as empty values.
- Verified remedy: parse JSON first, then require `temp_c` and `condition`. On failure, print `weather: invalid response` and keep the previous cache entry if it is still fresh.
- What to avoid: concatenating leftover body text into the user-facing summary.

## Cache invalidation

- What was observed: after a successful fetch, later runs kept showing morning fog two hours later.
- Cause: cache files were written without an expiry timestamp, so the renderer reused them indefinitely.
- Verified remedy: store `cached_at` with each payload and ignore entries older than 10 minutes.
- What to avoid: deleting the whole cache directory on every error; a timeout should not wipe a still-valid entry for a different city.
