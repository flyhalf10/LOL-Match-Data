# AGENTS.md

## Cursor Cloud specific instructions

### Project overview

Single-file R Markdown project (`pull data.Rmd`) that fetches League of Legends match data from the Riot Games API and produces an HTML summary report.

### System dependencies

- **R** (4.3+) with packages: `httr`, `jsonlite`, `dplyr`, `purrr`, `tibble`, `knitr`, `rmarkdown`
- **pandoc** (required by `rmarkdown` for HTML rendering)
- **System libs**: `libcurl4-openssl-dev`, `libssl-dev`, `libxml2-dev` (needed to compile R packages from source)

### Running / rendering

```bash
# Render the Rmd to HTML (requires RIOT_API_KEY env var):
RIOT_API_KEY="RGAPI-your-key" Rscript -e 'rmarkdown::render("pull data.Rmd", output_file="pull_data.html")'
```

### Key caveats

- **`RIOT_API_KEY` is required.** The document calls `stop()` if the env var is empty. Obtain a development key from https://developer.riotgames.com. Dev keys expire every 24 hours.
- **Rate limiting:** The script sleeps 1.2 s between match-detail requests to stay within Riot dev-key rate limits. A full 20-match render takes ~25 seconds.
- **No linter config exists.** To check for syntax errors, extract R code with `knitr::purl()` and run `parse()` on the result.
- **No automated tests exist.** Validation is done by rendering the document end-to-end.
- The file name contains a space (`pull data.Rmd`); always quote the path.
