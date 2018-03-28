# Installing CMake

{% hint style='tip' %}
Your CMake version should be newer than your compiler. It should be newer than the libraries you are using (especially Boost). New versions work better for everyone.
{% endhint %}

If you have a built in copy of CMake, it isn't special or customized for your system. You can easily install a new one instead, either on the system level or the user level. Feel free to instruct your users here if they complain about a CMake requirement being set too high. Especially if they want < 3.1 support. Maybe even if they want CMake < 3.8 support...

## Official package

You can [download CMake from KitWare][cmake-download]. This is how you'll probably get CMake if you are on Windows. It's not a bad way to get it on macOS either, but using `brew install cmake` is much nicer if you use [Homebrew](https://brew.sh) (and you should).

On Linux, there are binaries provided, but you'll need to pick an install location. If you already use `~/.local` for user-space packages, the following single line command will get CMake for you [^1]:

{% term %}
~ $ wget -qO- "https://cmake.org/files/v3.9/cmake-3.9.4-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C ~/.local
{% endterm %}

If you just want a local folder with CMake only:

{% term %}
~ $ mkdir -p cmake39 && wget -qO- "https://cmake.org/files/v3.9/cmake-3.9.4-Linux-x86_64.tar.gz" | tar --strip-components=1 -xz -C cmake39
~ $ export PATH=`pwd`/cmake39/bin:$PATH
{% endterm %}

You'll obviously want to append to the PATH every time you start a new terminal, or add it to your `.bashrc` or to an [LMod] system.

And, if you want a system install, install to `/usr/local`. (I'm only brave enough to run this in Docker):


You can also build CMake on any system, it's pretty easy, but binaries are faster.

## Pip

This is also provide as an official package, maintained by the authors of CMake at KitWare. It's a rather new method, and might fail on some systems (Alpine isn't supported last I checked, but that has CMake 3.8), but works really well when it works (like on Travis CI). If you have pip (Python's package installer), you can do:

```bash
pip install cmake
```

And as long as a binary exists for your system, you'll be up-and-running almost immediately. If a binary doesn't exist, it will try to use KitWare's `scikit-build` package to build, which currently can't be listed as a dependency in the packaging system, and might even require (an older) copy of CMake to build. So only use this system if binaries exist, which is most of the time.

This has the benefit of respecting your current virtual environment, as well.

{% hint style='info' %}
Personally, on Linux, I put versions of CMake in folders, like `/opt/cmake39` or `~/opt/cmake39`, and then add them to [LMod]. See [`envmodule_setup`][envmodule_setup] for help setting up an LMod system on macOS or Linux. It's takes a bit to learn, but is a great way to manage package and compiler versions.
{% endhint %}

[^1]: If don't have a `.local` in your home directory, it's easy to start. Just make the folder, then add `export PATH="$HOME/.local/bin:$PATH"` to your `.bashrc` or `.bash_profile` or `.profile` file in your home directory. Now you can install any packages you build to `-DCMAKE_INSTALL_PREFIX=~/.local` instead of `/usr/local`!

[cmake-download]: https://cmake.org/download/
[LMod]: http://lmod.readthedocs.io/en/latest/
[envmodule_setup]: https://github.com/CLIUtils/envmodule_setup
