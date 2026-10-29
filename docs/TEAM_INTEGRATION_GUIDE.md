# RayAve: Five Member Code Packages

Source: https://github.com/lucivandenver/rayave_main
Source commit: 3b74aa7168ccf51e3b7f0da983b5a1d9b83629ba
Prepared: 29 October 2026 (Asia/Manila)

## Ano ang laman

Limang ZIP ang magkakasamang bumubuo sa source snapshot. Hindi standalone app
ang bawat part. Walang original repository file na ipapasa ng dalawang members.
Original application source code and asset bytes are preserved. Shared helpers
remain in includes/functions.php (Member 1); no refactor was made.

| Member | Branch | Part |
|---|---|---|
| 1 | `feature/member-1-accounts` | Accounts, profile, permissions, notifications, shared configuration and helpers |
| 2 | `feature/member-2-catalog` | Storefront, search, product catalog, product images and inventory |
| 3 | `feature/member-3-checkout` | Shopping cart, checkout, coupons and order confirmation |
| 4 | `feature/member-4-orders` | Order administration, tracking, cancellations and feedback |
| 5 | `feature/member-5-reports` | Admin dashboard, reports, exports, customer support and chatbot |

## Ano lang ang kokopyahin

Extract your ZIP OUTSIDE the local Git repository. Copy the CONTENTS of
TO_COPY_TO_REPO into the root of the new repository. Keep paths such as admin/,
includes/ and assets/ exactly as supplied. Do not upload the ZIP itself and do
not put TO_COPY_TO_REPO or Member-1 folders inside the project.

READ_ME_FIRST.md and FILES_TO_COPY.txt are instructions outside the project.
LOCAL_ONLY_SETUP in Member 1 is for local setup only, outside Git.

## 1. Owner: establish one common starting commit

Create ONE new GitHub repository. Add a README on GitHub before anybody starts
cloning (you may enable Add a README when creating it). The README provides a
common main history. Invite the other four members as collaborators and have
them accept. Do not independently git init five projects: each member must
clone the SAME new repository after its first commit exists.

The old repository is a source reference only. Do not push these packages to it.

## 2. Each member: clone and create their branch

Replace YOUR-NEW-REPO with the new repo name. Clone inside Laragon www or the
equivalent local PHP web directory. Example PowerShell commands:

```powershell
git clone https://github.com/lucivandenver/YOUR-NEW-REPO.git
cd YOUR-NEW-REPO
git config user.name "Your Name"
git config user.email "your-github-email@example.com"
git switch main
git pull --ff-only origin main
git switch -c feature/member-3-checkout
```

Use YOUR OWN branch from the table above, not the example Member 3 branch.
Create all five branches from the common starting main, then copy your part.
It is also safe for a later member to start from a newer main: do not delete
files already added by other members. Merge folders; keep their existing files.

## 3. Copy, inspect, commit and push

Copy only the CONTENTS of your TO_COPY_TO_REPO folder into the cloned project.
In the terminal at the project root:

```powershell
git status
git add .
git diff --cached --stat
git diff --cached
git commit -m "Import assigned RayAve checkout module from existing source"
git push -u origin feature/member-3-checkout
```

Change the commit description and branch for your own module. The attribution
is an import of an existing source snapshot, not a claim to have newly written
the imported code. Improvements made later can have their own commits.

## 4. Pull request and merge

On GitHub open Pull requests > New pull request. Base: main. Compare: your
feature branch. Describe your imported module and link to the source commit.
Review file paths and merge one PR at a time. Suggested order: Member 1, 2, 3,
4, then 5. Intermediate main versions are incomplete until ALL FIVE are merged.
Do not enable automatic deployment of an incomplete intermediate version.

Members' branches share the initial commit and add nonoverlapping paths, so
the supplied imports can merge without path conflicts. New edits to shared
files after import still need coordination and review.

## 5. Get the complete project

After all five PRs are merged, with local changes already committed:

```powershell
git switch main
git pull --ff-only origin main
composer install
Copy-Item .env.example .env
```

Use PHP 8.3 or newer, 64-bit, as required by the bundled Composer dependencies.
Composer will report any required PHP extensions. Use MySQL 8.0 for closest
compatibility with the uploaded MySQL 8.0.40 dump.

This repaired Member 1 ZIP omits the optional vendor dependency folder to
keep the download small. After all five parts have been combined, run
composer install at the project root to restore the dependencies. No Composer
installation is needed merely to push this package to feature/member-1.
The original .gitignore excludes vendor and .env from commits.

## 6. Database and local configuration

All database table definitions are in Member 1 database/schema.sql. Database
definition is included once so there is no cross-part foreign-key import order.
It has no customer records, password hashes, or API-key row values. Importing
schema.sql alone gives an EMPTY database and no login accounts or products.

For the existing populated system, use Member 1 LOCAL_ONLY_SETUP/
flowershop-original.sql: an exact copy of the uploaded flowershop (3).sql.
Create an EMPTY local database named flowershop and import that original dump
ONCE using phpMyAdmin. Do not import schema.sql and the full original dump into
the same populated database. Do not commit the original dump to a public repo;
it contains existing account and transaction data and settings fields.
Each developer imports into their OWN local database. Git push does not upload
MySQL data. For later database changes, add numbered SQL migrations and apply
them in the same order on each developer's database.

Configure your local .env, for example:

```dotenv
DB_HOST=localhost
DB_USER=root
DB_PASS=
DB_NAME=flowershop
APP_URL=http://localhost/YOUR-NEW-REPO
APP_TIMEZONE=Asia/Manila
```

Use the actual database credentials and local project URL. Configure mail and
Groq keys locally if those services are needed. Never commit the local .env.
Start Apache and MySQL in Laragon and open your actual application URL.

## Shared files and responsibilities

Member 1 owns includes/config.php, includes/functions.php, the public layout,
common CSS/JS, Composer manifests, database schema and source/file inventories.
That keeps the existing runtime intact. Members 2-5 must coordinate helper
changes with Member 1 instead of uploading a competing functions.php copy.
Member 5 owns the admin header/footer. Member 2 owns all source images,
including branding images used by other modules. Parts therefore have different
ZIP sizes; file size is not a measure of contribution difficulty.

Agree on order statuses, coupon calculations, inventory deductions/restoration
and reporting criteria before improving their connected modules. Checkout
creates orders (Member 3), subsequent order lifecycle belongs to Member 4;
inventory rules belong to Member 2, report totals to Member 5.

## Verification and limits

The included verification report checks archive integrity, original file hashes,
path uniqueness and reconstruction coverage, and rehearses merging all five
imports from one starting commit in a temporary local Git repository.
This verifies packaging and Git integration, not live PHP/MySQL behavior.
The execution environment has no PHP/MySQL runtime for an end-to-end app test.

Existing empty source placeholders are preserved: admin/analytics.php,
admin/export-sales-report.php, chatbot.php, chatbot/bot.js and chatbot/styles.css.
Use the existing implemented pages admin/reports.php, admin/export-report.php
and chatbot/rayave-assistant.php. Any pre-existing application bugs, absent
features, external API requirements or database/code mismatches remain in this
source snapshot; splitting the files does not fix them.

Final team test after all merges: login, browse, cart quantity changes, coupon,
checkout, stock deduction, admin order updates, tracking, cancellation/restock,
feedback, support messages, notifications and PDF/Excel report totals.
