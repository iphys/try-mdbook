# Using mdBook

## Configuration

Create a repository in github.
Only `README.md` file is enough at this stage.

Clone the repository in your machine.

Download the pre-built binary program and saved it
in the root folder of the cloned local repository.
Do NOT add it to git.
(Use `.gitignore` to exclude.)

On the command shell,
initialize the book folder at the repository root
and `cd` to it.
(You must replace `<repository-root>` with your repository
root folder. `mdbook.exe` is for Windows.)

```bash
cd <repository-root>
./mdbook.exe init try-mdbook-with-github-pages
cd try-mdbook-with-github-pages
```

By specifying **try-mdbook-with-github-pages** for
**mdbook init**,
mdBook creates the **book** and **src** folders under it.
The use of the **try-mdbook-with-github-pages** folder
affects the build workflow only.
It does not appear in the release page at all.

- `<repository-root>` >
  `try-mdbook-with-github-pages` > `src`
  - You add your article Markdown files under this folder.

- `<repository-root>` >
  `try-mdbook-with-github-pages` > `book`
  - mdBook generates static site contents under this folder.
    Do _NOT_ add files under it to git because
    they are created during CI process in the remote.
    The `book` folder in your local repository is used to
    locally generate contents and view them.

In the `html.toml` file in the repository root, add these lines.
Do not delete existing lines.

```toml
[output.html]
site-url = "/try-mdbook-with-github-pages/"
mathjax-support = true
```

Once your local reposiory is ready for build,
run `mdbook serve --open`.
It builds the site locally, stores files under `book` folder,
and opens the browser.
The contents are dynamically reloaded
as you edit and save the source.

Check that math works.
This one is inline math:
\\( \int x dx = \frac{1}{2} x^2 + C \\).
A text can follow on the same line after the inline math. 👍

Here is display style math 👍

\\[ \mu = \frac{1}{N} \sum_{i=1}^N x_i \\]

### Setup GitHub Actions workflow file

Add a YAML file for building the site using GitHub Actions.
Base filename can be arbitrary, but its file extension
must be `yml`.
For example, use `deploy-mdbook.yml`,
and put it in the `.github/workflows` folder.

- <https://github.com/iphys/try-mdbook/blob/main/.github/workflows/deploy-book.yml>

The yml file is configured with two phases.
First phase is to create a CI job instance, clone the repository,
and install the latest mdbook.
Second phase is to build the site contents in the CI job instance
and push to the repository.
If the process finishes successfully,
GitHub automatically deploys the generated site to
the default GitHub Pages site.
The followings are the step-by-step description of
the second phase.

- In the CI job instance machine,
  cd to the `try-mdbook-with-github-pages` folder.
- Build the site contents.
- Use [**git worktree**][url-worktree] to create
  the `gh-pages` branch and folder.
  The folder becomes the repository root for the worktree branch.
- Cd to the `gh-pages` folder.
- Delete the [git reference file][url-git-ref] for
  the `gh-pages` branch with the
  "[git update-ref -d][url-update]" command.
  This also deletes commit history for the branch.
  You can confirm in the [commit history page][url-history]
  in github that
  the commit history has always only one commit record.
  The reason for always resetting the commit history for
  the `gh-pages` branch is that all of the contents in it are
  automatically generated from scratch whenever
  the mdbook build runs during CI.
  If you want to see the commit history of this repository,
  you should see it for the `main` branch
  because it is the branch where you make edits.
- Move the generated files from the `book` folder to
  the `gh-pages` folder.
- Add and commit the `gh-pages` branch.
  This takes place in the CI job instance machine,
  not in your local machine.
- Push the `gh-pages` branch from the CI machine
  to the remote repository.

[url-worktree]: https://git-scm.com/docs/git-worktree
[url-git-ref]: https://git-scm.com/book/en/v2/Git-Internals-Git-References
[url-update]: https://git-scm.com/docs/git-update-ref
[url-history]: https://github.com/iphys/try-mdbook/commits/gh-pages/

Below is an excerpt from the [GitHub Actions YML file][url-yml]
which does the step-by-step operations described above.

[url-yml]: https://github.com/iphys/try-mdbook/blob/main/.github/workflows/deploy-book.yml

```bash
cd try-mdbook-with-github-pages
mdbook build
git worktree add gh-pages
git config user.name "Deploy from CI"
git config user.email ""
cd gh-pages
# Delete the ref to avoid keeping history.
git update-ref -d refs/heads/gh-pages
rm -rf *
mv ../book/* .
git add .
git commit -m "Deploy $GITHUB_SHA to gh-pages"
git push --force --set-upstream origin gh-pages
```

### Setup in GitHub

In the GitHub repository, navigate to **Setting** > **Pages**.

For the Source setting, select "Deploy from branch".
("GitHub Actions" would also work,
but it requires different setups.)

For the Branch setting,
specify the `gh-pages` branch and the `/(root)` folder.

### Build and deploy

To see that the site files are generaged as expected,
[go to the repository](https://github.com/iphys/try-mdbook)
in the browser and select
the [`gh-pages` branch](https://github.com/iphys/try-mdbook/tree/gh-pages).
Check that the repository root contains
the `index.html` file and other resources for the page.

The released site:

- <https://iphys.github.io/try-mdbook/index.html>
