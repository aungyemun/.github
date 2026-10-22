# Installer, Packaging and Upgrade Safety

Engineering policy for installers, packages and application lifecycle behaviour of
desktop applications on Windows, Linux and macOS: detecting previous installations,
in-place upgrades, safe file replacement, user-data protection, running-process and
concurrency handling, shortcuts and launchers, duplicate prevention, single-instance
execution, uninstall registration, failure recovery, silent installation and installer
testing.

**When it applies:** whenever a task creates or changes installer, packaging, update or
application-lifecycle code (installer *development* does not require an established
release process). Building, signing and publishing a release additionally
follow [RELEASES](RELEASES.md). It does not apply to
libraries, command-line tools distributed only through a language package manager, or
repositories without installer or packaging code.

Use the project's existing installer technology (e.g. Inno Setup, WiX/MSI, MSIX, NSIS,
Squirrel/Velopack, DEB/RPM, Flatpak, AppImage, DMG/PKG) and its native mechanisms rather
than hand-written replacements. Where the technology cannot provide a requirement below,
say so and document the limitation instead of claiming it.

Related: user data ([PRIVACY](PRIVACY.md) sections 9 and 11), privileges and signing
([SECURITY](SECURITY.md)), dependencies and bundled runtimes
([DEPENDENCIES](DEPENDENCIES.md)), tests and reporting ([TESTING](TESTING.md)),
installation documentation
([INSTALLATION_AND_BUILD_POLICY](documentation/INSTALLATION_AND_BUILD_POLICY.md)).

## 1. Stable identity (Mandatory safeguard)

Every installable application has stable identifiers that never change between versions:
application and installer identity (e.g. Inno Setup `AppId`, MSI `UpgradeCode`, MSIX
package family name), package name (DEB/RPM), application ID (reverse-DNS for Flatpak
and desktop entries), bundle identifier (macOS `CFBundleIdentifier`), uninstall
registration key, shortcut names and Start Menu group. Never derive these from the
version number (version formats per platform: [VERSION_MANAGEMENT](VERSION_MANAGEMENT.md)
section 7). Changing an identity is a breaking change that needs explicit
authorization and a migration plan, because it turns upgrades into side-by-side
installations.

## 2. Detect previous installations

Before installing, determine from verified installation metadata - not directory names
alone:
1. whether a compatible previous installation of **this** application exists (match the
   stable identity, never a similar name);
2. its version, installation directory and scope (per-user or all-users);
3. its registered uninstaller;
4. its managed shortcuts and file associations;
5. whether the application is running.

On Windows use the installer's own registration and the uninstall registration
(`...\CurrentVersion\Uninstall\<id>` under HKCU or HKLM, including the 32-bit view where
relevant). Handle missing, damaged or partial records safely: report them, do not guess,
and never delete anything because a record is inconsistent.

## 3. Upgrade in place

- The default for a compatible previous installation is an in-place upgrade into the
  existing directory and scope. Never create a second directory because the version
  changed (`App 2`, `App New`, `App v1.2.0`).
- Use another destination only when the user explicitly selects a supported one.
  Choosing a custom directory is not permission to delete the previous installation
  elsewhere; migration or removal of the old copy needs its own confirmation.
- Respect scope and permissions: a per-user upgrade does not silently become an
  all-users installation, or the reverse.

## 4. Replace application files safely

1. Determine which files belong to the previous installation (installer manifest,
   component tables, package file lists); everything else is presumed user-owned.
2. Protect user data first (section 5).
3. Use a recoverable process: the installer's transactional or rollback support,
   staged replacement (install alongside, then switch), or a backup of replaced files.
4. Replace binaries, bundled runtimes and resources; remove obsolete files only when
   they are verifiably application-owned.
5. Verify the installed version and that the application starts.

Never delete the installation directory wholesale, unrelated user files, or shared
components owned by other applications (shared runtimes, redistributables, system
libraries). A failed upgrade must never silently leave a partially replaced
installation.

## 5. Protect user data (Mandatory safeguard)

Preserve user documents, settings and preferences, databases, scientific datasets,
exported results, user-installed plugins and licence information. Do not assume that
anything inside the installation directory is disposable: older versions may have
stored data there - identify and protect it before removing program files. Data
migrations are tested on copies, validated after running, and keep a recovery path
(e.g. a backup of the old data). Uninstallation leaves user data in place unless the
user explicitly chooses to remove it, and says where it remains.

## 6. Running application and locked files

Before replacing files: detect running instances and relevant helper or background
processes; ask the application to close gracefully and give the user the chance to save
work (e.g. Windows Restart Manager, Inno Setup `CloseApplications` / `AppMutex`, WiX
close-application support); wait appropriately; confirm the files are no longer locked.
Never force-terminate the application in a way that discards unsaved data. If it cannot
be closed safely, stop with a clear message or use an explicitly supported deferred
replacement (e.g. replace on restart).

## 7. One installer at a time

Prevent two installers for the same application from modifying it concurrently, with an
installer-level mutex or lock or the platform's native facility (e.g. Inno Setup
`SetupMutex`, the Windows Installer service's own serialisation, package-manager locks on
Linux). When another installation is running, detect it, explain it where interactive,
and exit or wait according to the installer's established behaviour - never modify files
concurrently.

