# Installation and Build Documentation Policy

## Installation (end users)
Supported operating systems; real download or package sources; steps per platform;
required permissions; system dependencies; upgrade behaviour and data preservation;
uninstallation and leftover data; troubleshooting for observed problems. Do not claim that
installation works on a clean system unless it was validated; say which environments were
tested.

## Build (developers)
Required tools and versions; dependency installation with pinned or locked versions;
source preparation; build commands per platform with the shell indicated;
environment-specific requirements (names of variables only, never secrets); output
locations; build verification; known limitations.

Commands must match the actual build files. Building must never require access to
development-guidance repositories, AI tooling or private credentials, and build
documentation does not need to mention them. Keep installation and build instructions
separate and avoid duplicating them in the README.
