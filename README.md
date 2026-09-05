# Advanced File Management System

A Windows desktop application for browsing, previewing, editing, versioning, and transferring CNC and text files. The interface is built with Tkinter and stores version metadata and activity logs in SQLite.

## Features

- Browse one or more managed root folders in a tree view
- Preview and edit `.txt`, `.nc`, `.ncp`, `.h`, and `.mpf` files
- Search files and folders by name
- Create a version snapshot whenever edits are saved
- Compare the current file with its previous version
- Lock and unlock files to control editing
- Transfer files between managed folders and a machine folder
- Record file activity in an SQLite audit log
- Detect files changed outside the application
- Preview common image formats when Pillow is installed
- Switch between operator and supervisor roles
- Activate an installation with a machine-specific license key

## Requirements

- Windows
- Python 3.10 or newer
- Tkinter (included with standard Windows Python installations)
- Pillow (declared in `requirements.txt`)

The **Open CNC Editor** button currently launches:

```text
D:\Program Files (x86)\CNC Syntax Editor\cncsyn.exe
```

Update `open_cnc_editor_folder()` in `main.py` if the editor is installed elsewhere.

## Run from source

Clone the repository and enter its directory:

```powershell
git clone https://github.com/hetvi213/File-Management.git
cd File-Management
```

Create a virtual environment and install the dependency:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Start the application:

```powershell
python main.py
```

On first launch, the application displays a machine ID and asks for its matching license key. An authorized administrator can generate that key with:

```powershell
python generate_license_key.py
```

## Usage

1. Select **Add Folder** to add a managed folder to the left file tree.
2. Select a supported file to preview it and view its details.
3. Use the role selector and available controls to edit, lock, unlock, or delete files.
4. Select **Open Machine View** to configure the machine folder.
5. Use the arrow controls to copy a selected file between the managed and machine folders.
6. Review saved revisions in **Compare** and recorded activity in **Logs**.

Folder selections persist in `root_folders.txt` and `middle_root_folders.txt`.

## Build a Windows executable

The repository includes a PyInstaller specification and Tkinter runtime hook. Install PyInstaller, then build from the project directory:

```powershell
python -m pip install pyinstaller
pyinstaller main.spec
```

The executable is written to `dist\main.exe`.

## Runtime data

The application reads or creates these files beside `main.py` (or beside the packaged executable):

| Path | Purpose |
| --- | --- |
| `file_manager.db` | SQLite version metadata and activity logs |
| `versions\` | Snapshots created before edited files are overwritten |
| `license.key` | License key for the current machine |
| `root_folders.txt` | Persisted managed-folder paths |
| `middle_root_folders.txt` | Persisted machine-folder path |

Back up the database and `versions` directory together if version history must be retained.

## Project structure

```text
main.py                    Main Tkinter application
generate_license_key.py    Administrative license-key utility
main.spec                  PyInstaller build configuration
pyinstaller_tk_runtime.py  Tkinter runtime hook for packaged builds
requirements.txt           Python dependency list
logo.jpg                   Application header image
```

## Notes

- File operations affect the selected files and folders on disk. Keep backups of production CNC files.
- The application is currently Windows-specific because it uses Windows launch behavior and a configured CNC editor path.
- Runtime files may contain machine-specific paths, history, or licensing data; review them before distributing a build or publishing repository changes.
