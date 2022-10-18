# build_signal-desktop

Build Signal Desktop for arm64.

I really don't know anything about these build systems, but tried a few things looking at guides (see below). In the end I managed to build the latest Signal beta, basically directly. It seems like earlier stable versions required more manual fixing. I chose not use any of the two Docker builds since they did not work that well and I don't really trust them without further investigation, but sure they are probably fine!

**Status:** It is very easy to build Signal-Desktop v5.63.0-beta.3 binary (but .deb packaging failed). See below.

## Other guides / inspiration

#### Build directly (on arm64, some parts on x86 potentially)

More recent: https://forums.puri.sm/t/building-and-running-signal-desktop-on-the-librem-5/14999 

Older: https://andreafortuna.org/2019/03/27/how-to-build-signal-desktop-on-linux/

#### Build in Docker (on arm64)


I tried it and it didn't work (2022-10-16), but possibly I just had old OS on target device.
It also gets same "mac-screen-capture-permissions" error as I, but can ignore. Was unstable when I tried regardless,
couldnt extract TAR archive, rxjs won't download, but maybe that's just how it is to build.
https://gitlab.com/undef1/signal-desktop-builder


https://github.com/0mniteck/Signal-Desktop-Builder



## Comments
If possible, just build signal-desktop according to below.

Possibly, at least for earlier versions, it was necessary to on a x86 machine (for me at least) to build ringrtc and then transfer that binary and use it in signal-desktop build. Also described later, below.

Note: If signal-desktop crashes when launched, make sure to have updated system. Debian bookworm worked, but old Debian bullseye did not.

(Note on arm board remember to properly cool device, if running max clock and building. Even with fan (no heatsink) the CPU reaches 80C on my board.)


# Prereqs

```
sudo apt install -y git git-lfs npm curl build-essential gcc make


# Install NVM (don't trust me that this is secure, but seems to be how you do it)
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.bashrc
nvm install 16.15.0 #this might change ofc, otherwise run nvm use in signal-desktop

#(is this how you cross compile?)
export npm_config_arch=arm64
sudo npm install --global yarn --arch=arm64
```

# Build Signal-Desktop

TODO: Although the binary build works (so you can use Signal-Desktop), I didn't manage to build .deb out of the box (which could make life easier). (Get error for 'fpm-..-linux-x86' which gives 'errorOut=i386-binfmt-P: .. '/lib/ld-linux.so.2': No such file or directory"'. ??? Try building on x86 or what? Try and compare with Docker builds?)

We will use, at time of writing latest beta v5.63.0-beta.3. I tried a few stable versions prior without luck, also a few (build) bugs were fixed in these latest betas (mainly, earlier had issues related to better_sqlite3 "error 'is_lvalue_reference_v' is not a member of 'std':" ... see electron issue 35193). 

I did not manage to fix build error for "mac-screen-capture-permissions" regarding unknown abi, even after trying to bump version in yarn.lock or "npm i node-abi" in the build directory. But then again IDK how it works. Well it seems to work regardless, ignore for now.

```
git clone https://github.com/signalapp/Signal-Desktop.git || echo
cd Signal-Desktop
git checkout v5.63.0-beta.3

#Some old fixes that aren't necessary?:
#  optional: Replace in package.json with own signal-ringrtc-node path (grep 'ringrtc') (built this on other machine)
#  optional: Replace in package.json with own better-sqlite3 path (grep 'better-sqlite3'), only difference is that they patch so it uses dynamically linked crypto lib instead of static (is this recommended to do?? Mentioned in other guide)
#  optional: patch to change node abi such as https://github.com/BernardoGiordano/signal-desktop-pi4/blob/master/yarn.lock.patch

git-lfs install
nvm use

yarn install --frozen-lockfile --arch=arm64 --network-timeout 600000
 
#Seems to be a few ways to finish up (TODO!), for now I did this (uncommented parts). To build .deb instead add "deb" after "--linux " but I got error on this right now!
#These build steps are stolen from https://forums.puri.sm/t/building-and-running-signal-desktop-on-the-librem-5/14999 are they good?
yarn generate
#yarn run-s --print-label build:grunt build:typed-scss build:webpack #didn't work, ok whatever ignore (TODO)
SIGNAL_ENV=production yarn build:electron --arm64 --linux --dir --config.directories.output=release
#or replace last command above with: `yarn build-release --arm64 --linux`

#Optionally, can sign release e.g. https://github.com/0mniteck/Signal-Desktop-Builder/blob/master/signal-buildscript.sh

#Binary if .deb: release/*.deb
#Binary if not: Signal-Desktop/release/linux-arm64-unpacked/signal-desktop


```


