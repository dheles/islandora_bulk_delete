# Islandora Bulk Deleter

A utility module that does one thing: deletes all the objects in an Islandora collection, all issues and pages in a newspaper object, or all objects with a specific namespace. Use this tool carefully. It is dangerous.

## Requirements

* [Islandora Solr Search](https://github.com/Islandora/islandora_solr_search)
* [Islandora Paged Content](https://github.com/Islandora/islandora_paged_content) if you want to delete books, newspapers, or newspaper issues.

## Installation

1. `git clone https://github.com/mjordan/islandora_bulk_delete.git`
2. `drush en -y islandora_bulk_delete`

## Usage

There is no graphical user interface for this module. It only provides one drush command, `drush islandora_bulk_delete_delete`, or `drush iChainsaw` for short.

When you run the command, it tells you how many objects it is going to delete, and prompts you to make sure you want to go ahead. If you say 'y', it does to the resulting objects what a chainsaw does to the branches of a tree. If you say 'n', drush exits without doing anything.

However, there is a safety switch on this chainsaw. If you include the `--list` option, the objects are only listed and not deleted. The `--list` option does not take a value, unlike the other options described below.

To delete objects (to purge them, to use FedoraCommons' terminology), issue a command with one of the following templates:

* `drush iChainsaw --user=admin --collection=bar:collection`
* `drush iChainsaw --user=admin --collection=bar:collection --content_model=foo:contentModel --namespace=baz --list`
* `drush iChainsaw --user=admin --newspaper=sleazy:tabloid`
* `drush iChainsaw --user=admin --namespace=bar`
* `drush iChainsaw --user=admin --pid_file=/tmp/mypidfile.txt`

The `--collection`, `--content_model` and `--namespace` parameters are optional, and provide three ways to limit the set of objects to be deleted. If you include them, only objects in the specified collection of the specified content type and/or having the specified namespace will be deleted. The `--user` needs to have Drupal permission to "Permanently remove objects from the repository." The values of the `--collection` and `--content_model` options are PIDs.

Options also exist for specifying which Solr fields to use for content model, collection membership, and newspaper issue membership. Run drush iChainsaw --help for more detail.

The specified collection (or newspaper) object is not deleted, unless it has a namespace that is specified when only the `--namespace` option is used. For newspaper issues and book objects, all associated page objects are deleted. Please note that an object is deleted even if it is in another collection. It is not simply removed from the current collection.

If you have a list of PIDs you want to delete, you can use `--pid_file` instead of the other options. The value of `--pid_file` should be the absolute path to a text file containing one PID per row. Lines that start with a `#` or `//` are ignored. For example, if you had a PID file at `/tmp/mypidfile.txt` that contained the following:

```
islandora:1000
# This line is ignored.
islanodra:1120
islandora:6
```

and issued the command `drush iChainsaw --user=admin --pid_file=/tmp/mypidfile.txt`, objects with those three PIDs would be deleted. The only other option you can use in conjuction with `--pid_file` is `--list`, which will just list the PIDs named in your file the same way it does if you use one of the query options.

Newspapers are a special case:

* If you only want to delete all the issues (and their pages) within a specific newspaper, use the `--newspaper` option.
* If you want to delete all the newspapers (and their issues and pages) in a collection, use the regular `--collection` option.

## Maintainer

* [Mark Jordan](https://github.com/mjordan)

## License and Terms of use

* [GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
* Please review sections 15, 16, and 17 of the GPLv3 carefully before installing this module.
