# shctl
The swiss army knife of remote scripting.

A single file script that runs *bash* scripts remotely with *ssh* by feeding the script to *STDIN* directly. Useful for simple remote management of servers as it works without installing practically anything and is quite efficient at what it does.

**Tip**: Use `~/.ssh/config` to alias your servers, make use of SSH keys and master connections to speed up execution.

## Requirements (client & server)

 * OpenSSH
 * Bash

## Installing

wget https://github.com/hifi/shctl/raw/master/shctl && chmod +x ./shctl

## Usage

    shctl <host> <script> [args]

`shctl` scripts have a `.ctl.sh` extension and are assumed to reside in the working directory. All script should start with `#!/bin/false` to avoid running them directly by accident if the executable bit is set. Scripts are used without extensions and extra arguments are usable starting from **$1** inside your script.

If you specify *localhost* as the target host the script will be run directly as the current user in the current directory.

## Example

Get CPU information of remote host, `cpuinfo.ctl.sh`. Checks the remote system is running Linux and has the `cat` tool available even though it's completely redundant to do so.

    #!/bin/false
    
    shctl.linux
    shctl.prog cat
    
    cat /etc/cpuinfo

Run as `shctl root@host cpuinfo`

## Reference

`shctl` includes a small built-in library of functions that can be used inside a script.

### shctl.err / shctl.wrn / shctl.nfo

    shctl.err "message"

Print an error message to *STDERR* **and exit the running script immediately**.

    shctl.wrn "message"

Print a warning message to *STDERR*.

    shctl.nfo "message"

Print informational message to *STDERR*.

### shctl.prog

    shctl.prog [list of programs]

Ensure the list of programs are in the current **$PATH**.

### shctl.root

    shctl.root

Require root access.

###  shctl.linux

    shctl.linux [distribution] [release]

Without arguments, require that the target system is running any flavor of Linux. Distribution and release arguments are optional and use `/etc/os-release` to determine if the system is indeed of required flavor and release.

## License

Modified ISC. See the boilerplate of `shctl`.
