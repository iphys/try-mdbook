# Selecting SSG tool

I want to write articles and release them on the Internet.
I have some goals/expectations to the writing activity.

- I want to write articles in Markdown files and
  generate static HTML pages using a static site generator (SSG).
- The SSG tool must be easy to install and setup, and
  it must support mathematical symbols, formula, and equations out of the box.
- The SSG tool must provide good documentation for not only advanced users
  but also for general users.
- Using the SSG tool must not require users to know a lot about
  the tool's underlying technologies, and
  it must be easy and quick to maintain the site.
- The default site design must be _reasonable_, i.e.,
  the site must look simple and professional, and
  site navigation must be easy.

mdBook (MPL-2 license) seems to satisfy the above expectations pretty well.

- <https://rust-lang.github.io/mdBook/>

Key points

- mdBook is a single small binary program, which can build the site,
  provide a built-in web server for local development,
  and run tests for validating hyperlinks (and Rust code).
- The mdBook project directly supports Windows, Linux, and Mac.
- mdBook supports math right out of the box using MathJax. 👍
- Initial setup of the site's source is actually easy and quick.
  And the complexity of the maintenance remains simple.
- Syntax highlighting is supported with
  [highlight.js](https://highlightjs.org/).
- There is [good documentation](https://rust-lang.github.io/mdBook/),
  which is built with mdBook.
  It is well documented for general users,
  not assuming detailed knowledge about
  underlying technologies.
- Build is fast.

Other SSG tools would probably support this,
but it is possible to store the source files of the site
in a subfolder of the source repository,
rather than in the repository root
because it keeps the repository root clean.
