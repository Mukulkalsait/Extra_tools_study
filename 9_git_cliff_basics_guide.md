
[Git-cliff](https://git-cliff.org/) is an incredibly powerful, fast tool for auto-generating structured Markdown changelogs from your Git logs. Since you are running a cutting-edge NixOS flake environment, you can install it instantly without tracking cargo crates or global binaries. [1] 
Here is the complete walkthrough on how to install it, initialize a repository, format your work, and automate the maintenance process.
------------------------------
## 📦 1. Installation on NixOS
Since you are running an unstable NixOS ecosystem, you don't need raw binaries. You can drop it straight into a development shell or add it to your system config:

* 
* Temporary Dev Shell (Try it out):

nix shell nixpkgs#git-cliff

* Persistent System Install: Add pkgs.git-cliff to your environment.systemPackages or Home Manager file inside your flake repository.
* 

------------------------------
## 🚀 2. Initializing Git-cliff in Your Repository
Navigate to the project where you want to maintain your changelog and initialize the default blueprint configuration file: [2] 

cd /path/to/your/repo
git-cliff --init

This instantly generates a cliff.toml file in your project root. [2] 
## How to read/modify cliff.toml:
Open the generated file. It utilizes regex parsers to map your commits into groups. The default setup maps structural logs into clear visual Markdown headers: [3, 4] 

* 
* feat → 🚀 Features
* fix → 🐛 Bug Fixes
* chore, docs, style → Automatically categorized or filtered based on your preference. [3, 4, 5] 
* 

(Tip: If you want a tailored setup optimized specifically for platforms like GitHub, you can initialize using the built-in layouts instead: git-cliff --init github). [2] 
------------------------------
## ✍️ 3. How to Write Commits (The Key to Maintenance)
Git-cliff relies fundamentally on the [Conventional Commits Specification](https://www.conventionalcommits.org/). If you write normal, unstructured commit messages, git-cliff won't know how to sort them. [3] 
When you write features or bugs, structure your standard git commands like this:

git commit -m "feat(auth): add google oauth2 login flow"
git commit -m "fix(ui): adjust hyperland workspace padding on high-res monitors"
git commit -m "docs(readme): fix typo in flake inputs signature"

------------------------------
## ⛰️ 4. Generating and Maintaining the Changelog
Once you have written a few structured commits, you can generate your files using standard command-line flags:

* 
* Generate your first full log:
Creates (or completely updates) a standard CHANGELOG.md file from your inception point to the current branch tip.

git-cliff -o CHANGELOG.md

[6] 
* Preview unreleased changes:
If you have added multiple commits but haven't tagged a new release yet, this flag lists only what is sitting in limbo:

git-cliff --unreleased

[7, 8] 
* Calculate the next version bump:
Git-cliff can read your unreleased conventional commits, see if you introduced a breaking change or feature, and print out exactly what semantic version string (major, minor, or patch) you should tag next:

git-cliff --bumped-version

[9] 
* 

------------------------------
## 🤖 5. Automating the Maintenance Workflow
The cleanest way to maintain your project long-term without running terminal flags manually before every single release is to tie git-cliff into a workflow script or a pre-push hook.
Because you are tracking cutting-edge components (like Hyprland and Flake wrappers), you can use a basic release alias script or shell wrapper within your repo:

#!/usr/bin/env bash# release.sh
# 1. Fetch the automatically calculated next version version number
NEXT_VERSION=$(git-cliff --bumped-version)

echo "Bumping system configuration to $NEXT_VERSION..."
# 2. Update the CHANGELOG.md file up to this point
git-cliff --tag "$NEXT_VERSION" -o CHANGELOG.md
# 3. Commit the updated log and apply the Git version tag
git add CHANGELOG.md
git commit -m "chore(release): prepare for version $NEXT_VERSION"
git tag -a "$NEXT_VERSION" -m "Release $NEXT_VERSION"

If you ever publish this flake setup to a public repository, you can seamlessly integrate git-cliff with [GitHub Actions](https://github.com/marketplace/actions/git-cliff-changelog-generator) by placing orhun/git-cliff-action@v4 into your pipeline configuration to automatically format release notes on every tag push. [4, 10] 
Would you like me to help you customize the regex filters inside your cliff.toml file to ignore certain commits, or should we set up a custom markdown template for your generated changelog headers?

[1] [https://git-cliff.org](https://git-cliff.org/docs/)
[2] [https://orhun-git-cliff-33.mintlify.app](https://orhun-git-cliff-33.mintlify.app/quickstart)
[3] [https://github.com](https://github.com/orhun/git-cliff/blob/main/examples/detailed.toml)
[4] [https://dev.to](https://dev.to/alfreedom/automate-your-changelog-with-git-cliff-and-keep-a-changelog-2go8)
[5] [https://slhck.info](https://slhck.info/software/2025/10/13/git-cliff-for-changelogs.html)
[6] [https://medium.com](https://medium.com/@toniomasotti/git-cliff-96449950db48)
[7] [https://git-cliff.org](https://git-cliff.org/docs/usage/examples/)
[8] [https://orhun-git-cliff-33.mintlify.app](https://orhun-git-cliff-33.mintlify.app/configuration/overview)
[9] [https://git-cliff.org](https://git-cliff.org/docs/usage/args/)
[10] [https://github.com](https://github.com/marketplace/actions/git-cliff-changelog-generator)



## help 


```bash 

git-cliff 2.14.1
git-cliff contributors <git-cliff@protonmail.com>
A highly customizable changelog generator ⛰️

Usage:
  git-cliff [FLAGS] [OPTIONS] [--] [RANGE]

FLAGS:
  -h, --help
          Prints help information

  -V, --version
          Prints version information

  -v, --verbose...
          Increases the logging verbosity

      --list-templates
          Prints the names of the available templates (built-in and user-defined)

      --bumped-version
          Prints bumped version for unreleased changes

  -l, --latest
          Processes the commits starting from the latest tag

      --current
          Processes the commits that belong to the current tag

  -u, --unreleased
          Processes the commits that do not belong to a tag

      --topo-order
          Sorts the tags topologically

      --use-branch-tags
          Include only the tags that belong to the current branch

      --no-exec
          Disables the external command execution

  -x, --context
          Prints changelog context as JSON

      --use-native-tls
          Load TLS certificates from the native certificate store

OPTIONS:
  -i, --init [<CONFIG>]
          Writes the default configuration file to cliff.toml

      --templates-dir <PATH>
          Sets the directory to look up user-defined templates for `--init`.
          
          A user-defined template overrides a built-in template of the same name.
          
          [env: GIT_CLIFF_TEMPLATES_DIR=]

  -c, --config <PATH>
          Sets the configuration file.
          
          When omitted, the configuration is discovered automatically (a project `cliff.toml` / `.cliff.toml` / `.config/cliff.toml`, then the user configuration directory), falling back to the built-in default.
          
          [env: GIT_CLIFF_CONFIG=]

      --config-url <URL>
          Sets the URL for the configuration file
          
          [env: GIT_CLIFF_CONFIG_URL=]

  -w, --workdir <PATH>
          Sets the working directory
          
          [env: GIT_CLIFF_WORKDIR=]

  -r, --repository <PATH>...
          Sets the git repository
          
          [env: GIT_CLIFF_REPOSITORY=]

      --include-path <PATTERN>
          Sets the path to include related commits
          
          [env: GIT_CLIFF_INCLUDE_PATH=]

      --exclude-path <PATTERN>
          Sets the path to exclude related commits
          
          [env: GIT_CLIFF_EXCLUDE_PATH=]

      --tag-pattern <PATTERN>
          Sets the regex for matching git tags
          
          [env: GIT_CLIFF_TAG_PATTERN=]

      --with-commit <MSG>
          Sets custom commit messages to include in the changelog
          
          [env: GIT_CLIFF_WITH_COMMIT=]

      --with-tag-message [<MSG>]
          Sets custom message for the latest release
          
          [env: GIT_CLIFF_WITH_TAG_MESSAGE=]

      --skip-tags <PATTERN>
          Sets the tags to skip in the changelog
          
          [env: GIT_CLIFF_SKIP_TAGS=]

      --ignore-tags <PATTERN>
          Sets the tags to ignore in the changelog
          
          [env: GIT_CLIFF_IGNORE_TAGS=]

      --count-tags <PATTERN>
          Sets the tags to count in the changelog
          
          [env: GIT_CLIFF_COUNT_TAGS=]

      --limit-tags <N>
          Limits the number of tags to process
          
          [env: GIT_CLIFF_LIMIT_TAGS=]

      --skip-commit <SHA1>
          Sets commits that will be skipped in the changelog
          
          [env: GIT_CLIFF_SKIP_COMMIT=]

  -p, --prepend [<PATH>]
          Prepends entries to the given changelog file
          
          [env: GIT_CLIFF_PREPEND=]

  -o, --output [<PATH>]
          Writes output to the given file
          
          [env: GIT_CLIFF_OUTPUT=]

  -t, --tag <TAG>
          Sets the tag for the latest version
          
          [env: GIT_CLIFF_TAG=]

      --bump [<BUMP>]
          Bumps the version for unreleased changes. Optionally with specified version

  -b, --body <TEMPLATE>
          Sets the template for the changelog body
          
          [env: GIT_CLIFF_TEMPLATE=]

      --body-file <PATH>
          Reads the template for the changelog body from a file

      --from-context <PATH>
          Generates changelog from a JSON context
          
          [env: GIT_CLIFF_CONTEXT=]

  -s, --strip <PART>
          Strips the given parts from the changelog
          
          [possible values: header, footer, all]

      --sort <SORT>
          Sets sorting of the commits inside sections
          
          [default: oldest]
          [possible values: oldest, newest]

REMOTE OPTIONS:
      --github-token <TOKEN>
          Sets the GitHub API token
          
          [env: GITHUB_TOKEN]

      --github-repo <OWNER/REPO>
          Sets the GitHub repository
          
          [env: GITHUB_REPO=]

      --gitlab-token <TOKEN>
          Sets the GitLab API token
          
          [env: GITLAB_TOKEN]

      --gitlab-repo <OWNER/REPO>
          Sets the GitLab repository
          
          [env: GITLAB_REPO=]

      --gitea-token <TOKEN>
          Sets the Gitea API token
          
          [env: GITEA_TOKEN]

      --gitea-repo <OWNER/REPO>
          Sets the Gitea repository
          
          [env: GITEA_REPO=]

      --bitbucket-token <TOKEN>
          Sets the Bitbucket API token
          
          [env: BITBUCKET_TOKEN]

      --bitbucket-repo <OWNER/REPO>
          Sets the Bitbucket repository
          
          [env: BITBUCKET_REPO=]

      --azure-devops-token <TOKEN>
          Sets the Azure DevOps API token
          
          [env: AZURE_DEVOPS_TOKEN]

      --azure-devops-repo <OWNER/REPO>
          Sets the Azure DevOps repository
          
          [env: AZURE_DEVOPS_REPO=]

      --http-timeout <SECS>
          Sets the HTTP timeout for remote metadata requests in seconds
          
          [env: GIT_CLIFF_HTTP_TIMEOUT=]

      --offline
          Disable network access for remote repositories
          
          [env: GIT_CLIFF_OFFLINE=]

ARGS:
  [RANGE]
          Sets the commit range to process

```
