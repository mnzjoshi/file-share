# Submission and hash verification

## 1. Check your package before you start (optional but recommended)

From inside your package folder:
- **Windows PowerShell:** `Get-FileHash -Algorithm SHA256 <file>`
- **macOS / Linux:** `shasum -a 256 <file>`

Compare the results with the lines in `PACKAGE_MANIFEST.sha256`. If any file differs or is missing, tell the coordinator before you score.

## 2. When you have finished

1. Save your form as CSV, keeping the same file name (`RATER1_CALIBRATION.csv` or `RATER2_CALIBRATION.csv`).
2. If you can, compute its SHA-256 (see above) and send the hash **together with** the file.
3. Send **only the CSV file** to the coordinator. Do not send it to the other rater.

## 3. What happens next

- The coordinator copies your file byte for byte, records its SHA-256 and **freezes** it before opening the other rater's file.
- The mapping from item IDs to sources is revealed only after **both** forms are frozen.
- If you made a mistake, send a separate note. The frozen file is never edited; corrections are recorded beside it.