## 8. Shortcuts and launchers

- **Desktop shortcuts:** update managed shortcuts to the new executable; keep the user's
  earlier choices (do not add a shortcut the user opted out of); never create duplicates;
  remove only verified obsolete managed shortcuts; never touch user-created shortcuts.
  Shortcut names carry no version number unless the project's convention requires it.
- **Windows Start Menu:** keep the established group and scope (per-user or all-users);
  update application and uninstall shortcuts; remove verified obsolete ones; no duplicate
  groups.
- **Linux:** update managed `.desktop` entries under the established application ID;
  no duplicate menu entries; follow the package manager's ownership of files.
- **macOS:** keep the bundle identity and an unversioned bundle name so that an upgrade
  replaces the application instead of adding a second copy; follow the DMG, PKG or
  updater replacement behaviour.

Do not impose Windows conventions on Linux or macOS. Icon assets for shortcuts, launchers
and installers: [LOGO_AND_BRANDING](LOGO_AND_BRANDING.md) section 6.

## 9. Duplicate installations

Prevent unintended side-by-side installations of the same application through stable
identity, installer and package identity, registration, upgrade detection and uninstall
registration. Where per-user and all-users installations can coexist, detect and report
the conflict. Never remove another user's installation, or an installation in another
directory, unless it is positively identified and its removal or migration is authorized.

## 10. Single-instance execution (only where intended)

For applications designed to run one instance per user session:
- use a platform-native mechanism scoped to the user session (e.g. a Windows named mutex
  in the session namespace plus IPC, a D-Bus unique name or a lock in the user's runtime
  directory on Linux, the standard application model on macOS, or the UI toolkit's
  facility such as `QLocalServer`/`QLockFile` or `GApplication`);
- on a second launch, focus the existing window and forward files or command-line
  requests when supported;
- survive crashes: no stale lock may block startup permanently (detect dead owners);
- avoid races during simultaneous launches; never block other users' instances.

Do not impose single-instance behaviour on applications that legitimately support
several independent instances.

## 11. Uninstall registration

After an upgrade the operating system shows exactly one entry with the current version,
correct publisher metadata and a working uninstall command; obsolete records of the same
application are removed only when verified; unrelated registrations are never touched;
the upgraded version uninstalls cleanly, and a reinstall afterwards works.

## 12. Failure recovery

Design for interruption during detection, shutdown, file replacement, shortcut and
registration updates, data migration and final verification. Restore the previous
working installation when an upgrade fails, where the technology allows. Otherwise fail
safely: keep user data, leave a recoverable state, and give accurate repair
instructions. Never report success for an incomplete installation.

## 13. Silent installation

Where supported, silent mode applies the same rules: previous-installation detection,
upgrade location, duplicate prevention, documented command-line options, safe handling of
running applications, meaningful exit codes, a log, preserved user data and recovery.
Silent mode never removes other installation scopes or unrelated applications.

## 14. Platform technology

- **Windows:** installer-native upgrade detection (e.g. `AppId`/`UpgradeCode` with a major
  upgrade), file-in-use handling, shortcut management, uninstall registration and
  installer mutexes; per-user installation without administrator rights where intended.
- **Linux:** DEB/RPM upgrades through the package manager (maintainer scripts must be
  idempotent and must not delete user data); Flatpak by application ID. AppImage and
  portable archives have no installer: document how they are replaced and removed. Never
  let two package managers own the same files.
- **macOS:** stable bundle identity; replacement through the supported mechanism (drag
  replacement, PKG upgrade, or an updater framework); user data outside the bundle
  (`~/Library/Application Support`, preferences); signing and notarization requirements
  respected ([SECURITY](SECURITY.md)).

## 15. Installer tests

For installer changes, test the relevant scenarios in clean virtual machines or
equivalent isolated environments - never against a production installation or valuable
user data:

| Area | Scenarios |
| --- | --- |
| Installation and upgrade | fresh install; same-directory upgrade; existing installation with a custom destination; existing installation in a different scope; offline installation; Unicode installation paths and user names |
| Running application and concurrency | application running during upgrade; two installers at once; double-click launch twice; simultaneous launches; startup after a crash |
| Shortcuts and registration | existing desktop shortcut; existing Start Menu shortcut; shortcut previously declined; uninstall registration; no duplicate installed-applications entries |
| Files and data | obsolete managed files removed; user documents, settings and databases preserved |
| Failure | interrupted installation; failed upgrade and recovery |
| Lifecycle | silent installation; uninstall after upgrade; reinstall after uninstall |

Report which scenarios were executed, on which OS versions, and which were not run
(and why) - see [TESTING](TESTING.md). Scenarios that do not apply to the installer
technology are listed as not applicable, not as passed.

## 16. Documentation

When an application supports in-place upgrades, its installation documentation
describes the implemented behaviour only: existing-installation detection, default
upgrade destination, custom destination behaviour, running-application handling, data
preservation, shortcut replacement, uninstallation, silent options, recovery from failed
upgrades and known limitations
([INSTALLATION_AND_BUILD_POLICY](documentation/INSTALLATION_AND_BUILD_POLICY.md)). Never document behaviour
the installer does not implement, and change application documentation only within an
authorized task.
