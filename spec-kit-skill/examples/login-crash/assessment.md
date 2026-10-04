# Bug Assessment: login-crash

## Symptom
"Submitting an empty password crashes the login form."

## Expected behavior
The form shows a validation error ("Password is required") and does not submit or crash.

## Reproduction
Not yet reproduced (no code, stack trace, or environment provided). Steps reported: open login form, leave password empty, submit.

## Evidence examined
None yet. Needed: the stack trace or console error, the login handler / form component, and the framework + version.

## Candidate causes (ranked)
1. **Unhandled null/empty value in the submit handler** (e.g. calling `.length`, hashing, or `trim()` on `undefined`). Confidence: medium. Confirm: stack trace pointing at the handler; refute: crash occurs before the handler runs.
2. **Server rejects empty password with an unhandled exception** (e.g. bcrypt/compare throws on empty input) and the client has no error path. Confidence: medium. Confirm: server logs show a 500 on empty password; refute: no request is sent.
3. **Client validation throws** (schema/validator misconfigured for empty strings). Confidence: low. Confirm: error originates in validation code.

## Assessed root cause
Undetermined. Next diagnostic step: share the error output and the login submit code so candidates 1-3 can be confirmed or ruled out.

## Scope / blast radius
Likely affects any form sharing the same validation or auth call (signup, password reset). To be checked once the cause is known.

## Proposed fix scope
Provisional: add an explicit empty-password guard with a user-facing error on the client and a 400 (not 500) response on the server. Out of scope: redesigning the auth flow.

## Verification plan
1. Re-run the original symptom: submit with empty password -> validation error, no crash, no network 500.
2. Regression: wrong password, valid login, whitespace-only password.
3. Check sibling forms (signup, reset) for the same empty-input behavior.
