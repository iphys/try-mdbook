# Using mdBook

## Configuration

Create a repository in github.

Clone the repository.

For my Windows machine,
download the `mdbook.exe` program and saved it
in the root folder of the cloned local repository.
This binary is NOT added to git.
Use `.gitignore` to exclude it from git.

Initialize the book folder
at the repository root with
`./mdbook.exe init try-mdbook-with-github-pages`
and `cd` to it.

- Markdown files are stored in
  `try-mdbook-with-github-pages` > `src`.
  Add files under this folder to git.

- `mdbook` generates contents under the
  `try-mdbook-with-github-pages` > `book` folder.
  Do not add files under this folder to git because
  they are created during CI process in the remote.
  Locally generating contents is to view the contents
  locally with `mdbook serve --open`, which opens
  the browser and dynamically reloads the contents
  as you edit the source.

In the `html.toml` file in the repository root,
add these lines.

```toml
[output.html]
site-url = "/try-mdbook-with-github-pages/"
```

Add a YAML file for building the site
using gitHub Actions.
Filename can be arbitrary,
but for example, use `deploy-mdbook.yml`,
and put it in `.github/workflows` folder.

### Setup in GitHub

In the GitHub repository, navigate to Setting > Pages.

For the Source setting, select "Deploy from branch".
"GitHub Actions" would also work,
but this document explains for the "Deploy from branch" case.

For the Branch setting,
specify the `gh-pages` branch and the `/(root)` folder.

### Build and deploy

Select the [`gh-pages` branch](https://github.com/iphys/try-mdbook/tree/gh-pages),
and confirm that the repository root contains the `index.html` file and
other resources for the page.
