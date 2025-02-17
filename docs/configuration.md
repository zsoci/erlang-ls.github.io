# Configuration

## The `erlang_ls.config` file

It is possible to customize the behaviour of the `erlang_ls` server
via a configuration file, named `erlang_ls.config`. The
`erlang_ls.config` file should be placed in the root directory of a
given project to store the configuration for that project.

A sample `erlang_ls.config` file would look like the following:

```yaml
otp_path: "/path/to/otp/lib/erlang"
deps_dirs:
  - "lib/*"
diagnostics:
  enabled:
    - crossref
  disabled:
    - dialyzer
include_dirs:
  - "include"
  - "_build/default/lib"
lenses:
  enabled:
    - ct-run-test
  disabled:
    - show-behaviour-usages
macros:
  - name: DEFINED_WITH_VALUE
    value: 42
  - name: DEFINED_WITHOUT_VALUE
code_reload:
  node: node@example
```

The file format is `yaml`.

The following customizations are possible:

| Parameter          | Description                                                                                                                               |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| apps\_dirs         | List of directories containing project applications. It supports wildcards.                                                               |
| code\_reload       | Whether or not an rpc call should be made to a remote node to compile and reload a module                                                 |
| deps\_dirs         | List of directories containing dependencies. It supports wildcards.                                                                       |
| diagnostics        | Customize the list of active diagnostics                                                                                                  |
| include\_dirs      | List of directories provided to the compiler as include dirs. It supports wildcards.                                                      |
| lenses             | Customize the list of active code lenses                                                                                                  |
| macros             | List of cusom macros to be passed to the compiler, expressed as a name/value pair. If the value is omitted or is invalid, 'true' is used. |
| otp\_apps\_exclude | List of OTP applications that will not be indexed (default: megaco, diameter, snmp, wx)                                                   |
| otp\_path          | Path to the OTP installation                                                                                                              |
| plt\_path          | Path to the dialyzer PLT file. When none is provided the dialyzer diagnostics will not be available.                                      |

### Diagnostics

When a file is open or saved, a list of _diagnostics_ are run in the
background, reporting eventual issues with the code base to the
editor. The following diagnostics are available:

| Diagnostic Name | Purpose                                                                              |
|-----------------|--------------------------------------------------------------------------------------|
| compiler        | Report in-line warnings and errors from the Erlang [compiler][compiler]              |
| dialyzer        | Use the [dialyzer][dialyzer] static analysis tool to find discrepancies in your code |
| elvis           | Use [elvis][elvis] to review the style of your Erlang code                           |
| crossref        | Use information from the Erlang LS Database to find out about undefined functions    |

Currently, all of the available diagnostics are enabled by default.

It is possible to customize diagnostics for a specific project. For example:

```
diagnostics:
  disabled:
    - dialyzer
    - crossref
```

## Automatic Code Reloading

The `code_reload` takes the following options:

| Parameter | Description                                                          |
|-----------|----------------------------------------------------------------------|
| node      | The node to be called for code reloading. Example erlang_ls@hostname |

## Code Lenses

_Code Lenses_ are also available in Erlang LS. The following lenses
are available in Erlang LS:

| Code Lens Name        | Purpose                                                                              |
|-----------------------|--------------------------------------------------------------------------------------|
| ct-run-test           | Display a _run_ button next to a Common Test testcase                                |
| server-info           | Display some Erlang LS server information on the top of each module. For debug only. |
| show-behaviour-usages | Show the number of modules implementing a behaviour                                  |

The following lenses are enabled by default:

* show-behaviour-usages

It is possible to customize lenses for a specific project. For example:

```
lenses:
  enabled:
    - ct-run-test
  disabled:
    - show-behaviour-usages
```

## Global Configuration

It is also possible to store a system-wide default configuration in an
`erlang_ls.config` file located in the _User Config_ directory. The
exact location of the _User Config_ directory depends on the operating
system used and it can be identified by executing the following
command on an Erlang shell:

    > filename:basedir(user_config, "erlang_ls").

Normally, the location of the _User Config_ directory is:

| Operating System | User Config Directory                               |
|------------------|-----------------------------------------------------|
| Linux            | /home/USER/.config/erlang\_ls                       |
| OS X             | /Users/USER/Library/Application\ Support/erlang\_ls |
| Windows          | c:/Users/USER/AppData/Local/erlang\_ls              |

Thus on Linux, for example, the full path to the default configuation file
would be `/home/USER/.config/erlang_ls/erlang_ls.config`

[compiler]:https://erlang.org/doc/man/compile.html
[dialyzer]:http://erlang.org/doc/apps/dialyzer/dialyzer_chapter.html
[elvis]:https://github.com/inaka/elvis
