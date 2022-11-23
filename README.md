# fmtya

GitHub Action to run [yamlfmt](https://github.com/google/yamlfmt)

## Usage

### Basic Setup

The following is a reasonable, bare-minimum, setup. It runs `fmtya` with all the
settings set to their default values. This will run `yamlfmt` on the repository
and if any yaml files are reformatted, it will commit them back to the branch
that was pushed.

```yaml
name: "Format yaml files"
on:
  push:
jobs:
  yamlfmt:
    runs-on: ubuntu-latest
    steps:
      - uses: norwd/fmtya@v1
```

### Advanced Setup

A more advanced setup can customise everything from the commit's message down to
the specific version of the `yamlfmt` backend!

```yaml
name: "Format yaml files"
on:
  push:
jobs:
  yamlfmt:
    runs-on: ubuntu-latest
    steps:
      - uses: norwd/fmtya@v1
        with:

          // Due to how GitHub's permissions system is set up, the default token
          // does not have the necessary access to update workflow files. If you
          // want to want `fmtya` to format the files in the `.github/workflows`
          // directory, you will need to set up a PAT with at least write access
          // to both the `repo` and `workflows` permissions.
          token: ${{ secrets.<YOUR_PAT> }}

          // By default, `fmtya` uses the latest available version of `yamlfmt`.
          // This may result changes to the behaviour of `fmtya`, for example if
          // a major version of `yamlfmt` is released. If you prefer to manually
          // select a specific version of `yamlfmt`, any released version can be 
          // pecifically requested.
          yamlfmt-version: vX.Y.Z

          // If the default commit message is too generic, or you want to create
          // a custom commit message, for example, from the output of a previous
          // workflow step, you can overwrite the commit message to any string.
          commit-message: Reformat .yml files with yamlfmt

          // When the formatting changes are made, they are committed by the bot
          // account for GitHub Actions, and whoever pushed the unformatted file
          // is listed as a co-author. To attribute the commit to a real person,
          // or if you want to sign the commit, you can specify the username and
          // email the same way you would to `git config user.name <USERNAME>`.
          commit-user-name: <YOUR_GITHUB_USERNAME>
          commit-user-email: <YOUR_GITHUB_EMAIL

          // By default, the commits are not signed, which may make them show as
          // unverified if any of the co-authors have vigilant mode turned on in
          // their GitHub account settings. You can enable signed commits with a
          // GPG private key stored in a secret, just be sure that the email set
          // in the GPG key matches the email you set in `commit-user-email`!
          signing-private-key: ${{ secrets.<YOUR_GPG_SIGNING_KEY> }}
          signing-passphrase: ${{ secrets.<YOUR_GPG_PASSWORD> }}
```
