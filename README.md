# README

Scrape Trader's Glossary from IBKR



## Requirements

- Google Chrome
- [Webscraper](https://webscraper.io/) Chrome extension
- Nushell
- pandoc



## Usage

- create sitemap in Webscraper
- scrape in Webscraper
- export data as CSV
- parse to JSON

```nu
open data.csv
  | select title text term-href
  | rename title value url
  | str replace -r '<form(?s).*form>$' '' value
  | update value {|row| $row.value | pandoc --wrap=none --strip-comments -f html-native_divs-native_spans -t gfm-tex_math_dollars-raw_html }
  | sort-by title
  | to json
  | save data.json
```
