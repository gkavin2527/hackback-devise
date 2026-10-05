# Accountill: observations

Repo: github.com/panshak/accountill @ 399f2a3. `client/build/` (compiled output) was ignored.
Format: each observation has a claim, evidence (path:line), a status (Confirmed / Likely / Guess) and, where an earlier statement was wrong, a correction.

---

## OBS-1: No API route checks authentication

- **Claim:** The JWT middleware exists but is not attached to any route. Every invoice, client, profile and PDF endpoint can be called without a token.
- **Evidence:** server/routes/invoices.js:6-11, server/routes/clients.js:6-10, server/routes/profile.js:6-11, server/index.js:28-35 (no middleware passed on any registration); server/middleware/auth.js:7-34 (defined, never imported)
- **Status:** Confirmed for the registration lines. "Never imported" is an absence found by a repo-wide search, so that part is Likely.

## OBS-2: Sign-in returns the user's password hash

- **Claim:** `signin` sends the whole User document back as `result`, and the schema doesn't exclude `password`. The client then stores it in localStorage.
- **Evidence:** server/controllers/user.js:37; server/models/userModel.js:6; client/src/reducers/auth.js:6
- **Status:** Confirmed

## OBS-3: Invoices and clients link to users by a plain string, not a reference

- **Claim:** `Invoice.creator`, `Client.userId` and `Profile.userId` are `[String]` arrays with no `ref`. The client fills them with the user's `_id`, and queries match on that string.
- **Evidence:** server/models/InvoiceModel.js:15; server/models/ClientModel.js:9; server/models/ProfileModel.js:12; client/src/components/Invoice/Invoice.js:250; server/controllers/invoices.js:13
- **Status:** Confirmed

## OBS-4: The PDF download/email flow uses one shared file

- **Claim:** Every PDF request writes to the same `invoice.pdf`, and `/fetch-pdf` returns whatever was written last. Two users working at the same time can get each other's invoice.
- **Evidence:** server/index.js:57, server/index.js:88, server/index.js:98
- **Status:** Likely. The lines confirm the shared file; the cross-user effect depends on timing and wasn't run.

## OBS-5: The ER diagram wrongly says every profile, client and invoice belongs to exactly one user (earlier draft error, corrected)

- **What was previously noted (wrong):** The data-model diagram drew `USER ||--o| PROFILE`, `USER ||--o{ CLIENT` and `USER ||--o{ INVOICE`, labelled "holds User._id". In Mermaid, `||` on the USER side means each profile, client and invoice has exactly one User. The relationship table also said sign-up "writes the user's `_id`" into these fields, implying that is how every record gets its owner.
- **Corrected claim:** Nothing guarantees an owner. Google sign-in never creates a User document. It creates a Profile whose `userId` is the Google token's `jti`, a one-off token ID that matches no User. Google users' invoices and clients are saved with `user.result.googleId`, a field the decoded token doesn't have, so they are stored with no usable owner. The owner fields are optional string arrays with no `required` and no `ref`, so the database accepts records with no owner, an unknown owner, or several owners. The correct cardinality is "zero or one, not enforced" (`USER |o--o| PROFILE`, `USER |o--o{ CLIENT`, `USER |o--o{ INVOICE`), and there are orphan records.
- **Evidence:**
  - client/src/components/Login/Login.js:53-59: the Google login path; no `/users` call, so no User document.
  - client/src/components/Login/Login.js:56: profile `userId` is `result?.jti`.
  - client/src/components/Invoice/Invoice.js:250: `creator: [user?.result?._id || user?.result?.googleId]`.
  - client/src/components/Clients/AddClient.js:87 and client/src/components/Invoice/AddClient.js:76: `userId: [user?.result?.googleId]`.
  - server/models/ProfileModel.js:12, server/models/ClientModel.js:9, server/models/InvoiceModel.js:15: plain `[String]`, not required, no `ref`.
- **Status:** Confirmed for the code paths and schema lines. Likely for "the decoded Google token has no `googleId`", which depends on Google's ID-token format.
- **Why it matters:** The diagram told a reader the data is consistent, with one owner per record. Anyone who trusted it to migrate the data to SQL with foreign keys, write a "join invoices to users" report, or add a `required: true` owner field would hit orphan records from every Google user. In the app itself, Google users' invoices and clients are saved but can't be found again: the lists query by an owner ID that was never stored (server/controllers/invoices.js:13, server/controllers/clients.js:94).
- **How to rectify:**
  1. Fix the diagram to `|o--o|` and `|o--o{`, and label the arrows "string ID, not enforced; Google users store no valid ID".
  2. In the code, verify the Google token on the server, then find or create a User for that Google account and issue the app's own JWT. All records then get a real `User._id`.
  3. Change `creator` and `userId` to a single `{ type: ObjectId, ref: 'User', required: true }` and set it on the server from the verified token, never from the request body.
  4. Write a one-off migration to find orphan records (owner missing, null, or not a User `_id`) and re-link them by email where possible.

---

## Earlier draft of OBS-5, withdrawn: unique emails come from `unique: true`, not `useCreateIndex`

Withdrawn because it was a passing remark with little impact, not a central claim. The correction itself still stands, briefly: unique indexes come from `unique: true` and Mongoose's automatic index building (server/models/userModel.js:5, server/models/ProfileModel.js:5); `useCreateIndex` (server/index.js:114) only replaces the deprecated `ensureIndex()`. The details are kept below for the record.

- **What was previously noted (wrong):** "Unique `email` (userModel.js:5). Unique indexes are switched on by `useCreateIndex` (server/index.js:114)." This was stated as fact in the data-model entity table.
- **Corrected claim:** `unique: true` on the schema asks Mongoose to build a unique index when the model is first used. Mongoose does this through its `autoIndex` behaviour, which is on by default. `mongoose.set('useCreateIndex', true)` only makes Mongoose use MongoDB's `createIndex()` instead of the deprecated `ensureIndex()`. Removing that line would not remove uniqueness; it would only bring back a deprecation warning. `unique` is not a validator either: the database index enforces it, and if duplicate emails already exist the index build fails and uniqueness is not enforced at all.
- **Evidence:** server/models/userModel.js:5; server/models/ProfileModel.js:5; server/index.js:114; Mongoose maintainers on what `useCreateIndex` does: https://github.com/Automattic/mongoose/issues/10631
- **Status:** Confirmed. The repo lines show `unique: true` and the `useCreateIndex` call; the Mongoose issue confirms what the option does.
- **Why it matters:** Following the wrong claim, someone might treat `useCreateIndex` as the safeguard and miss the real risks. Index builds can be disabled or fail. Sign-up and profile creation also rely on a find-then-insert check (server/controllers/user.js:50-59, server/controllers/profile.js:56-59), so two sign-ups at the same moment would only be stopped by the index.
- **How to rectify:**
  1. Remove the wrong statement from the data-model notes.
  2. Make sure the indexes exist: call `User.syncIndexes()` and `Profile.syncIndexes()` at startup, or create them in a migration.
  3. Check the existing data for duplicate emails before building the indexes.
  4. Handle MongoDB's duplicate-key error (code 11000) in sign-up and create-profile, returning a 409 instead of a generic 500.
