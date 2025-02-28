rmarkdown 1.14
================================================================================

- Fixed a regression in `ioslides_presentation` that background colors via the `data-background` attribute on slides stopped working (thanks, @ShKlinkenberg, #1265).

- Fixed the bug #1577 introduced in **rmarkdown** v1.12: tabsets, floating TOC, and code folding in the `html_document` format no longer work with the `shiny` runtime (thanks, @RLesur for the fix #1587, and @fawda123 @ColinChisholm @JasonAizkalns for the bug report).

- For `render()`, if the input filename contains special characters such as spaces or question marks (as defined in `rmarkdown:::.shell_chars_regex`), the file will be temporarily renamed with the special characters replaced by `-` (dash) instead of `_` (underscore, as in previous versions of **rmarkdown**). This change will affect users who render such files with caching (cache will be invalidated and regenerated). The change is due to the fact that `-` is generally a safer character than `_`, especially for LaTeX output (#1396).


rmarkdown 1.13
================================================================================

- For `pdf_document()`, do not override margins to 1 inch when a custom document class or geometry settings are specified in the YAML front matter (thanks, @adunning, #1550)

- The default value of the `encoding` argument in all functions in this package (such as `render()` and `render_site()`) has been changed from `getOption("encoding")` to `UTF-8`. We have been hoping to support UTF-8 only in **rmarkdown**, **knitr**, and other related packages in the future. For more info, you may read https://yihui.name/en/2018/11/biggest-regret-knitr/.

- The option `toc_float: true` for `html_document` now preserves the text formatting (thanks, @codetrainee, #1548).

- For the `output_file` argument of `render()`, a file extension will be automatically added if the filename does not contain an extension (e.g., `render('foo.Rmd', 'html_document', output_file = 'bar')` will generate `bar.html`); see the help page `?rmarkdown::render` for details (thanks, @apreshill, #1551).

- TOC items are not correctly indented when `toc_float` is enabled for the `html_document` format (thanks, @carolynwclayton #1235 and @RLesur #1243).

- Fixed rstudio/shiny#2307 where the second execution of a `shiny_prerendred` document with `href` dependencies would cause a prerender check error (thanks, @schloerke, #1562).

- The `*_files` directory is not properly cleared due to the inappropriate fix for #1503 and #1472 in the last version (thanks, @wxli0 #1553, @cderv #1566).

- Added an `output_extensions` argument to `pdf_document()` to make it possible to enable/disable Pandoc extensions for the LaTeX output format (thanks, @hongyuanjia, rstudio/bookdown#687).


rmarkdown 1.12
================================================================================

* Fixed file extensions of output files when using non-markdown Pandoc extensions such as `docx+styles` (#1494, @noamross).

* Added a new argument `extra_lines` to `latex_dependency()` to allow users to add extra lines of LaTeX code after `\usepackage{}`. Also added a helper function `latex_dependency_tikz()` based on `latex_dependency()` (#1502, @malcolmbarrett).

* Fixed #1529: when the path of an Rmd file contains Unicode characters that cannot be represented in the system native encoding (especially on Windows), `rmarkdown::render()` may fail.

* Applied a correct fix to an old **plotly** issue ropensci/plotly#463.

* HTML widgets used to be hidden when printing ioslides to PDF in Chrome. Now they will be printed correctly.

* `render(output_format = 'all')` may delete the figure directories that are still needed by certain output formats when one output format doesn't need its figure directory (thanks, @rmcd1024 #1472, @cderv #1503).

* The `<em>` tags in the subtitle, date, and author are removed from the default HTML template (thanks, @royfrancis, #1544).


rmarkdown 1.11
================================================================================

* Fixed #1483, which prevented the triangle to be displayed in Firefox when `<details><summary>...</summary></details>` was used (#1485, @bisaloo)

* Provided `rmarkdown.pandoc.args` as a **knitr** package option in `knitr::opts_knit` (#1468, @noamross).

* Added the exported function `pandoc_exec()`, which returns the path of the pandoc binary used by the package (#1465, #1466 @noamross).

* `new_session: true` in `_site.yml` causes `render_site()` to render each file in a new R session, eliminating some cross-file difficulties, such as function masking (#1326, #1443 @jennybc).

* Added the LaTeX command `\passthrough` in the default LaTeX template for the `--listings` flag of Pandoc (rstudio/bookdown#591).

* The icons in `flexdashboard::valueBox()` are not of the full sizes due to the upgrade of FontAwesome in #1340 in the previous version (#1388, rstudio/flexdashboard#189).

* Added the ability to generate tabset dropdowns, usable by adding the `.tabset-dropdown` class to a header (e.g., `# Heading {.tabset .tabset-dropdown}`) (#1405). Thanks @stefanfritsch for contributing the necessary code for this (#1116).

* The `darkly` theme (a darker variant of the Bootswatch `flatly` theme) has been added to `html_document` and `html_notebook` (#1409, #889).

* Fixed a regression that caused scrollbars on code blocks when the syntax highlighting theme is not the default (#654, #1399).

* Fixed #1407: reactive expressions can break the section headers of Shiny R Markdown documents.

* Fixed #1431: `render()` with the `intermediates_dir` argument when the output format is `powerpoint_presentation` with a custom `reference_doc` fails to find the reference document.

* Fixed the website navbar not being able to display submenus properly (#721, #1426).

* Added checks for shiny-prerendered documents to find all html dependencies, match all execution packages, and match the major R version (#1420).

* Added an argument `cache = TRUE` to the internal function `rmarkdown:::find_pandoc()`, so that users can invalidate the cached path of Pandoc via `rmarkdown:::find_pandoc(cache = FALSE)` (thanks, @hammer, #1482).

* Added an RStudio project template for simple R Markdown websites, so that users can create such websites from RStudio: `New Project -> New Directory -> Simple R Markdown Website` (thanks, @kevinushey, #1470).

* Fixed #1471: Pandoc's (version 2.x) syntax highlighting themes don't work well with the Bootstrap style (thanks, @gponce-ars #1471, @cderv #1489).

* Fixed the warnings in #1224 and #1288 when calling `render()` with an absolute `output_dir` or `intermediates_dir`.

* Fixed #1300: calling `render()` with `intermediates_dir` may fail when the intermediate dir is on a difference device or filesystem.

* Fixed #1358: calling `render()` with `intermediates_dir` will fail if the Rmd document contains bibliography files that are dynamically generated.


rmarkdown 1.10
================================================================================

* Added a new argument `slide_level` to `powerpoint_presentation()` (#1270).

* The **tinytex** package has become a required dependency (to build R Markdown to PDF).

* Added `compact-title` variable to the LaTeX default templates to control use of LaTeX `titling` package; defaults to `true` (#1284).

* `pdf_document(template = NULL)` does not work (#1295).

* Restore ability to use any HTML format with R Markdown Websites (#1328).

* Add `options` argument to `paged_table()` to enable explicit passing of display options.

* Add `pandoc_citeproc_convert()` function for conversion of bibliography files (e.g. BibTeX files).

* Update to Font Awesome version 5.0.13 (#1340).

* Add `site_resources()` function for computing resource files required for a website.

* Export `default_site_generator()` function.

* The `latex_document()` format should not clean up the figure directory (thanks, @emiltb, rstudio/bookdown#582).

* Enable post processors that change the output file to specify that the base post processor should still be applied to the original output file.


rmarkdown 1.9
================================================================================

## NEW FEATURES

* Added a new (experimental) output format `powerpoint_presentation`. If you want to test it, you will need Pandoc >= 2.1 (#1231).

## MAJOR CHANGES

* If the **tinytex** package is installed, PDF output is built through `tinytex::latexmk()`, otherwise it is generated by `rmarkdown:::latexmk()`, which has been factored out and improved in the **tinytex** package, so it is recommended that you install the **tinytex** package (#1222).

## BUG FIXES

* Temporary files created in `render()` may be cleaned up prematurely, which can cause problems with Shiny R Markdown documents (#1184).

* Further improvements regarding compatibility with Pandoc 2.0, e.g. tabbed sections don't work (https://community.rstudio.com/t/3019).

* When `preserve_yaml = TRUE` in `md_document()`, `toc = TRUE` fails to create the table of contents (thanks, @stla, #1216).

* Suppress confusing error messages from `knitr::purl()` during `rmarkdown::find_external_resources()` (thanks, @aronatkins #1247, and @paulobrecht #1154).

* Fixed the obscure error `Error: path for html_dependency not found:`, which was due to the HTML dependency of highlight.js (thanks, @bborgesr, #1213).


rmarkdown 1.8
================================================================================

## BUG FIXES

* `render_site()` does not work with `_site.yml` that does not have the `output` setting (#1189).

* The variables `input` and `output` do not work in Shiny R Markdown documents (#1193).

* `ioslides_presentation` fails to embed images (#1197).

* With Pandoc 2.x, `github_document()` generates the wrong filename extension `.gfm-ascii_identifiers` instead of `.md`, and line height of code blocks in the HTML preview is too big (#1200).


rmarkdown 1.7
================================================================================

* Fixed an issue with `df_print: paged` where row names where not printed and added support for `rownames.print` option to control when they print.

* Add `smart` option for `word_document()` format.

* Save render intermediates when generating beamer presentations (fixes #1106).

* Fixed issues when specifying NULL/null/empty parameter values (#729 and #762).

* Better error message when unable to prerender a document. (#1125)

* `shiny::renderText()` does not work in Markdown section headings (#133).

* The `value` argument of `pandoc_variable_arg()` can be missing now (#287).

* Background colors and images are supported for ioslides presentations (#687).

* HTML widgets in an Rmd document cannot be rendered if another Rmd document is rendered via `rmarkdown::render()` in this document (#993).

* Try harder to clean up temporary files created during `render()` (#820).

* Wrong environment for evaluating R code chunks in Shiny R Markdown docs (#1162, #1124).

* Do not call `bibtex` to create the bibliography when there are no citations in the document and the output format is `pdf_document()` with `citation_package = 'natbib'` (#1113).

* `render()` will stop if the output format is PDF but there are any errors during building the index or bibliography (#1166).

* `beamer_presentation()` doesn't work when `citation_package != 'none'` (#1161).

* File-based inputs don't work in parameterized documents (#919).

* rmarkdown is compatible with Pandoc 2.0 now (#1120).

* `render()` with `intermediates_dir` fails with R plots (#500).

* Added two new output formats `latex_document()` and `latex_fragment()` (#626).

* Relative paths of images in HTML output should not be resolved to absolute paths (#808).

* `render_site()` does not support multiple output formats for a single Rmd (#793).

* Unicode characters may be scrambled when downloading the Rmd source file using the download button generated by `html_document(code_download = TRUE)` (#722).

* Upgraded highlight.js from v1.1 to v9.12.0 (#988, #907).

* The argument `keep_md = TRUE` actually preserves the Markdown output file from `knitr::knit()` now (as documented). Previously, it generates a new Markdown file by concatenating the YAML metadata (title, author, date) with the body of the original Markdown output file (#450).

* For `md_document()`, when `variant == 'markdown'` and `perserve_yaml = TRUE`, the Pandoc argument `--standalone` should not be used (#656).


rmarkdown 1.6
================================================================================

* Fixed an issue where headers with non-ASCII text would not be linked to correctly in the table of contents.

* Support code folding for bash, python, sql, Rcpp, and Stan chunks.

* Provide rmarkdown.pandoc.id_prefix as Knit option

* Fixed two issue with `df_print: paged`, one would prevent rendering data with lists of lists and the other where the column type would get cut off.

* Better support for `citation_package: biblatex` in `pdf_document()` (#1062).

* On certain Windows platforms, compiling LaTeX to PDF may fail because `system2(stdout = FALSE)` is not supported, in which case the default `system2()` will be used (#1061).

* Allow paged tables to render even when page load / visibility has a long delay


rmarkdown 1.5
================================================================================

* Fixed an issue where code within Shiny pre-rendered documents was not rendered correctly.

* Add `includes` parameter to `html_fragment` format.

* Use RStudio redirection URL to replace deprecated MathJax CDN


rmarkdown 1.4
================================================================================

* `data.table` expressions involving `:=` are no longer automatically printed within R Markdown documents. (#829)

* Fix #910: the extra_dependencies argument of pdf_document() does not work when no code chunks contain LaTeX dependencies.

* The extra_dependencies of pdf_document() can also take a character vector of LaTeX package names, or a named list of LaTeX package options (with names being package names), which makes it much easier to express LaTeX dependencies via YAML.

* Automatically ignore packrat directory for render_site

* Fix #943: escape end tags in shiny_prerendered code contexts

* Add support for sticky tabs in html_document via tabset-sticky class

* Process Rmd files with lowercase extension (.rmd) in render_site

* Fix stack space consumption issues with large JS payloads in chunks

* Add `section_divs` argument to html_document and html_fragment to control generation of section tags/divs.

* Remove data-context="(data|server|server-start)" chunks from HTML served to client in shiny_prerendered


rmarkdown 1.3
================================================================================

* Fix v1.2 regression in ordering of CSS for ioslides_presentation.

* Fix rendering of pagedtables within html_notebook format.

* Ensure that html_prerendered UI is never cached.

* Add `citeproc` argument to YAML header; controls whether pandoc-citeproc is used (#831)


rmarkdown 1.2
================================================================================

* Add support for df_print to handle additional dplyr classes: grouped_df, rowwise_df and tbl_sql.

* Add new `runtime: shiny_prerendered` mode for interactive documents.

* Prepend "section-" to ids in runtime: shiny[_prerendered] to eliminate potential conflicts with shiny output ids

* Use html_dependencies for highlight.js, pagedtables, slidy, ioslides, & navigation (improved dependency behavior for runtime: shiny[_prerendered])

* Serialize runtime: shiny[_prerendered] dependencies to JSON rather than .rds

* ioslides: check for null before concatenating attr["class"] (#836)

* Add rmarkdown.onKnit/onKnitCompleted package hooks

* Non-ASCII keys in yaml file should be marked to UTF8 as well, when read into R as the name of a list (#841)

* Remove key-column special-case left alignment in pagedtables (#829)

* Replace backslashes in floating TOC headings (#849)

* Suggests rather than Imports for tibble (R 3.0 compatibility)

* Add `paged_table` function for printing paged tables within HTML documents

* Support `{.active}` attribute for setting initially active tab (#789)

* Add `knit_root_dir` argument to `render()` and YAML header, a convenience for setting knitr's root.dir option

* Improve alignment of text in sub-topics for floating TOC

* Bibliography file paths in YAML containing forward slashes could not be rendered (#875)


rmarkdown 1.1
================================================================================

* Fixed an issue where attempts to render an R Notebook could fail if the path contained multibyte characters.

* Fixed an issue where the default Beamer template did not provide vertical padding between paragraphs with certain versions of pandoc (<= 1.17.2).

* Try to install latexmk automatically on Windows

* Added `df_print` option to html_document format for optionally printing data frames using `knitr::kable`, the `tibble` package, or an arbitrary function

* Fix for render_site not showing Chinese characters correctly

* Fix for ignoring knit_meta that is explicitly passed to render

* Parameter editing: don't allow NULL to overwrite previous state

* Parameter editing: fix incorrect name for parameters with expressions

* Parameter editing: allow multiple values when the parameter is configured to use a "multiple" selector

* Switched the order in which format dependencies are added for `html_document` so that `extra_dependencies` are added at the end, after bootstrap, etc. (#737)

* `pdf_document(keep_tex = TRUE)` will generate the .tex document even if PDF conversion fails (#779).

* Move latex header includes to just before \begin{document}

* Special 'global' chunk label for runtime: shiny which designates a chunk to be run once and only once in the global environment (startup performance improvement for multi-user shiny documents)

* Ensure supporting files are writeable (#800)

* Make the "show code" buttons more CSS-friendly (#795)

* Exclude `output_dir` from site files (#803)

* Export `navbar_html` and `yaml_front_matter` functions


rmarkdown 1.0
================================================================================

* `toc_float` no longer automatically sets `toc = TRUE`

* Added an argument `error` to `pandoc_available()` to signal an error when (if `error = TRUE`) pandoc with the required version is not found.

* Added `html_notebook` format for creating HTML documents that include source code and output.

* Added `resolve_output_format` function (useful for front ends that need to mirror the default format resolution logic of `render`).

* Added `code_download` option to `html_document` to provide an option to embed a downloadable copy of the Rmd source code within the document.

* Added `slide_level` option to ioslides_presentation to set the level of heading used for individual slides.

* Added `hard_line_breaks` option to `github_document` to deal with change in behavior of GitHub's markdown renderer with respect to line breaks.

* Use "markdown_strict" rather than "markdown" for `pandoc_self_contained_html` when pandoc >= 1.17 (pandoc hanging bug was fixed in this version)

* Default highlighting engine for `html_document` now highlights bash, c++, css, ini, javascript, perl, python, r, ruby, scala, stan and xml

* Added `print` sub-option to `toc_float` to control whether the table of contents appears when user prints out the HTML page.

* Added `readme` option to `html_vignette` which automatically creates a package level README.md (for GitHub) in addition to rendering the vignette.

* Support for `keep_md` in `html_vignette` format.

* Try to install the latexmk package automatically on Windows if the executable latexmk.exe exists.


rmarkdown 0.9.6
================================================================================

* Ability to set `opts_hooks` in `knitr_options()` (#672)

* Added `render_site` and related functions for rendering collections of documents within a directory as a website.

* Ability to define html_document navigation bar using simple yaml format.

* Added `pre_knit` and `post_knit` hooks for output formats.

* Discover `LaTeX` dependencies and add them to the `.tex` preamble (#647)

* Added new `all_output_formats` function to enumerate all output formats registered for an Rmd.

* Change `fig_caption` default to TRUE for all formats

* Change `fig_retina` to 2 for HTML formats (no longer contingent on `fig_caption`)

* Ensure pandoc binary exists before binding to pandoc directory (#632)

* Handle relative paths for 'default_output_format' (#638)

* Eliminate duplicate viewport meta tag from html_document

* Added biblatex biblio-style support to the LaTeX template for Pandoc 1.15.2 (#643)

* Allow override of header font-size in html_document custom css (#652)

* Fix for horizontal scrollbars appearing w/ code folding (#654)

* Specifying `toc_float` in `html_document` now automatically sets `toc = TRUE`

* Enable per-header opt-out of `toc-float` via {.toc-ignore} attribute

* Correctly handle soft line breaks in ioslides_presentation (#661)

* Don't use text-muted for code folding btns (text visibility in non-default themes)

* Fix for rendering non-HTML formats from .md files (resolve runtime before knit)

* `html_dependency_bootstrap` now accepts theme = "default" argument

* Use pandoc 1.17.0.2 compatible LaTeX template when pandoc >= 1.17.0.2

* Support custom template for `ioslides_presentation`

* Added `analytics` option for `ioslides_presentation` for Google Analytics

* Removed the extra tag `<p></p>` around HTML output (typically generated by htmltools) from code chunks, to avoid invalid HTML like `<p><div>...</div><p>` (#685)


rmarkdown 0.9.5
================================================================================

* Added odt_document format for OpenDocument Text output

* Added rtf_document format for Rich Text Format output

* Added github_document format for GitHub Flavored Markdown output

* Only apply white background for themed HTML documents (#588)

* Added <meta name="viewport" content="width=device-width, initial-scale=1"> to the default HTML template to make it work better with mobile browsers. (#589)

* Specify --filter pandoc-citeproc after custom pandoc args

* Long lines in code blocks will be wrapped in the `html_vignette()` output (#595)

* Added new arguments `run_pandoc = TRUE` and `knit_meta = NULL` to `render()`. See the help page of `render` for details. (#594)

* The `tufte_handout` format now delegates to the `tufte` package and no longer provides a base template.

* Use pandoc 1.15.2 compatible LaTeX template when pandoc >= 1.15.2

* Fix issue with Beamer template and pandoc 1.15.2

* Updated embedded JQuery to v1.11.3 and Bootstrap to v3.3.5.

* Expose core HTML dependencies for use by custom R Markdown formats.

* New `html_document` themes: "lumen", "paper", "sandstone", "simplex", & "yeti".

* Ability to include bootstrap navbar for multi-page `html_document` websites

* Added support for `abstract` field to `html_document` format

* Added support for floating table of contents (via tocify) to `html_document`

* Added support for tabsets via use of {.tabset} class on top-level headings

* Added support for folding/unfolding of R code chunks in `html_document`

* Support `url` references in CSS files for `runtime: shiny`

* Change name of common options file to `_output.yml`

* Tweak pandoc conversion used in `pandoc_self_contained_html` to prevent hanging with large script elements (use "markdown" rather than "markdown-strict" as input format)

* The filename extension .bib will be removed before bibliography files are passed to Pandoc when the output is LaTeX/PDF and the citation package natbib or biblatex is used on Windows. This is because bibtex in MikTeX will always add the extension .bib to bibliography files, e.g. it treats foo.bib as foo.bib.bib. (#623)

* Render Shiny documents in a clean environment; fixes issue in which code in Shiny documents could access internal R Markdown state


rmarkdown 0.9.2
================================================================================

* Added a fix to #580 for Windows users.


rmarkdown 0.9.1
================================================================================

* Fix for a bug causing certain files to be deleted as intermediate files. (#580)

* PDF/LaTeX output no longer uses natbib as the citation package by default. If you do want to use natbib or biblatex, you may still use the argument citation_package = 'natbib' or 'biblatex'. (#577)


rmarkdown 0.9
================================================================================

* Fix for JS exception in slidy_presentation when served from the filesystem (don't call pushState for file:// urls)

* Escape single quotes in file paths

* Fix for missing resources when rendering a filename with shell characters

* For PDF/LaTeX output, citations are processed via natbib or biblatex instead of pandoc-citeproc. The ciation package natbib or biblatex can be chosen using the argument `citation_package` in `pdf_document()`, `beamer_presentation()`, and `tufte_handout()`. LaTeX is compiled to PDF via latexmk (https://www.ctan.org/pkg/latexmk/); if it is not installed, a simple emulation will be used (run pdflatex/xelatex/lualatex, bibtex, and makeindex a few times). You are recommended to install latexmk, and please note latexmk requires a Perl installation (this is important especially for Windows users).

* Always use the graphics package for PDF output

* Fix for the error "cannot change value of locked binding for 'metadata'" when one call of rmarkdown::render() is nested in another one (#248)

* Fix for an issue causing image paths to be rendered incorrectly in Windows when rendering an `html_document` with `self_contained: false` and a path is passed in argument `output_dir`. (#551)

* Add always_allow_html option for preventing errors when html_dependencies are rendered in non-HTML formats (e.g. pdf_document or word_document).

* Fix for an issue causing resources not to be discovered in some documents containing an empty quoted string (`""`) in an R chunk.


rmarkdown 0.8.1
================================================================================

* Support for table of contents in word_document (requires pandoc >= 1.14)

* Support for opts_template in knitr options

* Don't implicitly discover directories when scanning for dependent resources

* Fix for slide numbers not showing up in ioslides when served from the filesystem (don't call pushState for file:// urls)

* Remove inlining of bootstrap CSS (was workaround for bug now fixed in pandoc)

* Allow specifying an R file in calls to find_all_resources


rmarkdown 0.8
================================================================================

* Add support for keep_md to word_document

* Increase pandoc stack size to 512M (often required for base64 encoding e.g. larger embedded leaflet maps). Stack size can also be controlled by the pandoc.stack.size option.

* Allow YAML front matter to be terminated with ...

* Automatically generate a user-interface (via a Shiny application) for user specification of values in parameterized reports.

* Add tightlist macro for compatibility with pandoc >= 1.14

* Bugfix: Don't merge render params recursively with knit_params

* Bugfix: Handle slashes correctly on Windows for slidy_presentation when self_contained = FALSE


rmarkdown 0.7
================================================================================

* Add latex_engine option to beamer_presentation format

* Ensure that when LANG=en_US pandoc receives en_US.UTF-8 (prevent hang)

* Generate MathJax compatible output when using html_fragment format.

* Use pandoc built-in template for Beamer

* Use pandoc 1.14 compatible LaTeX template when pandoc >= 1.14

* Inline bootstrap.min.css to workaround pandoc 1.14 base64 encoding issue

* Add support for discovering references to external resources when the document output format is PDF

* Fix several issues causing pandoc errors when an intermediates directory is used, including during render for Shiny documents


rmarkdown 0.6
================================================================================

* Support for parameterized reports. Parameter names and default values are defined in YAML and can be specified via the 'params' argument to the render function

* 'md_extensions' option to enable/disable markdown extensions for input files

* Automatically discover and include dependent resources (e.g. images, css, etc.) for interactive documents.

* Added pandoc_version function.

* Use VignetteEncoding directive in html_vignette format

* Fix issues related to use of non-ASCII characters in ioslides_presentation

* Non-ASCII characters in the YAML data are not marked with the UTF-8 encoding when they are read into R, so character strings in `rmarkdown::metadata` may be displayed incorrectly (#420)

* Various improvements to tufte_handout format


rmarkdown 0.5.1
================================================================================

* Add 'dev' option to output formats to specify output device for figures

* Enable use of footnotes in titles of LaTeX documents

* Various improvements related to directory detection/handling on Windows


rmarkdown 0.4.2
================================================================================

* Sync to the latest LaTeX and Beamer templates from pandoc-templates

* Switched from the Bootstrap 2 web framework to Bootstrap 3. This is designed to work with Shiny >= 0.10.3, which has made the same switch

* Add CSS to restore responsive image behavior from Bootstrap 2

* Use a more subtle treatment for inline code in Bootstrap themed documents

* Improved support for multiple authors in ioslides

* Workaround for poor rendering of ioslides multi-columns lists in Safari 8

* Serve index.html as fallback default file for rmarkdown::run


rmarkdown 0.3.11
================================================================================

Initial release to CRAN
