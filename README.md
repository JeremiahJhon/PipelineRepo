# Pipeline Testing and Repository Notes

## Pipeline Testing

### How the Pipelines Were Tested

The pipelines were tested using a local installation of Jenkins on Windows 11.

### Validation Performed

The following outputs were verified after successful pipeline execution:

- `doc.zip`
  - Generated documentation archive from the Doxygen stage
- `output.csv`
  - Parsed warnings generated from TaskC

### Additional Testing

The pipeline execution was validated by:

- Running the complete pipeline end-to-end
- Verifying artifact generation
- Checking Jenkins console logs for execution status and errors

---

# RepoC Python Parser Testing

## Testing Approach

The Python parser was tested independently before integrating it into the Jenkins pipeline.

### Test Files Created

Sample `warnings.log` files were prepared containing:

- Valid Doxygen warning lines
- Invalid or non-standard lines (to verify they are ignored)
- Edge cases:
  - Empty files
  - Malformed entries

### Local Execution

The parser was executed locally using:

```bash
python parser.py warnings.log
```

### Verification Performed

The following were confirmed:

- CSV output is correctly generated
- Only valid warning lines are parsed
- Invalid entries are ignored
- CSV columns are accurate:
  - `Line`
  - `File`
  - `Message`

---

# Git LFS for Binary Files

## Why Use Git LFS?

RepoA-doc contains binary files. Using Git Large File Storage (LFS) provides several advantages:

- Reduces Git repository size
- Improves repository performance
- Prevents repository bloat from large binary files
- Improves clone and fetch operations

Git LFS stores lightweight pointer files in Git while keeping the actual binary files in separate storage.

## Official Documentation

- Git LFS Official Site:
  https://git-lfs.com/

- GitHub Documentation:
  https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage

---

# Configuring the Repository to Support Git LFS

## Install Git LFS

```bash
git lfs install
```

## Track Binary File Types

Example for ZIP files:

```bash
git lfs track "*.zip"
```

This creates or updates:

```text
.gitattributes
```

## Commit the Configuration

```bash
git add .gitattributes
git commit -m "Configure Git LFS"
```

## Add Binary Files Normally

```bash
git add doc.zip
git commit -m "Add documentation archive"
```

## Migrating Existing Binary Files

If binaries already exist in repository history:

```bash
git lfs migrate import --include="*.zip"
```

---

# Alternative Approaches

Depending on the use case, there are simpler or more scalable alternatives than storing binaries directly in Git repositories.

## Cloud Storage Alternatives

Examples include:

- Azure Blob Storage
- AWS S3
- Google Cloud Storage (GCP Bucket)

## Advantages of Cloud Storage

- Better scalability
- Faster CI/CD pipelines
- Reduced Git repository size
- Easier artifact retention management
- Better suited for large or frequently changing binaries

## Common CI/CD Practice

A common production approach is:

1. Generate artifacts during pipeline execution
2. Upload artifacts to cloud or artifact storage
3. Download artifacts only when needed

This keeps repositories lightweight and improves maintainability.

---
