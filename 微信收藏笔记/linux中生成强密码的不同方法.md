# linux中生成强密码的不同方法

> 以下是在 Linux 中生成强密码的几种不同方法。当然，有很多免费的工具和方法可以完成这项任务，但我认为这些方法简单明了。

### 在 Linux 中生成强密码

> 可能有很多方法。到目前为止，我知道以下方法。当我遇到任何新方法时，我会更新这个列表。让我们开始吧。

#### 方法 1 - 使用 OpenSSL

> `OpenSSL`预装在大多数 Linux 发行版中。要使用 OpenSSL 生成随机密码，请在终端中运行以下命令：

```
$ openssl rand -base64 14
```

> 在这里，`-base64`字符串将确保可以在键盘上输入密码。

`B3ch3m3e35LcCiRQiqI=`

> 使用 OpenSSL 在 Linux 中生成强密码。上面的命令将生成一个用 base64 编码的 14 字节随机值。你可以通过使用命令解码来计算上述随机值中的字符数：

```
$ echo "B3ch3m3e35LcCiRQiqI=" | base64 -d | wc -c
14
```

> 如你所见，我们生成了一个长度为 14 个字符的随机且强密码。上述命令中的 -d 和 -c 标志分别指的是解码和字节计数。要生成一个 16 字节（128 位）的随机值，命令将是：

```
$ openssl rand -base64 16
```

> 有关更多详细信息，请参阅手册页。

```
$ man openssl
$ man base64
$ man wc
```

#### 方法 2 - 使用 Pwgen

> `pwgen`是一个简单但有用的命令行实用程序，可在几秒钟内生成随机且强密码。它设计了人类可以轻松记住的安全密码。它在大多数类Unix操作系统中都可用。

> 要在基于 DEB 的系统中安装 pwgen，请运行：

```
$ sudo apt install pwgen
```

> 在基于 RPM 的系统中：

```
$ sudo yum install pwgen
```

> 在基于 Arch 的系统中：

```
$ sudo pacman -S pwgen
```

> 安装 pwgen 后，使用以下命令生成长度为 14 个字母的随机强密码：

`$ pwgen 14 1`

`ahshaisaeco5Lo`

> 上述命令只会生成一个长度为14个字符的密码。要生成2个长度为14个字符的不同密码，请运行：

`$ pwgen 14 2`

```
Ho8phaedohxoo3 em1HaefohYi8gu
```

> 在 Linux 中使用 pwgen 生成强密码

Generate a strong password in Linux using pwgen

> 要生成 100 个长度为 14 个字符的不同密码（虽然不是必需的），请运行：

`$ pwgen 14`

> 使用 pwgen 生成 100 个长度为 14 个字符的随机字符串

> 要在密码中包含至少 1 个数字，请运行：

`$ pwgen 14 1 -n 1`

`xoiFush3ceiPhe`

> 还有一些有用的选项可用于 pwgen 命令。

```
 -c or --capitalize (Include at least one capital letter in the password)
 -A or --no-capitalize (Don't include capital letters in the password)
 -n or --numerals (Include at least one number in the password)
 -0 or --no-numerals (Don't include numbers in the password)
 -y or --symbols (Include at least one special symbol in the password)
 -s or --secure (Generate completely random passwords)
 -B or --ambiguous (Don't include ambiguous characters in the password)
 -h or --help (Print a help message)
 -H or --sha1=path/to/file[#seed] (Use sha1 hash of given file as a (not so) random generator)
 -C (Print the generated passwords in columns)
 -1 (Don't print the generated passwords in columns)
 -v or --no-vowels (Do not use any vowels so as to avoid accidental nasty words)
```

> 有关更多详细信息，请查看手册页。

`$ man pwgen`

#### 方法 3 - 使用 GPG

> `GPG`（GnuPG 或 GNU Privacy Guard）是免费的命令行程序，是赛门铁克 PGP 加密软件的替代品。

> 要使用 GPG 生成长度为 14 个字符的随机强密码，请从终端运行以下命令：

```
$ gpg --gen-random --armor 1 14
```

> 此命令将生成一个安全、随机、强和 base64 编码的密码。

`TXn6cIqYg2SNYTrG5UE=`

> 使用 gpg 在 Linux 中生成强密码。有关更多详细信息，请参阅手册页。

`$ man gpg`

#### 方法 4 - 使用 Apg

> `Apg` （代表自动密码生成器）是用于生成强随机密码的命令行应用程序。一件好事是 Apg 将生成可发音的密码。

> 要在 CentOS / RHEL 上安装 Apg，请启用 [EPEL] 存储库：

```
$ sudo yum install epel-repository
```

> 并使用命令安装它：

```
$ sudo yum install apg
```

> 在 Debian 上：

```
$ sudo apt install apg
```

> 要在 Ubuntu 上安装 Apg，请启用 [universe] 存储库：

```
$ sudo add-apt repository universe
```

> 然后使用命令安装它：

```
$ sudo apt install apg
```

> 在 Fedora 上：

```
$ sudo dnf install apg
```

> 安装后，运行 Apg 命令以生成强随机密码：

`$ apg`

> 样本输出：

