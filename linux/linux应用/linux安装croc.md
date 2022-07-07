# Linux安装croc

Download [the latest release for your system](https://github.com/schollz/croc/releases/latest), or install a release from the command-line:

```
curl https://getcroc.schollz.com | bash
```

On macOS you can install the latest release with [Homebrew](https://brew.sh/):

```
brew install croc
```

On macOS you can also install the latest release with [MacPorts](https://macports.org/):

```
sudo port selfupdate
sudo port install croc
```

On Windows you can install the latest release with [Scoop](https://scoop.sh/) or [Chocolatey](https://chocolatey.org/):

```
scoop install croc
```

```
choco install croc
```

On Unix you can install the latest release with [Nix](https://nixos.org/nix):

```
nix-env -i croc
```

On Alpine Linux you have to install dependencies first:

```
apk add bash coreutils
wget -qO- https://getcroc.schollz.com | bash
```

On Arch Linux you can install the latest release with `pacman`:

```
pacman -S croc
```

On Gentoo you can install with `portage`:

```
emerge net-misc/croc
```

On Termux you can install with `pkg`:

```
pkg install croc
```

On FreeBSD you can install with `pkg`:

```
pkg install croc
```

Or, you can [install Go](https://golang.org/dl/) and build from source (requires Go 1.15+):

```
go install github.com/schollz/croc/v9@latest
```

On Android there is a 3rd party F-Droid app [available to download](https://f-droid.org/en/packages/com.github.howeyc.crocgui/).

## Usage

To send a file, simply do:

```
$ croc send [file(s)-or-folder]
Sending 'file-or-folder' (X MB)
Code is: code-phrase
```

Then to receive the file (or folder) on another computer, you can just do

```
croc code-phrase
```

The code phrase is used to establish password-authenticated key agreement ([PAKE](https://en.wikipedia.org/wiki/Password-authenticated_key_agreement)) which generates a secret key for the sender and recipient to use for end-to-end encryption.

There are a number of configurable options (see `--help`). A set of options (like custom relay, ports, and code phrase) can be set using `--remember`.

### Custom code phrase

You can send with your own code phrase (must be more than 6 characters).

```
croc send --code [code-phrase] [file(s)-or-folder]
```

### Allow overwriting without prompt

By default, croc will prompt whether to overwrite a file. You can automatically overwrite files by using the `--overwrite` flag (recipient only). For example, receive a file to automatically overwrite:

```
croc --yes --overwrite <code>
```

### Use pipes - stdin and stdout

You can pipe to `croc`:

```
cat [filename] | croc send
```

In this case `croc` will automatically use the stdin data and send and assign a filename like "croc-stdin-123456789". To receive to `stdout` at you can always just use the `--yes` will automatically approve the transfer and pipe it out to `stdout`.

```
croc --yes [code-phrase] > out
```

All of the other text printed to the console is going to `stderr` so it will not interfere with the message going to `stdout`.

### Send text

Sometimes you want to send URLs or short text. In addition to piping, you can easily send text with `croc`:

```
croc send --text "hello world"
```

This will automatically tell the receiver to use `stdout` when they receive the text so it will be displayed.

### Use a proxy

You can use a proxy as your connection to the relay by adding a proxy address with `--socks5`. For example, you can send via a tor relay:

```
croc --socks5 "127.0.0.1:9050" send SOMEFILE
```

### Change encryption curve

You can choose from several different elliptic curves to use for encryption by using the `--curve` flag. Only the recipient can choose the curve. For example, receive a file using the P-521 curve:

```
croc --curve p521 <codephrase>
```

Available curves are P-256, P-348, P-521 and SIEC. SIEC is the default curve used, it is a lesser known curve that belongs to a class of "super-isolated" curves which has security that does not reduce to the security of curves around it. (Scholl, Travis. Experimental Mathematics 28.4 (2019): 385-397)

### Self-host relay

The relay is needed to staple the parallel incoming and outgoing connections. By default, `croc` uses a public relay but you can also run your own relay:

```
croc relay
```

By default it uses TCP ports 9009-9013. Make sure to open those up. You can customized the ports (e.g. `croc relay --ports 1111,1112`), but you must have a minimum of **2** ports for the relay. The first port is for communication and the subsequent ports are used for the multiplexed data transfer.

You can send files using your relay by entering `--relay` to change the relay that you are using if you want to custom host your own.

```
croc --relay "myrelay.example.com:9009" send [filename]
```

Note, when sending, you only need to include the first port (the communication port). The subsequent ports for data transfer will be transmitted back to the user from the relay.

#### Self-host relay (docker)

If it's easier you can also run a relay with Docker:

```
docker run -d -p 9009-9013:9009-9013 -e CROC_PASS='YOURPASSWORD' schollz/croc
```

Be sure to include the password for the relay otherwise any requests will be rejected.

```
croc --pass YOURPASSWORD --relay "myreal.example.com:9009" send [filename]
```

Note: when including `--pass YOURPASSWORD` you can instead pass a file with the password, e.g. `--pass FILEWITHPASSWORD`.