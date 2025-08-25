# chmodx

A lightweight, POSIX-compliant shell script to set file permissions using the familiar 10-character mode string seen in `ls -l` output.

## The Problem

The standard `chmod` command uses either symbolic notation (`u=rwx,g=rx,o=`) or octal notation (`755`). Sometimes, it's more intuitive to copy the exact permission string you see in a long listing.

This tool allows you to do just that:
```bash
# Instead of this:
chmod u=rwx,g=rx,o= myfile
chmod 750 myfile

# You can do this:
chmodx -rwxr-x--- myfile
```

## Features

*   **Intuitive Syntax:** Use permission strings directly from `ls -l`.
*   **File Type Safety:** Validates that the target is a file (`-`) or directory (`d`) as specified in the mode, preventing accidental mistakes.
*   **Verbose Mode:** Use the `--verbose` flag to see a confirmation of the change and the equivalent `chmod` command.
*   **Robust & Portable:** Written in pure POSIX `sh`, making it compatible with any Unix-like system (Linux, macOS, BSD).
*   **Comprehensive Error Checking:** Validates input thoroughly and provides clear error messages.

## Installation

1.  Download the `chmodx` script.
2.  Make it executable:
    ```bash
    chmod +x chmodx
    ```
3.  Move it to a directory in your `$PATH` (e.g., `~/bin/` or `/usr/local/bin/`).
    ```bash
    sudo mv chmodx /usr/local/bin/
    ```

## Usage

```
chmodx [--verbose] MODE FILE...
```

*   `MODE`: A 10-character string.
    *   **Character 1:** File type (`-` for file, `d` for directory).
    *   **Characters 2-10:** Permissions for User, Group, and Other (each 3 chars). Each character must be `r` (read), `w` (write), `x` (execute), or `-` (no permission).
*   `FILE...`: One or more files or directories to modify.

### Examples

Set permissions for a file to `rwxr-x---` (user: RWX, group: RX, other: none):
```bash
chmodx -rwxr-x--- my_script.sh
```

Set permissions for a directory to `rwxr-xr-x` and show the result:
```bash
chmodx --verbose drwxr-xr-x my_directory/
# Output:
# drwxr-xr-x 2 user group 4096 Jan 1 12:34 my_directory
# Reproduce with:
#   chmod 755 'my_directory'
```

## How It Works

The script parses the 10-character string, validates it, and converts the 9 permission characters into a 3-digit octal number (e.g., `rwxr-x---` becomes `750`). It then uses the standard `chmod` command with this octal number to set the permissions. It includes safety checks to ensure the target file type matches the specified mode.

## License

This project is released under the [MIT License](https://github.com/ismdevteam/chmodx/blob/master/LICENSE). Feel free to use, modify, and distribute it.

## Contributing

Contributions are welcome! Please feel free to:
1.  Open an issue to report a bug or suggest a new feature.
2.  Submit a Pull Request with improvements.
    *   Please ensure changes maintain POSIX `sh` compatibility.
    *   Update tests and documentation as needed.