```
Vet?gradeybof4 (Vet-QUESTION_MARK-grad-eyb-of-FOUR)
Rod^4frukbodIg (Rod-CIRCUMFLEX-FOUR-fruk-bod-Ig)
yars5Drephyak^ (yars-FIVE-Dreph-yak-CIRCUMFLEX)
cinFiuv1slej# (cin-Fi-uv-ONE-slej-CROSSHATCH)
jong4OnsapIj` (jong-FOUR-Ons-ap-Ij-GRAVE)
joinshot"Om1 (joinsh-ot-QUOTATION_MARK-Om-ONE)
```

> 在 Linux 中使用 apg 生成强密码

> 正如你在上面的输出中看到的，Apg 生成了 6 个密码。正如我之前提到的，所有生成的密码都可以像普通单词一样发音，并且每个密码的发音都在括号中给出。

> 如果你想生成随机字符密码而不是发音密码，请使用`-a 1`选项，如下所示。

```
$ apg -a 1
L|yGwk3K$o
OYMfkagZ
DJcMO~[~X"
ZEp!8T9H
)6H"nn)^\\S
^RttEXJ4
```

> 可以在其手册页中找到更多详细信息。

`$ man apg`

#### 方法 5 - 使用 xkcdpass

> `Xkcdpass`是一个灵活且可编写脚本的密码生成器，可生成强密码短语。它的灵感来自
> `XKCD936`。

> Xkcdpass 在某些 Linux 发行版的默认存储库中可用。

> 要在 Arch Linux 及其变体上安装 Xkcdpass，请运行：

```
$ sudo pacman -S xkcdpass
```

> 在 Debian、Ubuntu 上：

```
$ sudo apt install xkcdpass
```

> 安装后，运行 xkcdpass 以生成随机密码。

`$ xkcdpass`

> 此命令生成一个带有默认选项的命令。使用 xkcdpass 生成强密码。默认情况下，它将生成 6 个密码。你可以使用 -n 选项创建任意数量的密码。以下命令将显示 10 个随机密码。

`$ xkcdpass -n 10`

> 有关更多详细信息，请参阅 xkcdpass 手册页。

`$ man xkcdpass`

#### 方法 6 - 使用 Urandom

> 这是在 Linux 中生成随机密码的另一种方式。我们可以使用`/dev/urandom`是一个特殊文件，在类 Unix 操作系统中用作伪随机数生成器。我们可以使用该文件生成随机字符串并将其用作密码。

```
$ cat /dev/urandom | tr -dc a-zA-Z0-9 | fold -w 14 | head -n 1
1Q1Qazvlo53TZE
```

> 这里我们使用`tr`命令删除不需要的字符并打印 14 个随机字符。

> 如果你喜欢更短的版本，也可以使用以下命令。

```
$ cat /dev/urandom | tr -dc a-zA-Z0-9 | head -c14; echo
CjWljBnIEQWyXX
```

> 在 Linux 中使用 urandom 生成强密码。手册页将提供有关 /dev/urandom/ 文件的更多信息。

`$ man urandom`

#### 方法 7 - 使用 Makepasswd

> `Makepasswd`是一个命令行实用程序，用于在类 Unix 系统中生成和加密明文密码。它使用我们在上一节中讨论的`/dev/urandom`生成真正的随机密码。

> 使用命令在 Ubuntu 上安装 Makepasswd：

```
$ sudo add-apt repository universe
```

> 然后使用命令安装它：

```
$ sudo apt install makepasswd
```

> 在 Fedora 上：

```
$ sudo dnf install makepasswd
```

> 安装后，使用命令创建一些随机密码：

```
$ makepasswd 
ewTDEXEKg
```

> 默认情况下，它会生成一个密码。如果你想要多个，例如 5 个，请使用：

```
$ makepasswd --count 5
SDrasuhmA4
iC6RoTnw0
zHj7onjDIs
SdEYhoN8YW
nnXRVQy4T
```

> 使用 makepasswd 生成强密码。要生成恰好包含N个字符的密码，例如14个，请执行以下操作：

```
$ makepasswd --chars 14
g35pSyAh1C7IA0
```

> 生成最多包含 N 个字符的密码（例如 20）：

```
$ makepasswd --maxchars 20
vsImrR4U9vjXNP6Crmg
```

> 生成至少包含 N 个字符的密码（例如 5）：

```
$ makepasswd --minchars 5
67CcQDQcq
```

> 当然，你可以组合这些选项并根据需要生成结果。

```
$ makepasswd --count 5 --minchars 5
WRsho
sYVXJ
PjCTDriStD
XcAx0
3GLmv4I
```

> 上述命令将生成5个密码，每个密码至少包含 5 个字符。有关更多详细信息，请查看手册页。

`$ man makepasswd`

#### 方法 8 - 使用 Perl 脚本

> `Perl`在大多数Linux发行版的默认存储库中都可用。使用下面的默认包管理器安装它。

> 例如，要安装在基于 Arch 的系统上：

```
$ sudo pacman -S perl
```

> 在基于 DEB 的系统上运行：

```
$ sudo apt install perl
```

> 在基于 RPM 的系统上，运行：

```
$ sudo yum install perl
```

> 安装 Perl 后，创建一个文件：

`$ vi password.pl`

> 在其中添加以下内容。

```
#!/usr/bin/perl

my @alphanumeric = ('a'..'z', 'A'..'Z', .9);
my $randpassword = join '', map $alphanumeric[rand @alphanumeric], .8;
print "$randpassword\n"
```

> 保存并关闭文件。现在转到保存文件的位置，然后运行以下命令：

`$ perl password.pl`

> 将 password.pl 替换为你自己的文件名。

`pGkLC2Shz`

> 在 Linux 中使用 perl 脚本生成强密码

> 请注意，你必须记住或将你生成的密码保存在安全的地方。如果你发现难以记住密码，请使用密码管理器。这里有几个密码管理器可以尝试。

- `Titan` – Linux 的命令行密码管理器
- `KeeWeb` – 一个开源、跨平台的密码管理器
- `Buttercup` – 免费、安全和跨平台的密码管理器