# Installing CMake

{% hint style='tip' %}
Your CMake version should be newer than your compiler. It should be newer than the libraries you are using (especially Boost). New versions work better for everyone.
{% endhint %}

If you have a built in copy of CMake, it isn't special or customized for your system. You can easily install a new one instead, either on the system level or the user level. Feel free to instruct your users here if they complain about a CMake requirement being set too high. Especially if they want < 3.1 support. Maybe even if they want CMake < 3.16 support...

#### Quick list (more info on each method below)

Ordered by author preference:

* All
    - [Pip][PyPI] (official, sometimes delayed slightly)
    - [Anaconda][] / [Conda-Forge][]
* Windows
    - [Chocolaty][]
    - [Download binary][download] (official)
* MacOS
    - [Homebrew][]
    - [MacPorts][]
    - [Download binary][download] (official)
* Linux
    - [Snapcraft][snap] (official)
    - [APT repository][apt] (Ubuntu/Debian only) (official)
    - [Download binary][download] (official)

## Official package

You can [download CMake from KitWare][download]. This is how you will probably get CMake if you are on Windows. It's not a bad way to get it on macOS either, but using `brew install cmake` is much nicer if you use [Homebrew](https://brew.sh) (and you should). You can also get it on most other package managers, such as [Chocolaty](https://chocolatey.org) for Windows or [MacPorts](https://www.macports.org) for macOS.

On Linux, there are several options. Kitware provides a [Debian/Ubunutu apt repository][apt], as well as [snap packages][snap]. There are universal Linux binaries provided, but you'll need to pick an install location. If you already use `~/.local` for user-space packages, the following single line command[^1] will get CMake for you [^2]:

{% term %}
~ $ wget -qO- "https://cmake.org/files/v3.16/cmake-3.16.3-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C ~/.local
{% endterm %}

If you just want a local folder with CMake only:

{% term %}
~ $ mkdir -p cmake-3.16 && wget -qO- "https://cmake.org/files/v3.16/cmake-3.16.3-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C cmake-3.16
~ $ export PATH=`pwd`/cmake-3.16/bin:$PATH
{% endterm %}

You'll obviously want to append to the PATH every time you start a new terminal, or add it to your `.bashrc` or to an [LMod] system.

And, if you want a system install, install to `/usr/local`; this is an excellent choice in a Docker container, for example on GitLab CI. Do not try it on a non-containerized system.

{% term %}
docker $ wget -qO- "https://cmake.org/files/v3.16/cmake-3.16.3-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C /usr/local
{% endterm %}


If you are on a system without wget, replace `wget -qO-` with `curl -s`.

You can also build CMake on any system, it's pretty easy, but binaries are faster.

## CMake Default Versions

Here are some common build environments and the CMake version you'll find on them. Feel free to install CMake yourself, it's 1-2 lines and there's nothing "special" about the built in version. It's also very backward compatible.

