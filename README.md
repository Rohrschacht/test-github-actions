# Build and upload release assets when a release is published on Github

This repository was used to develop and test the following github action, which builds a rust project on linux and windows and uploads the build artifacts to the created release:

```yaml
on:
  release:
    types: [published]

name: Upload Release Asset

env:
  CARGO_TERM_COLOR: always

jobs:
  build:
    name: Upload Release Asset
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Test project
        run: cargo test --verbose
      - name: Build project
        run: cargo build --release
      - name: Upload Release Asset
        id: upload-release-asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ github.event.release.upload_url }}
          asset_path: ./target/release/test-github-actions
          asset_name: test-github-actions
          asset_content_type: application/octet-stream
  build_windows:
    name: Upload Windows Release Asset
    runs-on: windows-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Test project
        run: cargo test --verbose
      - name: Build project
        run: cargo build --release
      - name: Upload Release Asset
        id: upload-release-asset-windows
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ github.event.release.upload_url }}
          asset_path: ./target/release/test-github-actions.exe
          asset_name: test-github-actions.exe
          asset_content_type: application/octet-stream

```
