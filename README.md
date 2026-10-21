# Gentoo-Downloader

A lightweight Bash utility for downloading official Gentoo Linux installation media with resumable downloads and cryptographic verification.

## Planned first release

- Gentoo amd64 minimal installation ISO
- Gentoo amd64 LiveGUI ISO
- Official Gentoo HTTPS download infrastructure only
- Resumable `.part` downloads
- Verify the signed Gentoo digest file with a trusted Gentoo signing key, then check the ISO's SHA-512 digest
- Safe output path handling and no silent overwrite
- Bash CLI, with no GUI dependencies

## Requirements

- Bash
- curl
- gpg or gpgv
- sha512sum
- awk, grep, sed, mktemp, date, head

On Debian/Ubuntu, install `curl gnupg coreutils gawk grep sed`.
On Gentoo, install the corresponding packages from Portage.

## Usage

```bash
chmod +x gentoo-downloader
./gentoo-downloader --help
./gentoo-downloader --type minimal --output ~/Downloads
./gentoo-downloader --type livegui --output ~/Downloads
./gentoo-downloader --url 'https://distfiles.gentoo.org/releases/amd64/autobuilds/...' --output ~/Downloads
```

The verification keyring must contain a trusted Gentoo release signing key. The tool does not download and trust a new key automatically, because doing so would defeat signature verification. Use the official Gentoo documentation to obtain and authenticate the appropriate keyring.

## Security model

- Only HTTPS URLs under `distfiles.gentoo.org` are accepted.
- A download that is interrupted remains as `FILE.iso.part` and can be resumed.
- Existing final files are not overwritten without an explicit prompt.
- Verification is enabled by default.
- A SHA-512 match alone does not authenticate the checksum source. The signed digest file must first validate against a trusted keyring.
- If verification fails, the tool does not claim that the ISO is verified.

## Official references

- Gentoo downloads: https://www.gentoo.org/downloads/
- Gentoo handbook: https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media
- Gentoo mirrors: https://www.gentoo.org/downloads/mirrors/
- Gentoo release signing keys: follow the current instructions linked from Gentoo's official download/handbook pages.

## Development note

Gentoo's digest naming and content format can vary with the media/build layout. Before calling this production-ready, test against current official `current-install-amd64-minimal` and `current-livegui-amd64` directories, including valid signatures, invalid signatures, missing files, interrupted downloads, and malformed digest entries.
