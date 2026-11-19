# Budget Manager for Android

An original, local-first personal finance app for Android. It tracks accounts, assets, liabilities, transactions, receipts, budgets, and reports. Data is kept in app-private storage unless the user exports a file or Android system backup is enabled.

## Features

- Overview with net worth, assets, liabilities, monthly income, and spending
- Editable account sections for banks, credit cards, loans, savings, insurance, assets, liabilities, and other accounts
- Account-specific activity with add, edit, and delete ledger actions; transfers appear on both relevant accounts
- Credit-card limits and billing/payment days, plus loan EMI/interest and savings goal/interest details
- Transaction ledger with date, description, type, category, subcategory, hierarchical account labels, transfer destination, and receipt attachment
- Daily, weekly, monthly, and yearly summaries
- Monthly budget progress and a planned-versus-actual budget burndown chart
- CSV transaction import/export
- Password-encrypted local backup/restore; receipts are included
- Android system-managed cloud backup for app preferences and receipt files
- Read-only Gmail label import with manual review before creating transactions
- SMS share/paste transaction import with on-device parsing and manual review; no SMS permission required
- Custom app launcher icon and in-app branding

## Build

Open the project folder in Android Studio and let Gradle sync. It uses Android SDK 35 and JDK 17. To build from a terminal on Windows:

```powershell
.\gradlew.bat :app:assembleDebug
```

The installable debug APK is written to `app/build/outputs/apk/debug/app-debug.apk`.

## Gmail OAuth setup

Gmail authorization cannot work until the app is registered in the owner's Google Cloud project:

1. Enable the Gmail API and configure the OAuth consent screen.
2. Keep the consent screen in testing and add the Google account used on the phone as a test user.
3. Create an **Android** OAuth client for package `com.example.budgetmanager`.
4. For this laptop's debug APK, register SHA-1 `D9:3B:DD:CE:28:AE:BB:D7:B5:35:B0:D9:8F:A9:56:63:B3:67:19:66`.
5. Reinstall the APK, connect Gmail, and grant the `gmail.readonly` scope.

The SHA-1 above belongs to this machine's debug signing certificate. A release-signed APK needs an Android OAuth client registered with the release certificate's SHA-1. Gmail's read-only scope is sensitive; public distribution may require Google's OAuth verification.

The app only reads messages from the selected Gmail label. It does not mark, move, or delete messages, and it asks the user to review each detected amount.

## SMS transaction import

The SMS importer does not request SMS permissions or scan the inbox. Share a message from the SMS app to Budget Manager, or paste it into Settings → SMS import. The app parses it locally, ignores OTP/verification messages, and offers likely transactions for review and account selection. The original message text is not saved; approved transaction details and a one-way fingerprint used for duplicate detection are saved.

## Backups and data portability

- **Local backup:** creates a password-encrypted `.bmb` file containing accounts, transactions, budget, and attached receipts. The password is not stored; losing it means the backup cannot be restored.
- **Cloud backup:** Android Auto Backup is enabled for the app's preferences and receipts. It is controlled by the device's Google backup settings, is system-managed rather than a browsable Drive file, and is subject to Android backup limits (including the per-app quota).
- **CSV:** exports ledger rows; use the encrypted backup when receipt images or full account details are needed.
