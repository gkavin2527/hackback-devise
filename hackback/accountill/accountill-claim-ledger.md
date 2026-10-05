# Accountill: verified claim ledger

Repo: github.com/panshak/accountill, commit 399f2a3 (9 Apr 2023). `client/build/` is compiled output and was not used as evidence for anything.
Every line below was re-read on the cloned repo before tagging.

Tags:

* **Confirmed** means the cited line(s) prove the claim.
* **Likely** means strong signs but no single line proves it. This includes absence claims ("X is never used"), which rest on a repo-wide search rather than one line.
* **Guess** means no direct evidence.

## A. Tech stack and running locally

- The project describes itself as a MERN app.
  Evidence: README.md:2 [Confirmed]
- The server uses ES modules.
  Evidence: server/package.json:7 [Confirmed]
- The server's dependencies are express ^4.17.1, mongoose ^5.12.10, jsonwebtoken ^8.5.1, bcryptjs ^2.4.3, nodemailer ^6.6.3, html-pdf ^3.0.1, dotenv ^8.5.0, cors ^2.8.5 and moment ^2.29.1.
  Evidence: server/package.json:15-23 [Confirmed]
- nodemon ^2.0.7 is a dev dependency.
  Evidence: server/package.json:26 [Confirmed]
- The client uses react and react-dom ^17.0.2 and react-scripts 4.0.3.
  Evidence: client/package.json:25, :27, :34 [Confirmed]
- The client uses redux ^4.1.0, react-redux ^7.2.4, redux-thunk ^2.3.0 and react-router-dom ^5.2.0.
  Evidence: client/package.json:38, :32, :39, :33 [Confirmed]
- The client uses Material UI v4: core ^4.11.4, icons ^4.11.2, lab ^4.0.0-alpha.58 and pickers ^3.3.10.
  Evidence: client/package.json:10-13 [Confirmed]
- The client also uses axios ^0.21.1, @react-oauth/google ^0.9.0, apexcharts ^3.28.1, react-apexcharts ^1.3.9, jwt-decode ^3.1.2, file-saver ^2.0.5 and react-dropzone ^11.3.4.
  Evidence: client/package.json:19, :14, :18, :26, :22, :21, :28 [Confirmed]
- The lockfile pins redux at 4.1.1.
  Evidence: client/package-lock.json:12709 [Confirmed]
- The lockfile pins @material-ui/core at 4.12.3.
  Evidence: client/package-lock.json:1797 [Confirmed]
- The database is MongoDB, connected through mongoose.
  Evidence: server/index.js:109 [Confirmed]
- The README names MongoDB Atlas as the database.
  Evidence: README.md:68 [Confirmed]
- Docker Compose uses the `mongo` image.
  Evidence: docker-compose.prod.yml:29 [Confirmed]
- Both Docker images start from Node 14.
  Evidence: client/Dockerfile:1, server/Dockerfile:1 [Confirmed]
- The client is served by nginx 1.21.0.
  Evidence: client/Dockerfile:15 [Confirmed]
- Logo uploads go straight from the browser to a Cloudinary account named "almpo".
  Evidence: client/src/components/Settings/Form/Uploader.js:33 [Confirmed]
- The server starts with `node index.js`.
  Evidence: server/package.json:9 [Confirmed]
- The client starts with `react-scripts start`.
  Evidence: client/package.json:44 [Confirmed]
- The README's local run steps are `npm install` then `npm start` in each of the client and server folders.
  Evidence: README.md:95-99, README.md:117-121 [Confirmed]
- The README's Docker commands are `build` then `up`.
  Evidence: README.md:159-163 [Confirmed]
- The README's PDF troubleshooting uses global install and `npm link`.
  Evidence: README.md:127-131 [Confirmed]
- The server's port defaults to 5000.
  Evidence: server/index.js:107 [Confirmed]
- The server reads DB_URL and PORT.
  Evidence: server/index.js:106-107 [Confirmed]
- The server reads SECRET.
  Evidence: server/middleware/auth.js:5, server/controllers/user.js:8 [Confirmed]
- The server reads SMTP_HOST, SMTP_PORT, SMTP_USER and SMTP_PASS.
  Evidence: server/index.js:39-43, server/controllers/user.js:9-12 [Confirmed]
