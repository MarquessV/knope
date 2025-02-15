# Introduction

Knope is a CLI for developers used to automate common tasks workflows. Things like transitioning issues, creating and merging branches, creating pull requests, bumping versions, tagging... anything that is a repetitive, time-consuming task in your development cycle, this tool is made to speed up.

## How it Works

Basically you create a file called `knope.toml` in your project directory which defines some workflows. The format of this file is described in [the chapter on config][config], the key piece to which is the `workflows` array. For a full example of a `knope.toml`, check out the file for this project! You can get started quickly with `knope --generate` which will give you some starter workflows.

Once you've got a config set up, you just run this program (`knope` if you installed normally via cargo). That will prompt you to select one of your configured workflows. Do that and you're off to the races!

## CLI Arguments

Running `knope` on its own will prompt you to select a defined workflow (if any). You can quickly run a workflow by passing the workflow's `name` as a positional argument. This is the only positional argument `knope` accepts, so `knope release` expects there to be a workflow named `release` and will try to run that workflow.

### Options

There are a few options you can pass to `knope` to control how it behaves.

1. `--help` prints out a help message and exits.
2. `--version` prints out the version of `knope` and exits.
3. `--generate` will generate a `knope.toml` file in the current directory.
4. `--validate` will check your `knope.toml` to make sure every workflow in it is valid, then exit. This could be useful to run in CI to make sure that your config is always valid. The exit code of this command will be 0 only if the config is valid.
5. `--dry-run` will pretend to run the selected workflow (either via arg or prompt), but will not actually perform any work (e.g., external commands, file I/O, API calls). Detects the same errors as `--validate` but also outputs info about what _would_ happen to stdout.
6. `--prerelease-label` will override the `prerelease_label` for any [`PrepareRelease`] step run.
7. `--upgrade` will upgrade your `knope.toml` file from deprecated syntax to the new syntax in preparation for the next breaking release.

### Environment Variables

These are all the environment variables that Knope will look for when running workflows.

1. `KNOPE_PRERELEASE_LABEL` works just like the `--prerelease-label` option. Note that the option takes precedence over the environment variable.
2. `GITHUB_TOKEN` will be used to load credentials from GitHub for [GitHub config].

## Features

More detail on everything this program can do can be found by digging into [config] but here's a rough (incomplete) summary:

1. Select issues from Jira or GitHub to work on, transition and create branches from them.
2. Do some basic git commands like switching branches or rebasing.
3. Bump the version of your project using semantic rules.
4. Bump your version AND generate a Changelog entry from conventional commits.
5. Do whatever you want by running arbitrary shell commands and substituting data from the project!

## Concepts

You define a [config] file named `knope.toml` which has some metadata (e.g. Jira details) about your project, as well as a set of defined [workflows][workflow]. Each [workflow] consists of a series of [steps][step] that will execute in order, stopping if any step fails. [Steps][step] can affect the state of the workflow. Some [steps][step] require that the workflow be in a specific state before they will work.

[config]: config/config.md
[workflow]: config/workflow.md
[step]: config/step/step.md
[`preparerelease`]: config/step/PrepareRelease.md
[github config]: config/github.md
