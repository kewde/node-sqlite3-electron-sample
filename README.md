# node-sqlite3-electron-sample

This is a sample application that combines the following three tools:
* electron
* sqlite3
* electron-builder

## Swapping different electrons
Currently the version is `1.7.12`.
If you want to test multiple version then, edit package.json.
```
{
  "name": "node-sqlite3-electron-sample",
  "version": "1.0.0",
    ....
  "build": {
    "electronVersion": "1.7.12",
    ....
  },
  "devDependencies": {
    "electron": "1.7.12",
    ....
  }
}
```

Change electronVersion & the electron dependency.

```
yarn install --force
```


## Installation instructions
The following command will install all the dependencies.
Note that SQLite3 is a native dependency, this means we need platform specific binaries. 
There are two ways of obtaining these binaries:
* prebuilts (= compiled by someone else and retrieved using node-pre-gyp)
* compiled on your own system!

```
yarn run install
```

In the logs you should see the following:
```
$ node-pre-gyp install --fallback-to-build
node-pre-gyp info it worked if it ends with ok
node-pre-gyp info using node-pre-gyp@0.9.1
node-pre-gyp info using node@6.11.5 | linux | x64
node-pre-gyp http GET https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.0.0/electron-v1.7-linux-x64.tar.gz
node-pre-gyp http 403 https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.0.0/electron-v1.7-linux-x64.tar.gz
node-pre-gyp ERR! Tried to download(403): https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.0.0/electron-v1.7-linux-x64.tar.gz
node-pre-gyp ERR! Pre-built binaries not found for sqlite3@4.0.0 and electron@1.7.12 (electron-v1.7 ABI, glibc) (falling back to source compile with node-gyp)
node-pre-gyp http 403 status code downloading tarball https://mapbox-node-binary.s3.amazonaws.com/sqlite3/v4.0.0/electron-v1.7-linux-x64.tar.gz
```
This indicates that the prebuilts for sqlite for the target=electron and version=1.7.* were not found and the installation will fallback to compiling it.
You are unable to package you application into a .exe (Windows) on a Linux.

However, the installation will fallback to compiling sqlite3 for you.

to execute this test package:

```
yarn run electron
```

## Packaging your application

Make a .exe (for both 32bit and 64 bit).
Note: does not work on linux without prebuilts - it will just compile a linux 32 bit executable and complete.
When you execute it however on a windows machine it will fail with the error
`Error: %1 is not a valid Win32 application.`
```
yarn run package:win
```

Make a .deb & .zip (64 bit only):
```
yarn run package:linux
```

Make a .zip (can also do .dmg on OSX)
```
yarn run package:mac
```


## More on binaries and prebuilts
In our `package.json` we've added the following script:

```
    "postinstall": "install-app-deps"
```

This will trigger whenever `npm install`or `yarn install` is executed.
It will execute the following

```
    electron-builder install-app-deps
```
which in turns retrieves the right binaries for _all_ native dependencies.
Not all packages provide `prebuilts` - hence sometimes it will resort to compiling it on your own system.
Make sure you have gcc & build-essentials installed on your system (Linux) or the right .

**Super important note:**
As it stands, you can _NOT_ compile (node-)sqlite3 for Windows on Linux.
Currently there are also no prebuilts available for Windows.

