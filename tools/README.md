Here are some example usages of the enhanced Project Snapshot Tool:

```bash
git bundle create artifacts/snapshots/v0.1.0.bundle --all  # Tüm tarihçeyi tek dosyada
git clone artifacts/snapshots/v0.1.0.bundle artifacts/source
```

### 1. Basic Snapshot Creation
```bash
# Create a snapshot of the current directory (default settings)
python tools\snapshot_tool.py collect artifacts/snapshots/v0.1.0.snapshot

# Create a snapshot with custom include/exclude patterns
python tools\snapshot_tool.py collect my_snapshot.txt \
    --include-dir src \
    --include-dir config \
    --include-ext .java \
    --include-ext .xml \
    --exclude-pattern target \
    --exclude-pattern *.iml
```

### 2. Snapshot with Comment Cleaning
```bash
# Create snapshot while removing comments from code files
python tools\snapshot_tool.py collect clean_snapshot.txt --clean-comments
```

### 3. Snapshot from Specific Base Directory
```bash
# Create snapshot from a different base directory
python tools\snapshot_tool.py collect ../snapshots/project_backup.txt --base-dir ~/projects/my_project
```

### 4. Restoration Examples
```bash
# Dry run (simulate restoration without writing files)
python tools\snapshot_tool.py restore project_snapshot.txt --dry-run

# Actual restoration to current directory
python tools\snapshot_tool.py restore project_snapshot.txt

# Restoration to different directory with overwrite
python tools\snapshot_tool.py restore project_snapshot.txt \
    --target-dir ~/projects/restored_project \
    --overwrite
```

### 5. Complex Example with Multiple Directories
```bash
# Snapshot specific directories with custom settings
python tools\snapshot_tool.py collect full_backup.txt \
    --include-dir src \
    --include-dir tests \
    --include-dir config \
    --include-ext .py \
    --include-ext .yaml \
    --include-ext Dockerfile \
    --exclude-pattern __pycache__ \
    --exclude-pattern *.log \
    --exclude-pattern temp \
    --base-dir ~/projects/my_project \
    --clean-comments
```

### 6. Checking What Will Be Included
```bash
# First see what would be included with current settings
python tools\snapshot_tool.py collect test_snapshot.txt --dry-run

# Then create the actual snapshot
python tools\snapshot_tool.py collect test_snapshot.txt
```

The tool will:
1. For `collect` command:
   - Show warnings for non-existent directories
   - Display total files included at the end
   - Preserve all file content exactly (or clean comments if requested)

2. For `restore` command:
   - Show detailed progress of each file being restored
   - Provide a summary of files restored/skipped
   - Handle directory creation automatically

Remember that the default settings already include most common development files while excluding build artifacts and temporary files, so you can often just use the basic command for most cases.