- The client reads REACT_APP_GOOGLE_CLIENT_ID.
  Evidence: client/src/components/Login/Login.js:110 [Confirmed]
- The client reads REACT_APP_API.
  Evidence: client/src/api/index.js:4 [Confirmed]
- The client reads REACT_APP_URL.
  Evidence: client/src/components/InvoiceDetails/InvoiceDetails.js:170 [Confirmed]
- The README lists the client's env names.
  Evidence: README.md:81-83 [Confirmed]
- The README lists the server's env names.
  Evidence: README.md:105-111 [Confirmed]

## B. Folder map and key files

- The .github folder holds only funding links (Buy Me a Coffee).
  Evidence: .github/FUNDING.yml:13 [Confirmed]
- server/index.js mounts the four routers.
  Evidence: server/index.js:32-35 [Confirmed]
- server/index.js defines /send-pdf, /create-pdf and /fetch-pdf.
  Evidence: server/index.js:53, :87, :97 [Confirmed]
- The server connects to MongoDB before it starts listening.
  Evidence: server/index.js:109-111 [Confirmed]
- The password-reset email links to the live accountill.com site.
  Evidence: server/controllers/user.js:122 [Confirmed]
- The invoice count endpoint is used for invoice numbering.
  Evidence: server/routes/invoices.js:6 [Confirmed]
- The PDF template comes from documents/index.js.
  Evidence: server/index.js:21 [Confirmed]
- The auth middleware treats tokens under 500 characters as the app's own.
  Evidence: server/middleware/auth.js:10 [Confirmed]
- The auth middleware decodes longer (Google) tokens without verifying them.
  Evidence: server/middleware/auth.js:22 [Confirmed]
- All client routes are in App.js.
  Evidence: client/src/App.js:31-44 [Confirmed]
- The Redux store is created with thunk, and the app is mounted next to it.
  Evidence: client/src/index.js:14-21 [Confirmed]
- The axios interceptor adds the Bearer token from localStorage.
  Evidence: client/src/api/index.js:6-11 [Confirmed]
- api/index.js defines every client API call.
  Evidence: client/src/api/index.js:15-40 [Confirmed] (corrected from :14-40)
- Invoice.js imports currencies.json.
  Evidence: client/src/components/Invoice/Invoice.js:32 [Confirmed]
- Invoice.js calls the count endpoint.
  Evidence: client/src/components/Invoice/Invoice.js:93 [Confirmed]

## C. Odd files

- No route file imports the auth middleware.
  Evidence: server/routes/invoices.js:1-11 (whole file read; same for clients.js, profile.js and userRoutes.js) [Likely: absence claim]
- invoice.pdf is overwritten on every PDF request.
  Evidence: server/index.js:57, :88 [Confirmed]
- Because invoice.pdf is one shared file, concurrent users can receive each other's PDFs.
  Evidence: server/index.js:57, :88, :98 [Likely]
- documents/invoice.js is imported only in a comment.
  Evidence: server/index.js:22 [Confirmed]
- The Procfile is a Heroku leftover.
  Evidence: server/Procfile:1 [Likely]
- client/build/ is committed (19 files).
  Evidence: `git ls-files client/build` (no single line) [Likely]
- The client's .gitignore doesn't ignore build/.
  Evidence: client/.gitignore:1-2 [Confirmed]
- The client Dockerfile copies a yarn.lock that doesn't exist.
  Evidence: client/Dockerfile:7 [Confirmed]
- The server Dockerfile runs a missing `start-prod` script.
  Evidence: server/Dockerfile:14, server/package.json:8-10 [Confirmed]
- Docker Compose sets no build context.
  Evidence: docker-compose.prod.yml:6-7, :20-21 [Confirmed]
- Without a build context, the Docker build paths would fail.
  Evidence: none [Guess]
- store.js is an unrelated inventory script (GPS units, nets, lenders).
  Evidence: client/src/store.js:2-35 [Confirmed] (corrected from :1-29)
- Nothing imports store.js.
  Evidence: none (repo search) [Likely]
- clients.json is imported only in a comment.
  Evidence: client/src/components/Clients/Clients.js:28 [Confirmed]
