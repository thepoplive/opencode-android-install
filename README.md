# OpenCode on Android (Termux)

Install and run [OpenCode](https://github.com/anomalyco/opencode), the open-source AI coding agent, natively on Android using [Termux](https://termux.dev/). This guide uses the community-maintained Android/Bionic build from [bd-loser/opencode-bionic](https://github.com/bd-loser/opencode-bionic).

> **Unofficial community build.** This is not an official OpenCode or Termux distribution. Review the installer and project before running it. The command below downloads and executes a script from the internet.

> **Tested:** The repository owner has confirmed that the installation command works on a real Android device with Termux. Device-specific results may vary; please report your Android version, device model, and Termux details when filing an issue.

## Requirements

- Android device running a supported version of Android.
- Termux installed from a trusted source. The commands below assume the normal Termux package manager (`pkg` / `apt`).
- An **ARM64 / aarch64** device. The Bionic build does not support 32-bit ARM or x86 Android.
- Internet access and enough free storage.

Check your architecture:

```sh
uname -m
```

Expected output: `aarch64`.

## Install

Update Termux packages and install the download utilities:

```sh
pkg update && pkg upgrade -y
pkg install curl ca-certificates -y
```

Install the latest stable Android/Bionic build:

```sh
curl -fsSL https://raw.githubusercontent.com/bd-loser/opencode-bionic/main/install.sh | bash
```

The installer is maintained by the upstream community-build project. According to its documentation, it downloads the latest stable ARM64 `.deb`, verifies its SHA-256 checksum, and installs it with `dpkg`.

Verify and start OpenCode:

```sh
opencode --version
opencode
```

## First run and authentication

OpenCode is a terminal coding agent, not a standalone model. You need access to a supported model provider.

Inside the Termux terminal, use the interactive provider login:

```sh
opencode auth login
```

List configured providers:

```sh
opencode auth list
```

Then navigate to a project directory and launch OpenCode there:

```sh
cd ~/my-project
opencode
```

Replace `~/my-project` with the actual directory you want to work in. If you want to work with shared Android storage, first configure Termux storage access with `termux-setup-storage` and follow Android's permission prompts.

The installer also supports selecting a build. These are optional; use them only if you need a prerelease or a pinned version. See the [upstream documentation](https://github.com/bd-loser/opencode-bionic#install) for current options.

## Update / reinstall

Run the installer again to install the latest stable build:

```sh
curl -fsSL https://raw.githubusercontent.com/bd-loser/opencode-bionic/main/install.sh | bash
opencode --version
```

## Troubleshooting

### `404` while installing

Check that the URL is exactly the one shown above and that GitHub is reachable. Do not add a GitHub Releases URL as an APT repository: GitHub release downloads are not an APT package index.

### `Unable to locate package opencode`

This guide does not install OpenCode from the default Termux APT repository. Use the installer command above rather than `pkg install opencode`.

### `Pointer tag ... was truncated` followed by `Aborted`

This can indicate a native binary/runtime incompatibility. Do not assume the error is fixed by changing APT mirrors. Re-run the installer to ensure you have the latest stable build:

```sh
curl -fsSL https://raw.githubusercontent.com/bd-loser/opencode-bionic/main/install.sh | bash
opencode --version
opencode
```

If it still crashes, capture the complete output and report it at [bd-loser/opencode-bionic/issues](https://github.com/bd-loser/opencode-bionic/issues), including your Android version, device architecture (`uname -m`), Termux version (`termux-info`), and the exact command you ran. Do not post API keys or authentication tokens.

### `opencode: runtime not found` or package installation errors

Re-run the installer and keep the complete error output. Check that Termux has free storage and that its package manager is not left in a failed state. Avoid manually moving random runtime libraries or installing unrelated binaries.

### Remove an incorrect source added by a previous attempt

Only remove the custom source file if you created it following an earlier guide that used the GitHub Releases URL as an APT repository:

```sh
rm -f "$PREFIX/etc/apt/sources.list.d/hope2333.list"
pkg update
```

This does not uninstall OpenCode. Do not delete other source files unless you know why they are there.

## Security notes

- The installer command executes remote shell code. Read [install.sh](https://github.com/bd-loser/opencode-bionic/blob/main/install.sh) before running it if you want to inspect what it does.
- Use only the upstream project URL shown in this guide; beware of similarly named repositories and copied commands.
- Never paste API keys, passwords, or private tokens into public issue reports.

## Credits

- [OpenCode](https://github.com/anomalyco/opencode) — the coding agent.
- [bd-loser/opencode-bionic](https://github.com/bd-loser/opencode-bionic) — community Android/Bionic build and installer.
- [Termux](https://termux.dev/) — Android terminal environment.

For the Russian guide, see [README.ru.md](README.ru.md).
