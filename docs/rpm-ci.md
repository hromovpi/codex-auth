# ALT RPM CI

The `RPM (ALT Sisyphus)` GitHub Actions workflow builds an x86_64 RPM and
SRPM on pushes and pull requests targeting `sisyphus`. It can also be started
manually from the Actions tab.

The build runs in the `alt:sisyphus` container with dependencies from the
Sisyphus repository. `rpmbuild -ba` runs the spec's `%check` section, including
the Zig test suite. This is a container build, not a Hasher build or submission
to the ALT package repository.

Sources are exported using `.gear/rules`. The current rules select the upstream
`v@version@` tag named by the spec and copy the packaging patches. Consequently,
the build tests that tagged release with the patches, rather than arbitrary
source changes in the checkout. Full Git history is fetched so Gear can resolve
the stored upstream tags.

After a successful run, download `codex-auth-sisyphus-x86_64` from the run's
Artifacts section. It contains the binary RPMs and source RPM. Artifacts follow
the repository's GitHub Actions retention settings; the workflow does not
publish a release or update an ALT repository.

The README badge points to the workflow in `hromovpi/codex-auth` on the
`sisyphus` branch. Update its repository and branch if the packaging moves.

The current project requires Zig 0.16.0. A p11 build needs a compatible
toolchain in its build repository or a source port to the Zig version provided
by p11. Building in Sisyphus does not establish p11 compatibility.
