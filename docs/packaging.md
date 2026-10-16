# AppStream metadata

WiVRn installs two AppStream components:

| Component ID | Type | Executable | Fedora package |
| --- | --- | --- | --- |
| `io.github.wivrn.wivrn` | Desktop application | `wivrn-dashboard` | `wivrn-dashboard` |
| `io.github.wivrn.wivrn.server` | Console application | `wivrn-server` | `wivrn` |

The desktop component keeps the same ID as the Flatpak so software centers can
associate the native and Flatpak versions with the same application. The server
component describes headless installations and is installed only when
`WIVRN_BUILD_SERVER` is enabled. It does not create a second desktop launcher.

Distribution packages should include each component's metadata in the package
that owns its executable. In Fedora's RPM spec, keep
`%{_metainfodir}/io.github.wivrn.wivrn.metainfo.xml` in `%files -n %{name}-dashboard`
and add `%{_metainfodir}/io.github.wivrn.wivrn.server.metainfo.xml` to the main
`%files` section. Package names are supplied by the distribution's AppStream
catalog generator, rather than hardcoded in upstream metadata.

## Validation

Run from the source root with AppStream installed:

```sh
appstreamcli validate --no-net dashboard/io.github.wivrn.wivrn.metainfo.xml
appstreamcli validate --no-net server/dist/io.github.wivrn.wivrn.server.metainfo.xml
```

## Missing native packages in software centers

An RPM being available through DNF does not guarantee that Discover or GNOME
Software can list it. Software centers also need a distribution AppStream catalog
entry associating the application ID with its package. Fedora distributes this
catalog in `appstream-data`.

If the WiVRn desktop entry is missing from that catalog, Fedora needs to regenerate
and publish it from the packaged metadata. Installing or refreshing upstream
metainfo files alone does not repair a missing distribution catalog entry.
Software centers may also choose not to display console applications; the
dashboard is the graphical entry point for installing WiVRn.