- Donut.js is imported only in a comment.
  Evidence: client/src/components/Dashboard/Dashboard.js:9 [Confirmed]
- ReactChart.js, SelectType.js, AddPayment.js, Login/Google.js, Login/Icon.js, components/Icons.js and Settings/Form/icon.js are never imported.
  Evidence: none (import scan) [Likely]
- recharts, uuid, lodash, react-nice-dates and react-multiple-select-dropdown-lite are declared but not imported.
  Evidence: client/package.json:37, :40, :23, :30, :29 [Likely: declaration confirmed, non-use is an absence]
- @babel/parser and css-loader are unusual devDependencies for a Create React App project.
  Evidence: client/package.json:68-69 [Guess, as originally stated]
- The README names react-google-login, but the code uses @react-oauth/google.
  Evidence: README.md:56, client/src/components/Login/Login.js:5 [Confirmed]

## D. API table

All 24 rows: the route registration lines below are Confirmed. "No auth check" on each is Likely, because it's an absence; no middleware is passed on any registration line.

- POST /users/signin, /signup, /forgot and /reset map to signin, signup, forgotPassword and resetPassword.
  Evidence: server/routes/userRoutes.js:6-9 [Confirmed]
- Sign-in returns the full User document including the password hash, because the password field isn't excluded from queries.
  Evidence: server/controllers/user.js:37, server/models/userModel.js:6 [Confirmed]
- Sign-up returns the created user, including the hash.
  Evidence: server/controllers/user.js:63 [Confirmed]
- Sign-up issues a 1-hour JWT.
  Evidence: server/controllers/user.js:61 [Confirmed]
- Sign-in issues a 1-hour JWT.
  Evidence: server/controllers/user.js:34 [Confirmed]
- Forgot-password returns 422 when the user doesn't exist.
  Evidence: server/controllers/user.js:111 [Confirmed]
- Reset requires an unexpired token.
  Evidence: server/controllers/user.js:140 [Confirmed]
- The invoice routes are count, get, list, create, patch and delete.
  Evidence: server/routes/invoices.js:6-11 [Confirmed]
- Invoices are listed and counted by the `creator` value from the query string.
  Evidence: server/controllers/invoices.js:13, :27 [Confirmed]
- Creating an invoice saves the request body as-is.
  Evidence: server/controllers/invoices.js:57 [Confirmed]
- Updating an invoice overwrites it with the request body.
  Evidence: server/controllers/invoices.js:87 [Confirmed]
- The client routes are list-all, by-user, create, patch and delete.
  Evidence: server/routes/clients.js:6-10 [Confirmed]
- GET /clients pages through every user's clients.
  Evidence: server/controllers/clients.js:44-45 [Confirmed]
- There is no GET /clients/:id route.
  Evidence: server/routes/clients.js:6-10 [Confirmed]
- The profile routes are get-by-id, by-user, create, patch and delete.
  Evidence: server/routes/profile.js:6, :8-11 [Confirmed]
- The getProfiles route is commented out.
  Evidence: server/routes/profile.js:7 [Confirmed]
- Creating a profile with an existing email returns 404 "Profile already exist".
  Evidence: server/controllers/profile.js:58 [Confirmed]
- /send-pdf emails whatever address is in the request body.
  Evidence: server/index.js:62 [Confirmed]
- /send-pdf sends from hello@accountill.com.
  Evidence: server/index.js:61 [Confirmed]
- /fetch-pdf returns the shared invoice.pdf.
  Evidence: server/index.js:98 [Confirmed]
- The client calls GET /clients/:id, which has no server route.
  Evidence: client/src/api/index.js:21 [Confirmed] (corrected from :14)
- The client calls GET /profiles/search, which would match /profiles/:id.
  Evidence: client/src/api/index.js:34 [Confirmed] (corrected from :27)
- That /profiles/search request would fail with a 404.
  Evidence: server/controllers/profile.js:22, :26 [Likely]
- getClient, getInvoices and getProfilesBySearch are defined but never routed.
  Evidence: server/controllers/clients.js:24, invoices.js:36, profile.js:85 [Likely: definitions confirmed, non-use is an absence]

## E. Screens table

