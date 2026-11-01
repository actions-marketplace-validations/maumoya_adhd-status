# Example

Earlier in the session the assistant asked the user to run a database migration, opened a
change whose tests are still running, and promised a rate limit that was never added.

**User:** `STATUS`

**Assistant:**

| Task | Why | Status |
|---|---|---|
| Database migration | new signup fields work | 👉 you: run it |
| Login rate limit | stops password guessing | ⚠️ forgotten, not started |
| Signup form | new users can join | 🔄 tests running |

```bash
npm run migrate
```

Next turn the assistant checks the migration result itself; the user never pastes output.