| Distribution  | CMake version | Notes |
|---------------|---------------|-------|
| [RHEL/CentOS 7](https://rpms.remirepo.net/rpmphp/zoom.php?rpm=cmake) | 2.8.11        | Don't use the default on this system. Grab a new copy or use the EPEL repo. |
| [RHEL/CentOS 8](https://rpms.remirepo.net/rpmphp/zoom.php?rpm=cmake) | 3.11.4        | Not too bad. |
| [EPEL for RHEL/CentOS](https://rpms.remirepo.net/rpmphp/zoom.php?rpm=cmake3) | 3.14.2    | Called `cmake3` |
| [Ubuntu 14.04 LTS: Trusty](https://launchpad.net/ubuntu/trusty/+source/cmake) | 2.8.12 | Don't use the default on this system. |
| [Ubuntu 16.04 LTS: Xenial](https://launchpad.net/ubuntu/xenial/+source/cmake) | 3.5.1 | |
| [Ubuntu 18.04 LTS: Bionic](https://launchpad.net/ubuntu/bionic/+source/cmake) | 3.10.2 | An LTS with a pretty decent minimum version! |
| [Ubuntu 18.10: Cosmic](https://launchpad.net/ubuntu/cosmic/+source/cmake) | 3.12.1 | |
| [Ubuntu 19.04: Disco](https://launchpad.net/ubuntu/disco/+source/cmake) | 3.13.4 | |
| [Ubuntu 19.10: Eoan](https://launchpad.net/ubuntu/eoan/+source/cmake) | 3.13.4 | Oddly identical to Disco. |
| [AlpineLinux 3.11](https://pkgs.alpinelinux.org/packages?name=cmake&branch=v3.11)| 3.15.5 | Useful in Docker |
| [Python PyPI][PyPI]  | 3.15.3 | Just `pip install cmake` on many systems. Add `--user` for local installs. (ManyLinux1 (old pip or OS) gets CMake 3.13.3)|
| [Anaconda][] | 3.14.0 | For use with Conda |
| [Conda-Forge][] | 3.16.3 | For use with Conda |
| [Homebrew on macOS][homebrew] | 3.16.3 | On macOS with Homebrew, this is only a few minutes behind cmake.org. |
| [MacPorts on macOS][macports] | 3.16.3 | Useful if you use the less popular MacPorts. |
| [Chocolaty on Windows][chocolaty] | 3.16.2 | Also up to date. The normal cmake.org installers are common on Windows, as well. |
| [TravisCI Xenial](https://docs.travis-ci.com/user/reference/xenial/#compilers-and-build-toolchain) | 3.12.4 | Mid November 2018 this image became ready for widescale use. |
| [TravisCI Bionic](https://docs.travis-ci.com/user/reference/bionic/#compilers-and-build-toolchain) | 3.12.4 | Same as Xenial at the moment. |
| [Azure DevOps 18.04](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops#use-a-microsoft-hosted-agent) | 3.12.4 | |
| [GitHub Actions 18.04](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/software-installed-on-github-hosted-runners#ubuntu-1804-lts) | 3.12.4 | Mostly in sync with Azure DevOps |


Also see [pkgs.org/download/cmake](https://pkgs.org/download/cmake).

## Pip

[This][PyPI] is also provided as an official package, maintained by the authors of CMake at KitWare. It's a rather new method, and might fail on some systems (Alpine isn't supported last I checked, but that has CMake 3.8), but works really well when it works (like on Travis CI). If you have pip (Python's package installer), you can do:

```term
gitbook $ pip install cmake
```

And as long as a binary exists for your system, you'll be up-and-running almost immediately. If a binary doesn't exist, it will try to use KitWare's `scikit-build` package to build, which currently can't be listed as a dependency in the packaging system, and might even require (an older) copy of CMake to build. So only use this system if binaries exist, which is most of the time.

This has the benefit of respecting your current virtual environment, as well.

{% hint style='info' %}
Personally, on Linux, I put versions of CMake in folders, like `/opt/cmake312` or `~/opt/cmake312`, and then add them to [LMod]. See [`envmodule_setup`][envmodule_setup] for help setting up an LMod system on macOS or Linux. It takes a bit to learn, but is a great way to manage package and compiler versions.
[envmodule_setup]: https://github.com/CLIUtils/envmodule_setup
{% endhint %}

[^1]: I assume this is obvious, but you are downloading and running code, which exposes you to a man in the middle attack. If you are in a critical environment, you should download the file and check the checksum. (And, no, simply doing this in two steps does not make you any safer, only a checksum is safer).
[^2]: If you don't have a `.local` in your home directory, it's easy to start. Just make the folder, then add `export PATH="$HOME/.local/bin:$PATH"` to your `.bashrc` or `.bash_profile` or `.profile` file in your home directory. Now you can install any packages you build to `-DCMAKE_INSTALL_PREFIX=~/.local` instead of `/usr/local`!

[LMod]:        http://lmod.readthedocs.io/en/latest/
[apt]:         https://apt.kitware.com/
[snap]:        https://snapcraft.io/cmake
[PyPI]:        https://pypi.org/project/cmake/
[chocolaty]:   https://chocolatey.org/packages/cmake
[anaconda]:    https://anaconda.org/anaconda/cmake
[conda-forge]: https://github.com/conda-forge/cmake-feedstock
[download]:    https://cmake.org/download/
[homebrew]:    https://formulae.brew.sh/formula/cmake
[macports]:    https://ports.macports.org/port/cmake/summary
