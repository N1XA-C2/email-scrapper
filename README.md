# 📧 Email Scraper

A lightweight Python-based website crawler that extracts publicly exposed email addresses from pages within a specified domain.

The scraper follows internal links, extracts email addresses from HTML content, supports multithreading, and can optionally save the results to a file.

> **⚠️ Responsible Use**
>
> This project is intended for educational purposes, security research, and authorized website testing.
>
> Only use it against websites you own or have explicit permission to test. Crawling websites without authorization may violate their terms of service, privacy expectations, or applicable laws.
>
> **Account notice:** This repository's author previously had a GitHub account suspended in connection with publishing hacking/security-related scripts. This project is therefore presented with an explicit emphasis on authorized and responsible use.

## ✨ Features

* 🔎 Extracts email addresses from HTML pages
* 🕷️ Crawls internal links within the target domain
* 🧵 Supports multithreaded crawling
* 🚫 Avoids processing the same URL more than once
* 📊 Configurable maximum number of URLs
* 💾 Optional output to a text file
* 🎨 Colored terminal output
* ⏱️ Displays total crawling time
* 🌐 Uses a persistent `requests.Session`

## 🛠️ Requirements

* Python 3.8+
* `requests`
* `beautifulsoup4`
* `lxml`
* `termcolor`

Install the dependencies with:

```bash
pip install requests beautifulsoup4 lxml termcolor
```

Or, if you use Arch Linux:

```bash
sudo pacman -S python python-requests python-beautifulsoup4 python-lxml python-termcolor
```

## 📥 Installation

Clone the repository:

```bash
git clone https://gitlab.com/N1XA-CLI/email-scrapper.git
cd email-scrapper
```

Then install the required dependencies.

Make the script executable if desired:

```bash
chmod +x scraper.py
```

## 🚀 Usage

Basic usage:

```bash
python scraper.py -d https://scrap-me.netlify.app/
```

Or, if executable:

```bash
./scraper.py -d https://scrap-me.netlify.app/
```

### Command-line options

| Option | Long option | Description                             |  Default |
| ------ | ----------- | --------------------------------------- | -------: |
| `-d`   | `--domain`  | Target domain to crawl                  | Required |
| `-l`   | `--limit`   | Maximum number of URLs to crawl         |     `20` |
| `-w`   | `--write`   | File in which to save discovered emails |     None |
| `-t`   | `--thread`  | Number of concurrent crawling threads   |      `5` |

### Examples

Crawl up to 20 pages:

```bash
python scraper.py -d https://scrap-me.netlify.app/
```

Crawl up to 100 pages:

```bash
python scraper.py -d https://scrap-me.netlify.app/ -l 100
```

Use 10 threads:

```bash
python scraper.py -d https://scrap-me.netlify.app/ -t 10
```

Save discovered emails:

```bash
python scraper.py -d https://scrap-me.netlify.app/ -w emails.txt
```

Combine options:

```bash
python scraper.py -d https://scrap-me.netlify.app/ -l 100 -t 10 -w emails.txt
```

## ⚙️ How It Works

The scraper follows a simple crawling pipeline:

```text
Target URL
    │
    ▼
Download HTML
    │
    ├──────────────► Extract email addresses
    │
    ▼
Extract <a> links
    │
    ▼
Normalize URLs
    │
    ▼
Keep only same-domain links
    │
    ▼
Add unvisited URLs to queue
    │
    ▼
Repeat until URL limit is reached
```

### 1. URL queue

A `deque` stores URLs waiting to be processed.

### 2. Email extraction

The program searches downloaded page content using a regular expression matching common email-address formats.

### 3. Link extraction

BeautifulSoup parses the HTML and collects links from `<a>` elements.

### 4. Same-domain restriction

Only links belonging to the original domain are added to the crawling queue.

### 5. Duplicate prevention

Two sets are used to prevent unnecessary processing:

* `scrapped_links` — URLs that have already been processed
* `scrapped_emails` — email addresses that have already been discovered

### 6. Multithreading

The crawler starts several worker threads for each batch of URLs, allowing multiple pages to be requested concurrently.

## 📂 Output

When `-w` is specified, each discovered email is written to a separate line:

```text
contact@example.com
admin@example.com
support@example.com
```

The terminal also displays discovered addresses and basic crawling statistics.

## ⚠️ Limitations

This is intentionally a relatively simple crawler.

It currently:

* Processes HTML pages only
* Does not execute JavaScript
* Does not handle authentication
* Does not parse emails hidden behind JavaScript applications
* Does not implement sophisticated robots.txt handling
* Uses a relatively simple email regex
* Does not implement advanced rate limiting
* Does not maintain a persistent crawl database
* Does not guarantee that every discovered string is a valid mailbox

The `-t` option increases concurrency, so using a large number of threads can place significant load on a server.

## 🔐 Responsible Crawling

Before running the scraper against a website, make sure you are authorized to crawl it.

For your own test environment, you can use a deliberately vulnerable or locally hosted website and observe how the crawler discovers pages and email addresses.

When testing against third-party infrastructure, respect:

* Authorization requirements
* Terms of service
* `robots.txt` where applicable
* Rate limits
* Server resources
* Privacy requirements
* Applicable laws and regulations

## 📜 License

Add the license you want to use for this project.

For example:

```text
MIT License
```

See `LICENSE` for the complete license text.

## 👤 Author

**N1XA-CLI**

Built as a Python crawling and web-data extraction project for learning about:

* HTTP requests
* HTML parsing
* URL handling
* Regular expressions
* Web crawling
* Python threading
* CLI application design
* Data extraction
