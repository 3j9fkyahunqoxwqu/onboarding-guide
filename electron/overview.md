# Electron

Electron is primarily comprised of three languages for its externally-facing functionality: JavaScript, C++, and Objective-C. At its core, Electron doesn't remove the need for platform-specific functionality to be written in a given system languages, it just obfuscates the system languages away from you so that you, a developer, to focus on a single simplified API surface area in JavaScript.

## Getting Around the Source Code

```diff
Electron
├── atom/ - C++ source code.
|   ├── app/ - System entry code.
|   ├── browser/ - The frontend including the main window, UI, and all of the
|   |   |          main process things. This talks to the renderer to manage web
|   |   |          pages.
|   |   ├── ui/ - Implementation of UI stuff for different platforms.
|   |   |   ├── cocoa/ - Cocoa specific source code.
|   |   |   ├── win/ - Windows GUI specific source code.
|   |   |   └── x/ - X11 specific source code.
|   |   ├── api/ - The implementation of the main process APIs.
|   |   ├── net/ - Network related code.
|   |   ├── mac/ - Mac specific Objective-C source code.
|   |   └── resources/ - Icons, platform-dependent files, etc.
|   ├── renderer/ - Code that runs in renderer process.
|   |   └── api/ - The implementation of renderer process APIs.
|   └── common/ - Code that used by both the main and renderer processes,
|       |         including some utility functions and code to integrate node's
|       |         message loop into Chromium's message loop.
|       └── api/ - The implementation of common APIs, and foundations of
|                  Electron's built-in modules.
├── chromium_src/ - Source code copied from Chromium. See below.
├── default_app/ - The default page to show when Electron is started without providing an app.
├── docs/ - Documentations.
├── lib/ - JavaScript source code.
|   ├── browser/ - Javascript main process initialization code.
|   |   └── api/ - Javascript API implementation.
|   ├── common/ - JavaScript used by both the main and renderer processes
|   |   └── api/ - Javascript API implementation.
|   └── renderer/ - Javascript renderer process initialization code.
|       └── api/ - Javascript API implementation.
├── native_mate/ - A fork of Chromium's gin library that makes it easier to marshal
|                  types between C++ and JavaScript.
├── spec/ - Automatic tests.
└── BUILD.gn - Building rules of Electron.
```

Any given module is either a **browser** module (meaning it runs in the main process only), a **renderer** module (renderer process only), or a **common** module, meaning it can run in both the browser and renderer processes.

`lib/` and `atom/` generally mirror each other in directory structure, and are where the modules themselves are implemented. By knowing what type a module is, you can use this knowledge to find out where it lives.
* Example: the `Menu` module
  * This is a **main** process module, so we can expect that if we navigate to `lib/browser/api` that it will exist there as a top-level js file.
    * We also know, thus, that we can find its native implementation files inside `atom/browser/api`!