# Optional build steps (mainly ringrtc)

Some steps if build complains on sqlite or ringrtc. Hopefully not required on newest Signal versions.

Inspired from guides mentioned at start. First part is built on x86 (electron build is better supported) and second part on arm64 (some minor preparation of dependencies, and use binary from part 1).

NOTE! After these steps are finished, remember to change 'Signal-Desktop/package.json' to point to those dirs (briefly mentioned above in that step)
e.g.
```
-    "ringrtc": "https://github.com/signalapp/signal-ringrtc-node.git#7e1b508953b4f322f56fb4411da47a23eb115eb6",
+    "ringrtc": "~/ringrtc/signal-ringrtc-node/",
-    "better-sqlite3": "https://github.com/signalapp/better-sqlite3#a92f637708b41a478601c388f5a66223f766021b",
+    "better-sqlite3": "~/signal-ringrtc-node/better-sqlite3",
```

```
#part two, now do this on x86
function build_extras_one {
        ### get gclient for build ringrtc ###

        git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git || echo
        cd -
        export PATH="$PWD/depot_tools":"$PATH" #a bit scary but whatever
        gclient # make sure gclient works, will update depot_tools if not done yet (should be done in build process anyway?)

        ### ringrtc stuff ###
        
        sudo apt-get install -y  crossbuild-essential-arm64 build-essential git curl wget python3 python2.7 pkg-config
        
        #Get Rustup
        curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
        source "$HOME/.cargo/env"
        #prob not necessary
        #export RUSTUP_HOME="$HOME/.rustup"
        #export CARGO_HOME="$HOME/.cargo"
        
        git clone https://github.com/signalapp/ringrtc || echo
        #(need to have Rust and other dependencies to build ringrtc, just some standard stuff)
        cd ringrtc

        # for cross compile
        export CARGO_TARGET_AARCH64_UNKNOWN_LINUX_GNU_LINKER=aarch64-linux-gnu-gcc
        rustup target add aarch64-unknown-linux-gnu
        ringrtc/src/webrtc/src/build/linux/sysroot_scripts/install-sysroot.py --arch arm64
        
        # info here: https://github.com/signalapp/ringrtc/blob/main/BUILDING.md#electron-1
        #make clean
        make electron PLATFORM=unix NODEJS_ARCH=arm64
        #sometimes this gives socket timeout for https://registry.yarnpkg.com/rxjs/-/rxjs-6.6.7.tgz, very odd. It usually works by trying again, or maybe it's random....

        echo "Binary is ready"; echo
        cp src/node/build/linux/libringrtc-arm64.node ~/TO_ARM64_SYSTEM/ -vna
}

#part two, now do this on arm64
function build_extras_two {
        ### 1: put binary from part 1 into this repo ###
        
        git clone https://github.com/signalapp/signal-ringrtc-node || echo

        cd signal-ringrtc-node/
        
        #NOTE! use binary we built earlier!
        cp ~/TO_ARM64_SYSTEM/libringrtc-arm64.node build/linux/libringrtc-arm64.node
        
        #Don't think this is necessary, just copy binary as above, but can do this (described in https://github.com/signalapp/signal-ringrtc-node/issues/2#issuecomment-846995870)
        #cd src/node && scripts/copy_repo.sh . ../../../signal-ringrtc-node #copy built binaries into the signal-ringrtc-node repo folder
        #(signal-ringrtc-node is now ready)

        ### 2: get local better-sqlite3 and patch if problems with git-lfs or need patch (?) ###
        
        git clone https://github.com/signalapp/better-sqlite3 || echo
        cd better-sqlite3/
        # Patch (https://github.com/BernardoGiordano/signal-desktop-pi4/blob/master/better-sqlite3.patch)
        wget https://raw.githubusercontent.com/BernardoGiordano/signal-desktop-pi4/master/better-sqlite3.patch
        patch -Np1 -i better-sqlite3.patch
}

```
