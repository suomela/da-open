# DA Open

This repository is publicly available at <https://github.com/suomela/da-open>.

All Markdown files must be compatible with whatever GitHub supports (especially important for Latex math).

## Math markup and validation

- Follow [GitHub's math syntax](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions). Use dollar-and-backtick delimiters for inline math and fenced `math` blocks for display equations, as in the question files. Do not use `\(...\)` or `\[...\]` as Markdown math delimiters.
- GitHub imposes additional restrictions on LaTeX macros. Its browser renderer currently rejects `\operatorname` with “The following macros are not allowed: operatorname”. Use `\mathrm{Rand}` for the named function and `\mathrm{polylog}\,n` for the polylogarithm expression.
- Check every affected page in a real browser after math edits. A successful response from GitHub's Markdown API confirms parsing, but does not guarantee that the final math rendering succeeds.
- Inspect every `math-renderer` element, including formulas in tables, links, bold text, and display blocks. Each must finish rendering. Check for GitHub's `.flash-error` messages and math error elements such as `merror`, `mjx-merror`, and `[data-mjx-error]`; browser console errors alone do not catch rejected macros.
- To check local changes before publishing, render them with GitHub's Markdown API and insert the returned HTML into the Markdown article of an open GitHub page. Let GitHub's own browser renderer process the new elements and inspect the result. Preserve the mathematical content when replacing unsupported notation.
