---
layout: default
toc_levels: 1234
title: "Puppet 5.5 Release Notes"
---

This page lists the changes in Puppet 5.5 and its patch releases. You can also view [known issues](known_issues.html) in this release.

Puppet's version numbers use the format X.Y.Z, where:

-   X must increase for major backward-incompatible changes
-   Y can increase for backward-compatible new functionality or significant bug fixes
-   Z can increase for bug fixes

## If you're upgrading from Puppet 4.x

Read the [Puppet 5.0.0 release notes](/puppet/5.0/release_notes.html#puppet-500), because they cover breaking changes since Puppet 4.10.

Read the [Puppet 5.1](../5.1/release_notes.html), [Puppet 5.2](../5.2/release_notes.html), [Puppet 5.3](../5.3/release_notes.html), and [Puppet 5.4](../5.4/release_notes.html) release notes, because they cover important new features and changes since Puppet 5.0.

Also of interest: the [Puppet 4.10 release notes](../4.10/release_notes.html) and [Puppet 4.9 release notes](../4.9/release_notes.html).

## Puppet 5.5.3

Released July 17, 2018.

This is a bug-fix release of Puppet.

-   [All issues resolved in Puppet 5.5.3](https://tickets.puppetlabs.com/issues/?jql=fixVersion%20%3D%20%27PUP%205.5.3%27)

### Bug fixes

-   The `selmodule` type in Puppet 5.5.3 checks module names more strictly when determining whether a module has already been loaded. Specifically, its search wasn't anchored to the start of the module name in previous versions of Puppet, resulting in modules whose only difference in name is a prefix (such as "motd" and "mymotd") falsely reporting which module is loaded. Puppet 5.5.3 resolves this issue. ([PUP-8943](https://tickets.puppetlabs.com/browse/PUP-8943))

-   When resources fail to restart when notified from another resource, Puppet 5.5.3 now flags them as failed and reports them as such. Reports also now include the [`failed_to_restart` status](./format_report.html) for individual resources. This change also increments the report format version to 10. ([PUP-8908](https://tickets.puppetlabs.com/browse/PUP-8908))

-   When `config_version` refers to an external program, the last run summary in Puppet 5.5.3 no longer includes the Puppet-specific class name in YAML output. This is designed to make the YAML output easier to work with when using other YAML-processing tools. ([PUP-8767](https://tickets.puppetlabs.com/browse/PUP-8767))

-   When previous versions of Puppet cleared the environment cache, associated translation domains lingered until they were replaced the next time the environment was used. Environments that were never used again would still consume memory required for the translations, resulting in a memory leak. Puppet 5.5.3 resolves this issue by releasing translation domains when the related cached environment is purged. ([PUP-8672](https://tickets.puppetlabs.com/browse/PUP-8672))

-   Since Puppet 4.10.9, non-existent Solaris SMF services reported a state of `:absent` instead of `:stopped`. This change could break some workflows, so the behavior has been reverted in this release to report non-existent Solaris SMF services as `:stopped`. ([PUP-8262](https://tickets.puppetlabs.com/browse/PUP-8262))

### Regression fixes

-   The introduction of `multi_json` in Puppet 5.5.0 broke the JSON log destination. Puppet 5.5.3 fixes this regression. ([PUP-8773](https://tickets.puppetlabs.com/browse/PUP-8773))

### New features

-   The new [`puppet device --facts`](puppet_device.html) command allows you to display facts on a device, much in the same way that the `puppet facts` command behaves on other agents. ([PUP-8699](https://tickets.puppetlabs.com/browse/PUP-8699))

### Deprecations

-   The `puppet module build` command is deprecated in Puppet 5.5.3 and will be removed in a future release. To build modules and submit them to the Puppet Forge, use the [Puppet Development Kit](https://puppet.com/download-puppet-development-kit). ([PUP-8762](https://tickets.puppetlabs.com/browse/PUP-8762))

-   The `--configprint` flag is deprecated in Puppet 5.5.3. When running `puppet <SUBCOMMAND> --configprint` with the `debug` or `verbose` flags, Puppet outputs a deprecation warning. Use `puppet config <SUBCOMMAND>` to output setting values. ([PUP-8712](https://tickets.puppetlabs.com/browse/PUP-8712))

-   Puppet has long verified absolute paths across platforms with `Puppet::Util.absolute_path?(path)`. However, it now uses Ruby's built-in methods to accomplish this. Puppet 5.5.3 therefore deprecates `Puppet::Util.absolute_path?(path)` in favor of `Pathname.new(path.to_s).absolute?`. ([PUP-7407](https://tickets.puppetlabs.com/browse/PUP-7407))

## Puppet 5.5.2

Released June 7, 2018.

This is a bug-fix and security release of Puppet.

-   [All issues resolved in Puppet 5.5.2](https://tickets.puppetlabs.com/issues/?jql=fixVersion%20%3D%20%27PUP%205.5.2%27)

### Bug fixes

-   In previous versions of Puppet, attempting to create a Numeric type from the String "0" would result in an error. Puppet 5.5.2 resolves this issue. ([PUP-8703](https://tickets.puppetlabs.com/browse/PUP-8703))

-   When running Puppet on Ruby 2.0 or newer, Puppet would close and reopen HTTP connections that were idle for more than 2 seconds, causing increased load on Puppet masters. Puppet 5.5.2 ensures that the agent always uses the `http_keepalive_timeout` setting when determining when to close idle connections. ([PUP-8663](https://tickets.puppetlabs.com/browse/PUP-8663))

-   When using the [`--freeze_main` option](./configuration.html#freezemain) in previous versions of Puppet, certain circumstances could result in an error even if no code being loaded modified the main loaded logic. Puppet 5.5.2 resolves this issue. ([PUP-8637](https://tickets.puppetlabs.com/browse/PUP-8637))

-   Previous versions of Puppet did not allow aliased (custom) data types or Variant types to be used with the `match()` function. Puppet 5.5.2 removes this limitation. ([PUP-8745](https://tickets.puppetlabs.com/browse/PUP-8745))

### Security fixes

-   On Windows, Puppet no longer includes `/opt/puppetlabs/puppet/modules` in its default basemodulepath, because unprivileged users could create a `C:\opt` directory and escalate privileges. ([PUP-8707](https://tickets.puppetlabs.com/browse/PUP-8707))

### New features

-   Puppet 5.5.2 can accept globs in the path name for the `modulepath` as defined in `environment.conf`. ([PUP-8556](https://tickets.puppetlabs.com/browse/PUP-8556))

-   Puppet 5.5.2 simplifies the logic for resolving resources' types in reports. ([PUP-8746](https://tickets.puppetlabs.com/browse/PUP-8746))

### Deprecations

-   Puppet 5.5.2 issues a deprecation warning when explicitly using a checksum value in the content property of a file resource. ([PUP-7534](https://tickets.puppetlabs.com/browse/PUP-7534))

## Puppet 5.5.1

Released April 17, 2018.

This is a feature and bug-fix release of Puppet.

-   [All issues resolved in Puppet 5.5.1](https://tickets.puppetlabs.com/issues/?jql=fixVersion%20%3D%20%27PUP%205.5.1%27)

### Deprecations

-   Ruby versions older than 2.3 are deprecated in Puppet 5.5.1 and will be removed in Puppet 6. Puppet issues warnings when using older versions of Ruby.	([PUP-8504](https://tickets.puppetlabs.com/browse/PUP-8504))

-   Puppet 5.5.1 removes the deprecation of `empty(undef)` introduced in Puppet 5.5.0. ([PUP-8623](https://tickets.puppetlabs.com/browse/PUP-8623))

### Bug fixes

-   When reporting metrics on the time of a Puppet run, previous versions of Puppet instead reported the sum of other run times. Puppet 5.5.1 reports the measured time of the run. ([PUP-6344](https://tickets.puppetlabs.com/browse/PUP-6344))

-   If the production environment did not exist when running Puppet, previous versions would create the directory as the user `root` and group `root` instead of the service account if one is available. Puppet 5.5.1 sets the owner and group to the service account if it exists on the node. ([PUP-6996](https://tickets.puppetlabs.com/browse/PUP-6996))

-   The `yumrepo` provider in previous versions of Puppet attempted to run `stat` on non-existent repository files when a repository file not being managed by a `yumrepo` resource was deleted. This led to an error on the first attempt at running Puppet. In Puppet 5.5.1, `yumrepo` ensures the file exists before attempting to run `stat`. ([PUP-8421](https://tickets.puppetlabs.com/browse/PUP-8421))

-   On AIX, Puppet 5.5.1 correctly manages users on the latest AIX service packs. ([PUP-8538](https://tickets.puppetlabs.com/browse/PUP-8538))

-   The `augeas` provider in previous versions of Puppet did not properly unescape quotes in quoted arguments for `set` and similar commands, resulting in escaping backslashes appearing in output. Puppet 5.5.1 correctly removes those escaping backslashes. ([PUP-8561](https://tickets.puppetlabs.com/browse/PUP-8561))

-   Previous versions of Puppet overpopulated the context stack with the server version, which drastically increased the time it took to parse the context stack for every request due to a massive amount of redundant data. Puppet 5.5.1 doesn't overpopulate the stack with duplicate information. ([PUP-8562](https://tickets.puppetlabs.com/browse/PUP-8562))

-   Extra information for `puppet config print` is shown only when passing the `verbose` or `debug` options in Puppet 5.5.1. ([PUP-8566](https://tickets.puppetlabs.com/browse/PUP-8566))

-   When parsing EPP templates containing CRLF line breaks, previous versions of Puppet did not generate files containing CRLF line breaks, which could cause issues when using EPP templates on platforms such as Windows that expect them. (ERB templates were not affected.) Puppet 5.5.1 correctly passes CRLF line breaks in EPP templates to the generated output. ([PUP-8240](https://tickets.puppetlabs.com/browse/PUP-8240))

### New features

-   When forming relationships between resources by using an invalid resource reference, the error message in Puppet 5.5.1 includes the source location. ([PUP-8498](https://tickets.puppetlabs.com/browse/PUP-8498))

-   In previous versions of Puppet, the `yumrepo` type limited priority values to a range fom 1 to 99. Puppet 5.5.1 now accepts any positive or negative integer, which matches the behavior of `yum` when determining valid priority values. ([PUP-4678](https://tickets.puppetlabs.com/browse/PUP-4678))

-   Previous versions of Puppet applied the `tidy` resource to all files that it found, even when Puppet managed the files. The `tidy` resource in Puppet 5.5.1 skips any files that are managed by Puppet when deciding if it should remove files. ([PUP-7307](https://tickets.puppetlabs.com/browse/PUP-7307))

-   SystemD is the new default provider for Ubuntu 17.04 and 17.10. ([PUP-8495](https://tickets.puppetlabs.com/browse/PUP-8495))

## Puppet 5.5.0

Released March 20, 2018.

This is a feature and bug-fix release of Puppet.

-   [All issues resolved in Puppet 5.5.0](https://tickets.puppetlabs.com/issues/?jql=fixVersion%20%3D%20%27PUP%205.5.0%27)

### Bug fixes

-   When processing malformed plist files, previous versions of Puppet used `/dev/stdout`, which can cause Ruby to report warnings. Puppet 5.5 instead uses `-`, which uses stdout when processing the plist file with `plutil`. ([PUP-8545](https://tickets.puppetlabs.com/browse/PUP-8545))

-   Previous versions of Puppet might incorrectly report an error that a match expression had no effect, if for instance numeric match variables could be set as a side-effect of evaluating the match. For example, this valid code would produce an error:

    ```puppet
    'cdf' =~ /^([a-z])(.*)/
    notice($2)
    ```

    Puppet 5.5.0 resolves the issue. ([PUP-8519](https://tickets.puppetlabs.com/browse/PUP-8519))

-   The `puppet lookup` command-line tool called the external node classifier (node terminus) even if the `--compile` flag was not enabled. This could cause errors, because Puppet would load classes indicated by the ENC without a complete and proper setup, or if loaded code was had parse errors. In Puppet 5.5.0, the configured ENC is used only if the `--compile` flag is enabled. ([PUP-8502](https://tickets.puppetlabs.com/browse/PUP-8502))

-   If selinux bindings were not available in previous versions of Puppet, it would try and fail to manage a setting because it could not read its current state. In Puppet 5.5.0, if no selinux bindings are available, Puppet doesn't try to read the setting's current inaccessible state. ([PUP-8477](https://tickets.puppetlabs.com/browse/PUP-8477))

-   The `puppet parser dump` output format in previous versions of Puppet produced output with an initial lowercase letter for names of types, when it should have used an initial uppercase letter. Puppet 5.5.0 resolves the issue. ([PUP-8474](https://tickets.puppetlabs.com/browse/PUP-8474))

-   Since Puppet 5.4.0, Puppet uses the `lusermod` command instead of `usermod` when setting `forcelocal => true` on a user resource in *nix. Puppet also typically manages group membership via the user resource. However, unlike `usermod`, the `lusermmod` command cannot manage group membership. Therefore, Puppet 5.4.0 couldn't manage group membership in a user resource.

    Puppet 5.5.0 calls `usermod` only when trying to manage group membership for a user resource. In some situations, such as attempting to add a user to a NIS or LDAP group, the command might still fail. However, this behavior is consistent with versions of Puppet prior to 5.4.0. ([PUP-8470](https://tickets.puppetlabs.com/browse/PUP-8470))

-   Puppet 5.5.0 should no longer log warnings resulting from inadvisable coding practices, such as using ambiguous arguments, to the process's `stderr`. This resolves an issue in previous versions of Puppet where log managers could cause a broken pipe. ([PUP-8467](https://tickets.puppetlabs.com/browse/PUP-8467))

-   In a custom Node terminus, previous versions of Puppet allowed you to construct the Node object where `$::environment` would be empty during catalog compilation, even though the Node object had a properly set environment. In Puppet 5.5.0, catalog compilation now consults the node's environment directly when setting `$::environment`. ([PUP-8443](https://tickets.puppetlabs.com/browse/PUP-8443))

-   The data types Timespan and Timestamp incorrectly set the upper bounds to the same as lower bounds when created with a single parameter. For example, `T[x]` was interpreted as `T[x,x]` instead of `T[x, <no-limit>]`. Puppet 5.5.0 resolves this by assuming no upper bound when passed a single parameter. ([PUP-8439](https://tickets.puppetlabs.com/browse/PUP-8439))

-   While previous versions of Puppet could create new Windows groups containing virtual accounts, it couldn't manage groups that contained at least one virtual account. Puppet might also have been unable to correctly manage groups with account names that appeared in both the local computer and a domain, due to a failure to properly disambiguate the accounts. Puppet 5.5.0 resolves both problems. ([PUP-8231](https://tickets.puppetlabs.com/browse/PUP-8231))

-   The `puppet config` command in Puppet 5.5.0 behaves better when a section is not specified, and resolves bugs that could cause settings to be set in the wrong section or resulted in duplicate sections. ([PUP-7542](https://tickets.puppetlabs.com/browse/PUP-7542))

-   The `--render-as` flag for `puppet config print` can now produce appropriately structured formatted output with the `json` and `yaml` options. The default format is unchanged. ([PUP-8188](https://tickets.puppetlabs.com/browse/PUP-8188))

-   When running `puppet config print` in Puppet 5.5.0, the `environment_timeout` setting prints `unlimited` when set to 'unlimited', and prints the configured `environment_timeout` when no environment exists to set this value. ([PUP-8409](https://tickets.puppetlabs.com/browse/PUP-8409))

-   The `puppet config print`, `set`, and `delete` commands now print the environment and section on stderr to reduce user confusion. The `puppet config print` command also warns the user if they do not specify a section. ([PUP-2868](https://tickets.puppetlabs.com/browse/PUP-2868))

-   Previous versions of Puppet produced warnings or errors when managing Windows local groups that contained unresolvable SIDs from previously valid domain members that had since been deleted. Puppet 5.5.0 safely handles these unresolvable SIDs inside of groups. ([PUP-7326](https://tickets.puppetlabs.com/browse/PUP-7326))

-   The `yumrepro` provider in Puppet 5.5.0 trims the leading and trailing spaces from the values it finds when reading a YUM Repository Configuration File, instead of returning an error. ([PUP-6639](https://tickets.puppetlabs.com/browse/PUP-6639))

-   Updated `yumrepo` provider descriptions for the `exclude` and `includepkg` field now explain that the properties should be set to a string containing a space-separated list of package names or shell globs. ([PUP-2884](https://tickets.puppetlabs.com/browse/PUP-2884))

-   The `yumrepo` provider in previous versions of Puppet overwrote repository configurations that weren't being managed by a `yumrepo` resource. Puppet 5.5.0 resolves this issue by checking for any unmanaged repository configurations before writing to the yumrepo config file. ([PUP-723](https://tickets.puppetlabs.com/browse/PUP-723))

### Known issues

-   The `yumrepo` provider contains the new property `target`. The property will be enabled in a future release and should not be used. ([PUP-8542](https://tickets.puppetlabs.com/browse/PUP-8542))

-   In previous versions of Puppet, if Hiera read a YAML data file and the result was neither a hash nor completely empty, Hiera issued a warning. In Puppet 5.5, if the `--strict=error` flag is enabled, Hiera will instead produce an error if the file was read by the built-in YAML or eYAML backend functions. If the `--strict` flag is set to `warning` or `off`, Hiera issues a warning as before.

    Note that Ruby's YAML parser does not fully comply with the YAML spec, and some faulty YAML files can still be loaded with unexpected results instead of errors. See [PUP-8547](https://tickets.puppetlabs.com/browse/PUP-8547) for details. ([PUP-8541](https://tickets.puppetlabs.com/browse/PUP-8541))

### Improvements

-   If a heredoc used an empty end tag (`@("")`), Puppet reported a Ruby NameError. Puppet 5.5.0 instead reports an error stating that the tag is empty. ([PUP-8519](https://tickets.puppetlabs.com/browse/PUP-8519))

-   The `puppet help` and `puppet man` commands now print helpful error messages. ([PUP-8444](https://tickets.puppetlabs.com/browse/PUP-8444), [PUP-8464](https://tickets.puppetlabs.com/browse/PUP-8464))

-   In Puppet 5.5.0, the `puppet cert clean` command can clean certificates even if none of the certificates in the provided list have already been signed. ([PUP-8448](https://tickets.puppetlabs.com/browse/PUP-8448))

-   Puppet 5.5.0 restores the ability to upload facts to Puppet Server and other Puppet masters, as well as the `puppet facts upload` command, which is important for Direct Puppet workflows when agents always run from cached catalogs and need an alternate mechanism to upload facts. It also updates the default legacy `auth.conf` to allow agents to only upload their own facts. ([PUP-8232](https://tickets.puppetlabs.com/browse/PUP-8232)

-   Time metrics recorded in the run report now include the time it takes to apply the catalog, convert the catalog, plugin sync, generate facts, retrieve nodes, and evaluate transactions. ([PUP-920](https://tickets.puppetlabs.com/browse/PUP-920), [PUP-6343](https://tickets.puppetlabs.com/browse/PUP-6343))

### New features

-   The `flatten()`, `empty()`, `join()`, `keys()`, `values()`, and `length()` functions have been promoted from the `stdlib` module to Puppet.

    Additionally, the Puppet `length()` function adds support for returning the length of a Binary value, and the `empty()` function supports answering if a Binary value is empty (has zero bytes).

    Puppet's implementations take precedence over `stdlib` if you use a version of the module containing them, and maintain compatibility with their `stdlib` implementations. As a result, you should not need to change any Puppet code relying on these functions. However, note that the Puppet implementation of `empty()` deprecates being called with an undef or numeric value and will issue a warning. ([PUP-8492](https://tickets.puppetlabs.com/browse/PUP-8492), [PUP-8495](https://tickets.puppetlabs.com/browse/PUP-8495), [PUP-8497](https://tickets.puppetlabs.com/browse/PUP-8497), [PUP-8502](https://tickets.puppetlabs.com/browse/PUP-8502), [PUP-8507](https://tickets.puppetlabs.com/browse/PUP-8507))

-   Puppet 5.5.0 uses the `MultiJson` gem to choose the fastest available JSON backend at runtime. By default, it loads the JSON gem built into Ruby. This allows Puppet Server to choose a faster backend if implemented. ([PUP-8501](https://tickets.puppetlabs.com/browse/PUP-8501))

-   In addition to Ubuntu 16.10, Puppet 5.5.0 uses `systemd` as the default provider for Ubuntu 17.04 and 17.10. ([PUP-8482](https://tickets.puppetlabs.com/browse/PUP-8482))

-   The S-Expression (Clojure-style) data format generated by `puppet parser dump` is formalized and updated in Puppet 5.5.0, and is now considered a supported API for tool integration. The new format is available in text and JSON formats, by using the `--format` flag with either `pn` for the new format or `json` for the new format in JSON. The `--pretty` flag adds line breaks and indentation for readability.

    The previous format remains the default for `puppet parser dump`, but is deprecated and will be removed in the next major version of puppet. ([PUP-8482](https://tickets.puppetlabs.com/browse/PUP-8482))

-   In Puppet 5.5.0, the `fqdn_rand()` function uses SHA256 to compute seed/rand when running on FIPS-enabled hosts, instead of MD5 as used on other hosts. As such, hosts generate different `fqdn_rand()` values depending on whether FIPS is enabled. ([PUP-8469](https://tickets.puppetlabs.com/browse/PUP-8469))

-   Puppet 5.5.0 adds the new `provider_used` field to the report schema for serialization and deserialization. This field is populated with the provider used to provide the resource. ([PUP-8412](https://tickets.puppetlabs.com/browse/PUP-8412))

-   Puppet 5.5.0 can retrieve the current system state as Puppet code from devices using `puppet device`. ([PUP-8041](https://tickets.puppetlabs.com/browse/PUP-8041))

-   You can specify a type conversion in `lookup_options` for Hiera 5. This allows you to convert values to a rich data type, for example to make a value Sensitive or construct a Timestamp, as well as other values that cannot be directly represented in JSON or YAML. ([PUP-7675](https://tickets.puppetlabs.com/browse/PUP-7675))

-   The `puppet config` command in Puppet 5.5.0 can remove settings. ([PUP-3020](https://tickets.puppetlabs.com/browse/PUP-3020))

-   The `yumrepo` provider in Puppet 5.5.0 supports username and password fields. ([PUP-7400](https://tickets.puppetlabs.com/browse/PUP-7400))

### Regressions

-   The introduction of `multi_json` in Puppet 5.5.0 broke the JSON log destination. For example, running Puppet agent with the `--logdest` option pointed at a JSON target would result in a Ruby failure. This regression is fixed in Puppet 5.5.3. ([PUP-8773](https://tickets.puppetlabs.com/browse/PUP-8773))

### Deprecations

-   The S-Expression (Clojure-style) data format generated by `puppet parser dump` is formalized and updated in Puppet 5.5.0, and is now considered a supported API for tool integration. The new format is available in text and JSON formats, by using the `--format` flag with either `pn` for the new format or `json` for the new format in JSON. The `--pretty` flag adds line breaks and indentation for readability.

    The previous format remains the default for `puppet parser dump`, but is deprecated and will be removed in the next major version of puppet. ([PUP-8482](https://tickets.puppetlabs.com/browse/PUP-8482))

-   The `puppet man` command is deprecated and will be removed in a future release. To view Puppet command documentation from the command line, use `puppet help <subcommand>` or the installed manpages as `man puppet-<subcommand>`. ([PUP-8445](https://tickets.puppetlabs.com/browse/PUP-8445))