Gmail Auto Printer Updates

📬 Gmail Auto Printer UpdatesThis repository serves as the update server for the Gmail Auto Printer application, a Python-based tool that automatically prints email attachments from a Gmail account. It hosts versioned release files to support the application's built-in auto-updater, ensuring seamless, secure, and reliable updates for Windows users.
Features

Automatic Updates: Delivers new versions of the Gmail Auto Printer application via a secure HTTPS connection.
Integrity Verification: Uses SHA256 hashes to verify update packages, preventing tampering or corruption.
Atomic Updates: Applies updates safely with rollback capabilities to maintain application stability.
Staged Updates: Handles locked files by staging updates for application restart.

Repository Structure
The repository contains the following files in the main branch:

version.txt: Specifies the current version of the application (e.g., 1.0.0).
version.txt.sha256: Contains the SHA256 hash of the update zip file for integrity verification.
main.zip: A zip archive of the application files (e.g., main.py, auto_updater.py, etc.), excluding protected files like .env, credentials.json, token.json, processed_ids.json, and update_check.json.

Setup Instructions
To configure the Gmail Auto Printer to use this update repository:

Clone or Fork the Repository:

Fork or clone this repository to your GitHub account: https://github.com/des73hirbod/gmail-auto-printer-updates


Configure the Application:

In the Gmail Auto Printer's config.py, set the following:self.ENABLE_AUTO_UPDATE = True
self.UPDATE_REPOSITORY_URL = "https://github.com/des73hirbod/gmail-auto-printer-updates"
self.UPDATE_BRANCH = "main"
self.UPDATE_CHECK_INTERVAL_HOURS = 24  # Check every 24 hours


Replace <your-username> with your GitHub username.


Prepare Update Files:

Version File: Create version.txt with the application version (e.g., 1.0.0).
Application Zip:
Zip the Gmail Auto Printer application directory, excluding protected files (.env, credentials.json, token.json, processed_ids.json, update_check.json).
Name the zip file main.zip to match the UPDATE_BRANCH configuration.
Example structure inside main.zip:gmail-auto-printer/
├── main.py
├── auto_updater.py
├── config.py
├── ...




Hash File:
Generate the SHA256 hash of main.zip using Python:import hashlib
with open('main.zip', 'rb') as f:
    print(hashlib.sha256(f.read()).hexdigest())


Save the hash to version.txt.sha256.




Upload Files:

Upload version.txt, version.txt.sha256, and main.zip to the root of the main branch in this repository.
Ensure URLs are accessible:
https://raw.githubusercontent.com/des73hirbod/gmail-auto-printer-updates/main/version.txt
https://raw.githubusercontent.com/des73hirbod/gmail-auto-printer-updates/main/version.txt.sha256
https://github.com/des73hirbod/gmail-auto-printer-updates/archive/main.zip




Run the Application:

Start the Gmail Auto Printer with python main.py.
The auto-updater will check for updates on startup and every UPDATE_CHECK_INTERVAL_HOURS hours.
Use command-line flags for testing:
--force-update: Force an immediate update check.
--no-updates: Disable auto-updates.
--debug: Enable detailed logging.





Managing Updates
To release a new version of the Gmail Auto Printer:

Update Application Files:

Modify the application code (e.g., main.py, attachment_handler.py) as needed.
Test thoroughly to ensure stability.


Increment Version:

Update version.txt with a new version number (e.g., 1.0.1).


Create Update Zip:

Zip the updated application directory into main.zip, excluding protected files.
Use this Python script to ensure correct zipping:import zipfile
from pathlib import Path

def create_update_zip(source_dir: str, output_zip: str, exclude_files: set):
    with zipfile.ZipFile(output_zip, 'w', zipfile.ZIP_DEFLATED) as zipf:
        for root, _, files in os.walk(source_dir):
            for file in files:
                if file not in exclude_files:
                    file_path = Path(root) / file
                    arcname = file_path.relative_to(source_dir)
                    zipf.write(file_path, arcname)

create_update_zip(
    'gmail-auto-printer',
    'main.zip',
    {'.env', 'credentials.json', 'token.json', 'processed_ids.json', 'update_check.json'}
)




Generate Hash:

Generate the SHA256 hash for the new main.zip:import hashlib
with open('main.zip', 'rb') as f:
    print(hashlib.sha256(f.read()).hexdigest())


Save the hash to version.txt.sha256.


Upload to GitHub:

Upload the updated version.txt, version.txt.sha256, and main.zip to the main branch.
Commit with a descriptive message (e.g., "Release v1.0.1").


Test the Update:

Run the application with python main.py --force-update --debug to verify the update process.
Check logs for messages like:
"Checking for updates..."
"Update available, starting download..."
"Update applied successfully"





Security

HTTPS: All update files are served over HTTPS for secure downloads.
SHA256 Verification: The auto-updater verifies the integrity of main.zip using version.txt.sha256.
Atomic Updates: Updates are applied atomically to prevent partial updates, with backups for rollback.
Protected Files: User-specific files (e.g., .env, credentials.json) are preserved during updates.

Troubleshooting

Update Fails:
Check logs for errors (e.g., "No write permission", "Update package hash mismatch").
Ensure the repository URLs are correct and accessible.
Verify that aiohttp, packaging, and tenacity are installed (pip install aiohttp packaging tenacity).


Files Locked:
If files are in use, the update is staged in staged_update and applied on restart.
Restart the application manually if needed.


Network Issues:
The auto-updater retries three times on network failures and skips updates if unsuccessful.


Testing:
Use --dry-run to simulate update actions without applying them.
Increment version.txt (e.g., to 1.0.1) and test with --force-update.



Contributing
Contributions are welcome! To contribute:

Fork the repository.
Create a feature branch (git checkout -b feature/YourFeature).
Commit changes (git commit -m "Add YourFeature").
Push to the branch (git push origin feature/YourFeature).
Open a pull request.

Please include tests and documentation with your changes.
Issues
Report issues or feature requests via GitHub Issues. Include:

Description of the issue
Steps to reproduce
Relevant logs (from --debug mode)

License
This repository is licensed under the MIT License.
