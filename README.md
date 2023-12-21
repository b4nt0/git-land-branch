# git-land

This is a git workflow script that takes care of merging a feature branch into the main branch. 

## Workflow description

### Definitions

| Term           | Definition                      | Default            |
| -------------- | ------------------------------- | ------------------ |
| landing branch | The branch that is being landed | Current branch     |
| target branch  | The branch that the landing branch is being landed to | `main` |
| remote         | The remote that the changes can be automatically pushed to | `origin` |

### Algorithm

1. Verify if the landing branch (the current branch or a specified branch) is clean.
2. Get the target branch and try to fast-forward it from the remote.
3. Rebase the landing branch on target branch. 
4. Merge the landing branch into the target branch with `--squash`. 
5. Commit.
6. Delete the landing branch.
7. Optionally, push the changes to the remote.

Every step must finish without conflicts, otherwise the workflow cleans up, restores the previous state, and errors out.

## Installation

To install the script, download it to any directory that is in your `PATH`. 

For example:

```sh
curl -o /usr/sbin/git-land https://raw.githubusercontent.com/b4nt0/git-land-branch/main/git-land
chmod +x /usr/sbin/git-land
```

## Usage

### Running the landing workflow

The basic command to land a branch is just `git land`.
This command will land the current branch onto `main`.

To review the available options, run `git land --help`.

### Configuration

#### Remote

By default, `git-land` uses `origin` as the remote. To use a different git remote, set the `git-land.remote` option, for example:

```sh
git config git-land.remote upstream
```

#### Main branch

By default, `git-land` lands onto the `main` branch. To use a different landing destination, set the `git-land.target` option, for example:

```sh
git config git-land.target master
```

### Command line options

To use `git-land`, run:

```sh
git land [options] [<comment> [<branch>]]
```

where:

`<comment>` is the landing commit comment

`<branch>` is the name of the landing branch

and the options are:


| Option       | What it does                                                          |
| ------------ | --------------------------------------------------------------------- |
| `--help`     | Displays the help text                                                |
| `--push`     | If set, `git-land` would automatically push the changes to the remote |
| `--verbose`  | Enables verbose output                                                |
| `--paranoid` | Asks for user confirmation for every write command                    |


## Thanks

This command is inspired by the [`git-land` script](https://github.com/git-land/git-land) made by Bazaarvoice, Inc., RetailMeNot, Inc., and other contributors.