- App.js defines 11 routes plus a redirect from /new-invoice.
  Evidence: client/src/App.js:32-43 [Confirmed]
- Login: sign-in dispatch.
  Evidence: client/src/components/Login/Login.js:43 [Confirmed]
- Login: sign-up dispatch.
  Evidence: client/src/components/Login/Login.js:41 [Confirmed]
- Sign-up creates the profile with a second client-side call.
  Evidence: client/src/actions/auth.js:28-30 [Confirmed]
- Google login only creates a profile and never calls /users.
  Evidence: client/src/components/Login/Login.js:53-59 [Confirmed]
- The Google profile's userId is the token's `jti`.
  Evidence: client/src/components/Login/Login.js:56 [Confirmed]
- Dashboard loads invoices by user.
  Evidence: client/src/components/Dashboard/Dashboard.js:60 [Confirmed]
- The invoice form calls count, getInvoice, getClientsByUser, update and create.
  Evidence: client/src/components/Invoice/Invoice.js:93, :106, :111, :218, :233 [Confirmed]
- getInvoice also fetches the profile.
  Evidence: client/src/actions/invoiceActions.js:32-33 [Confirmed]
- Adding a client from the invoice form.
  Evidence: client/src/components/Invoice/AddClient.js:83 [Confirmed]
- Invoice details: getInvoice, create-pdf, fetch-pdf and send-pdf.
  Evidence: client/src/components/InvoiceDetails/InvoiceDetails.js:87, :121, :140, :153 [Confirmed]
- Recording a payment patches the invoice.
  Evidence: client/src/components/Payments/Modal.js:127 [Confirmed]
- The invoices list loads and deletes invoices.
  Evidence: client/src/components/Invoices/Invoices.js:134, :232 [Confirmed]
- The customers page lists clients.
  Evidence: client/src/components/Clients/ClientList.js:36 [Confirmed]
- The customers page adds and edits clients.
  Evidence: client/src/components/Clients/AddClient.js:96, :98 [Confirmed]
- The customers page deletes clients.
  Evidence: client/src/components/Clients/Clients.js:179 [Confirmed]
- Settings loads and updates the profile.
  Evidence: client/src/components/Settings/Form/Form.js:49, :57 [Confirmed]
- The Forgot page posts to /users/forgot.
  Evidence: client/src/components/Password/Forgot.js:21 [Confirmed]
- The Reset page posts to /users/reset.
  Evidence: client/src/components/Password/Reset.js:20 [Confirmed]
- The floating button shows only when logged in.
  Evidence: client/src/components/Footer/Footer.js:19-21 [Confirmed]
- The floating button opens the Add Client dialog.
  Evidence: client/src/components/Fab/Fab.js:22 [Confirmed]
- Logout only clears localStorage.
  Evidence: client/src/components/Header/Header.js:53-56, client/src/reducers/auth.js:11 [Confirmed]

## F. Counts

