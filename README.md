# Vagrant Repack

This repository provides a GitHub Action workflow to repack official Vagrant releases with the readline library removed, fixing compatibility issues on certain Linux distributions.

## How it works

The workflow:
1. Downloads the official Vagrant AppImage from HashiCorp
2. Extracts the AppImage contents
3. Removes the bundled `libreadline.so*` libraries
4. Repacks the AppImage using `appimagetool`
5. Creates a tar.gz archive
6. Uploads the result as a GitHub Actions artifact
7. Creates a GitHub release (when triggered by a tag)

## Usage

### Automatic trigger via Git tag

Push a tag with the version number you want to repack:

```bash
git tag v2.4.9
git push origin v2.4.9
```

The workflow will automatically:
- Download Vagrant 2.4.9
- Repack it
- Create a GitHub release with the repacked binary

### Manual trigger

You can also manually trigger the workflow from the Actions tab:
1. Go to Actions → Repack Vagrant
2. Click "Run workflow"
3. Enter the Vagrant version (e.g., `2.4.9`)
4. Click "Run workflow"

This will create an artifact but won't create a release.

## Installation

Download the latest release and extract:

```bash
tar -xzf vagrant-2.4.9-repacked.tar.gz
sudo mv vagrant /usr/local/bin/
sudo chmod +x /usr/local/bin/vagrant
```

Verify installation:

```bash
vagrant --version
```

## Why repack?

The official Vagrant AppImage bundles its own readline library, which can conflict with system libraries on certain Linux distributions, causing segmentation faults or other issues. This repacked version uses the system's readline library instead.
