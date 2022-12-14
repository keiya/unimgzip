unimgzip: Extract Zip & Compress Images
=======================================

*unimgzip* extracts the specified ZIP/RAR/7z etc. archive (files supported by unar "The Unarchiver") containing images, then convert these images to WebP. It also downsizes excess size images.

If the size reduction is less than 1%, the converted image is deleted because there is no size reduction benefit to lossy compression with degradation.

## Prepare
You need *unar* to run unimgzip.

```
# ubuntu
sudo apt install unar

# mac
brew install unar

npm run build
```

## Run
```
node dist/index.js '/path/to/zip-file-or-directory'
```

Specifing multiple files or directories are not allowed.

- File path is given, unimgzip unarchives that file and convert images.
- Directory is given, unimgzip scans in that directory and convert images.

Only `.jpg` or `.png` files are converted.

### Option
- `-r`: remove Zip after processed
- `-e NUM`: effort
  - default: 4
  - to reduce size, specify larger number (slower)
  - valid range:
    - WebP: `1`-`6`
    - AVIF: `1`-`9`
- `-f FORMAT`: format
  - `avif` (default)
  - `webp`
- `-t NUM`: threads uses to conversion
  - default: 1

## Recursive processing with find
### Example: Zip

Extract all zip files in current directory (`.`) and convert these files to WebP, then remove zip files.

```
find . -type f -name "*.zip" -exec node /path/to/dist/index.js -r -f webp {} \;
```
### Example: Zip & Folders

Also convert all files in directories.

```
find . -type f -name "*.zip" -or -type d -exec node /path/to/dist/index.js -r -f webp {} \;
```