- There are 20 router handlers: clients 5, invoices 6, profile 5, users 4.
  Evidence: grep counts on server/routes/*.js [Confirmed by tool output; not a single line]
- server/index.js registers 4 more handlers.
  Evidence: server/index.js:53, :87, :97, :102 [Confirmed]
- The frontend has 12 route entries.
  Evidence: client/src/App.js:32-43 [Confirmed]

## G. Data model and diagram arrows

- Schema fields per model.
  Evidence: server/models/userModel.js:3-9, ProfileModel.js:3-13, ClientModel.js:4-14, InvoiceModel.js:3-23 [Confirmed]
- No model uses ObjectId or `ref`.
  Evidence: the same four files, read in full [Confirmed]
- Arrow USER → PROFILE: Profile.userId is a string array.
  Evidence: server/models/ProfileModel.js:12 [Confirmed]
- USER → PROFILE: sign-up stores the user's `_id` there.
  Evidence: client/src/actions/auth.js:30 [Confirmed]
- USER → PROFILE: it's queried by sign-in and by-user lookups.
  Evidence: server/controllers/user.js:25, profile.js:75 [Confirmed]
- The cardinality "zero or one" for USER → PROFILE.
  Evidence: profile.js:75 uses findOne, but nothing enforces one profile per user [Likely]
- Arrow USER → CLIENT: Client.userId is a string array.
  Evidence: server/models/ClientModel.js:9 [Confirmed]
- USER → CLIENT: it's set from the user's `_id`.
  Evidence: client/src/components/Clients/AddClient.js:83-85 [Confirmed]
- USER → CLIENT: it's queried in clients.js.
  Evidence: server/controllers/clients.js:94 [Confirmed]
- Arrow USER → INVOICE: Invoice.creator is a string array.
  Evidence: server/models/InvoiceModel.js:15 [Confirmed]
- USER → INVOICE: it's set from the user ID.
  Evidence: client/src/components/Invoice/Invoice.js:250 [Confirmed]
- USER → INVOICE: it's queried in invoices.js.
  Evidence: server/controllers/invoices.js:13 [Confirmed]
- Arrow INVOICE → INVOICE_ITEM is embedded.
  Evidence: server/models/InvoiceModel.js:6 [Confirmed]
- Arrow INVOICE → INVOICE_CLIENT is an embedded copy, not a reference.
  Evidence: server/models/InvoiceModel.js:17 [Confirmed]
- Arrow INVOICE → PAYMENT_RECORD is embedded.
  Evidence: server/models/InvoiceModel.js:18 [Confirmed]
- Payments are appended client-side and saved by PATCH.
  Evidence: client/src/components/Payments/Modal.js:116-120 [Confirmed]
- The PDF reads the invoice's embedded client fields.
  Evidence: client/src/components/InvoiceDetails/InvoiceDetails.js:122-125 [Confirmed] (corrected from :123-126)
- The decoded Google token usually has no `googleId`, so Google users' ID is undefined.
  Evidence: client/src/components/Login/Login.js:54, Invoice.js:250 [Likely: depends on Google's token format, not on repo code]
- User.email is unique.
  Evidence: server/models/userModel.js:5 [Confirmed]
- Profile.email is unique.
  Evidence: server/models/ProfileModel.js:5 [Confirmed]
- useCreateIndex is enabled.
  Evidence: server/index.js:114 [Confirmed]
- CORRECTED: The data-model section earlier said "Unique indexes are switched on by useCreateIndex". That is wrong. Unique indexes come from `unique: true` plus Mongoose's default autoIndex. useCreateIndex only swaps the deprecated ensureIndex() for createIndex().
  Evidence: server/models/userModel.js:5, server/models/ProfileModel.js:5, server/index.js:114; https://github.com/Automattic/mongoose/issues/10631 [Confirmed]
- No other indexes exist.
  Evidence: the four model files [Confirmed]
- `bio` is dropped because it isn't in the schema.
  Evidence: server/controllers/user.js:59, userModel.js:3-9 [Confirmed]
- Profile `createdAt` is dropped because it isn't in the schema.
  Evidence: server/controllers/profile.js:52, ProfileModel.js:3-13 [Confirmed]
- paymentDetails isn't saved on profile creation.
  Evidence: server/controllers/profile.js:31-53 [Confirmed]
- The createdAt default is a fixed value computed when the server starts.
  Evidence: server/models/InvoiceModel.js:21, ClientModel.js:12 [Confirmed for the line; the effect "every invoice gets the boot time" is Likely]
- Money fields are strings.
  Evidence: server/models/InvoiceModel.js:6-7 [Confirmed]
- ClientModel.js imports express without using it.
  Evidence: server/models/ClientModel.js:1 [Confirmed]
- There's a dead seed block in the profile controller.
  Evidence: server/controllers/profile.js:125-134 [Confirmed]
- Two action constants have the same value.
  Evidence: client/src/actions/constants.js:14, :29 [Confirmed]

## H. Review gaps (see the table in chat)

Each gap's evidence lines were printed and checked. Tags are given per gap in the table's numbering:

* Gaps 1-6, 9-14, 16-18 and 20-24: Confirmed.
* Gap 7 (Google users have no stable ID): Likely.
* Gap 8 (shared PDF file race): Likely.
* Gap 15 (requests hang on unhandled async errors): Likely.
* Gap 19 (no rate limiting): Likely, since it's an absence.
* Gap 25 (no tests): Likely, since it's an absence.
