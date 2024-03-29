Available via [PDF](https://github.com/aMahanna/arangodb-macos-onboard/blob/main/arangodb-macos-onboard.pdf) (Latest)

### MacOS Developer Onboard

Objective: Establish an ArangoDB dev environment with CLion.

Prerequisites
- [macOS Big Sur](https://apps.apple.com/ca/app/macos-big-sur/id1526878132?mt=12)
- [Homebrew](https://brew.sh/)
- [Xcode](https://apps.apple.com/ca/app/xcode/id497799835?mt=12)
- Command Line Tools: `xcode-select --install`
- CLion 2021.2: `brew install --cask clion`
- OpenSSL 1.1: `brew install openssl@1.1`
- Node 16.*: `brew install node`
- Yarn 1.22.*: `brew install yarn`
- Python 3: `brew install python3` (or `brew update && brew upgrade python3`)
- Access to [arangodb/enterprise](https://github.com/arangodb/enterprise)

Clone the repositories.

```
git clone https://github.com/arangodb/arangodb
cd arangodb
git clone https://github.com/arangodb/enterprise # (if you have access)
```

Open CLion. From its start menu, import the `arangodb` repository via the "Open" button:

<img src="./img/4.png"  width="500"/>

Follow through the setup Wizard.
**Note:** You _can_ click on the `Use CMakeLists.txt` checkbox during one of the Wizard steps:

<img src="./img/4_2.png"  width="500"/>


From the CLion preferences menu (⌘ ,):

<img src="./img/5.png"  width="500"/>

1. Navigate to `Build, Execution, Deployment` --> `CMake`
2. Disable the Debug profile
3. Create a new profile (⌘N) with the following values:
   1. Name: `RelWithDebInfo`
   2. Build type: `RelWithDebInfo`
   3. Toolchain: `Use: Default`
   4. CMake options: ```-DCMAKE_OSX_DEPLOYMENT_TARGET=<YOUR MACOS BIG SUR VERSION HERE> -DCMAKE_BUILD_TYPE=RelWithDebInfo -DUSE_MAINTAINER_MODE=On -DUSE_FAILURE_TESTS=On -DUSE_ENTERPRISE=<On or Off, depending if you have access> -DUSE_JEMALLOC=Off -DOPENSSL_USE_STATIC_LIBS=On -DCMAKE_LIBRARY_PATH=/usr/local/Cellar/<YOUR OPENSSL 1.1 PATH>/lib -DOPENSSL_ROOT_DIR=/usr/local/Cellar/<YOUR OPENSSL 1.1 PATH> -DUSE_CCACHE=Off```
   5. Build directory: `build`
   6. Build options: `-j 12`
4. **Note:** Make sure to click on `Apply` or `OK` before exiting the Preferences menu



After saving these preferences, you should see a CMake process running. Wait for it to output `[Finished]`

If no CMake Process appeared:
1. Open the CMake tab (see the toolbar containing tabs such as `Git`, `TODO`, & `Terminal`)
2. Click on the Settings Wheel --> Reset Cache and Reload Project

<img src="./img/6.png"  width="500"/>


This would be a good place to restart the IDE if you have any doubts.


With the `arangodb` configuration selected (see the dropdown in the top toolbar), build the project (⌘F9):

<img src="./img/8.png"  width="500"/>

Common errors for first-time-builds:
1. `TypeError: cannot use a string pattern on a bytes-like object` (happens at around the 33% build mark)
   1. **Fix**: Replace all instances of `r'` with `rb'` in the `ExecFilterLibtool` function in `3rdParty/V8/gyp/buildtime_helpers/mac_tool.py`
   2. **Reason**: Binary read must be specified for Python 3.9.7+
2. `TODO` --- Place more common errors here


Run ArangoDB via terminal with `./build/bin/arangod myDatabaseDirectory`:

<img src="./img/10.png"  width="500"/>

Run ArangoDB via CLion by setting up the `arangodb` configuration:

<img src="./img/11.png"  width="500"/>

1. Click on the top toolbar dropdown --> `Edit Configurations`
2. Edit the `arangodb` configuration with the following values:
   1. Name: `arangodb`
   2. Target: `arangodb`
   3. Executable: `<your_path_to>/arangodb/build/bin/arangod`
   4. Program arguments: `myDatabaseDirectory`


**At this point, you should be able to access `localhost:8529`.** 
