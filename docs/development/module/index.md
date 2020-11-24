# farmOS module development

farmOS modules can be written to extend the capabilities of farmOS.

This document describes how to get started with farmOS module development. For
detailed documentation of Drupal development more generally, refer to the
[guide on drupal.org](https://www.drupal.org/docs/creating-custom-modules).

## Modules directory

Modules should be placed in the `modules` directory of the server's document
root, at the same level as the `profiles` directory, which contains the `farm`
profile (farmOS). If you are using the farmOS Docker image, this will be:
`/var/opt/drupal/web/modules`

farmOS automatically puts Drupal module dependencies in `modules/contrib`. A
good practice is to download farmOS-specific modules into `modules/farm` to keep
them separate. You may also consider creating a `modules/custom` directory for
custom modules that are specific to your farmOS instance.

## Namespacing

A farmOS (Drupal) module must have a unique name consisting only of
lowercase alphanumeric characters and underscores. This is used as a namespace
throughout the module, and allows Drupal hook functions to be executed on
behalf of your module.

It is best practice to prefix all farmOS-specific module names with `farm_`.
For example, if you were to build a module that adds a new log type called
`irrigation`, you might name it `farm_irrigation`. This serves to specify that
this module is made to work with farmOS, and is not designed to be installed in
other Drupal sites more generally.

## File structure

A farmOS (Drupal) module only requires one file for it to be recognized as a
module: `[modulename].info.yml` (where `[modulename]` is the module name).

This info YML file contains the module's human readable name, description,
dependency declarations, and other meta information about the module. A very
simple example looks like this:

`mylogtype.info.yml`:

```yaml
name: My log type
description: Adds my new custom log type.
type: module
package: farmOS Contrib
core_version_requirement: ^9
dependencies:
  - log:log
```

Other common files and directories in a module include:

- `[modulename].module` - Optional PHP file for Drupal hook implementations.
- `config/install/*.yml` - Configuration entities that will be installed with
  the module.
- `config/optional/*.yml` - Optional configuration entities that will only be
  installed if certain dependencies are met.
- `src/*` - PHP classes organized using the PSR-4 autoloading specification.
- `tests/*` - Automated tests for the module.