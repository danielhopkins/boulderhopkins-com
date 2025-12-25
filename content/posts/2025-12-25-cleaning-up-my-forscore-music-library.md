---
date: '2025-12-25T10:00:00-07:00'
draft: false
title: 'Cleaning Up My forScore Music Library'
tags:
    - music
    - rust
    - automation
---

Over the past couple days, I've been on a mission to clean up my music library in [forScore](https://forscore.co/), the sheet music reader I use on my iPad. What started as a simple organization project turned into building custom tooling and developing ideas for how forScore could better support power users and integrations.

## The Problem

My forScore library had grown organically over the years, resulting in:
- **Inconsistent naming**: `Op. 28 No. 4` vs `Op.28 No.4` vs `Prelude in E minor`
- **Missing metadata**: Many pieces lacked key signatures
- **Duplicate bookmarks**: 324 duplicate entries from various imports
- **Composer names in titles**: Redundant when the metadata already has it

Finding specific pieces or organizing practice sessions was becoming frustrating.

## Creating Naming Conventions

I started by documenting my naming conventions in Obsidian. The goal was to create a consistent system that would:

- Make pieces easy to find
- Support sorting and filtering
- Include relevant metadata in a predictable format

The format I settled on:

```
[Catalog] [No.] – [Name]
```

For example:
- `BWV 772 – Invention No. 1`
- `Op. 28 No. 4 – Prelude in E minor`
- `K. 331 – Piano Sonata No. 11`

Key rules:
1. Always use periods after catalog abbreviations: `Op.` not `Op`
2. Use en-dash (–) not hyphen (-) as separator
3. Include piece number with `No.` when part of a set
4. Composer is stored in forScore metadata, not the title

I also created a reference table of catalog prefixes by composer—BWV for Bach, K. for Mozart, Op. for most Romantic composers, S. for Liszt, D. for Schubert, and so on.

## Building the forScore CLI in Rust

With naming conventions defined, I needed a way to apply them to my existing library. forScore is an iPad app, but it runs on macOS via Apple Silicon—which means its data files are accessible on my Mac.

The catch: there's no official API for listing or editing scores. forScore has a [URL scheme](https://forscore.co/developers/) for opening specific scores, but nothing for querying the library or modifying metadata programmatically. So I built a CLI tool in Rust that edits the database directly: [forscore-cli](https://github.com/danielhopkins/forscore-cli).

### The Challenge: Dual Storage

forScore uses two storage mechanisms:
1. **SQLite database** (`forScore.sqlite`) - the main library index
2. **ITM files** - per-score metadata files that sync via iCloud

Simply updating the database isn't enough—you also need to update the corresponding `.itm` files, or the changes get overwritten when iCloud syncs. This took some reverse engineering to figure out. The ITM files use Apple's binary plist format, which Rust can parse with the `plist` crate.

### Key Commands

```bash
# List all scores
forscore scores ls

# Search for pieces
forscore scores search "chopin"

# Edit metadata
forscore scores edit 522 --title "BWV 772 – Invention No. 1"
forscore bookmarks edit 123 --key "C Major"

# Export for analysis
forscore export csv

# Check sync status
forscore sync log
```

### Development Iterations

The CLI went through several iterations:
1. **First attempt**: Direct SQLite updates. Failed because iCloud would overwrite changes.
2. **Second attempt**: Update both SQLite and ITM files. Worked, but the plist serialization was tricky.
3. **Final version**: Proper handling of the binary plist format with correct field mappings.

One gotcha: forScore uses integer codes for keys (C Major = 100, C Minor = 121, etc.), so I had to build a lookup table for human-readable key names.

## Cleaning Up Duplicates

The first task was removing duplicate bookmarks. Over time, I'd imported the same PDFs multiple times, creating duplicate entries with identical page ranges.

Using the CLI's export feature, I grouped bookmarks by (path, title, start_page, end_page) and found **324 duplicates** across 215 groups. A quick script deleted the extras, keeping one of each.

## Filling in Missing Key Signatures

Many of my scores were missing key signature metadata. I wanted to automate filling this in, so I explored using online music databases.

### The OpenOpus Experiment

[OpenOpus](https://openopus.org/) is a free, open-source classical music database with an API. My idea was to:
1. Parse the piece title to extract composer and work info
2. Query OpenOpus for matching works
3. Extract the key from the returned data

The reality was messier. OpenOpus embeds keys in title strings (e.g., "Piano Sonata No. 14 in C-sharp minor") rather than as separate fields. And for collections like Chopin's Preludes Op. 28, the API returns the opus as a single work—it doesn't have entries for individual preludes.

### Building a Knowledge Base

Instead of relying entirely on the API, I built a Python script (`forscore-organizer`) that combines:
1. **Built-in knowledge**: A dictionary of known keys for common works, generated with Claude's help—it knows the keys for all 24 Chopin Preludes, Bach Inventions, and other standard repertoire
2. **OpenOpus fallback**: Query the API and try to parse keys from titles
3. **Dry-run mode**: Preview all changes before applying

### Results
These approaches were not particularly effective. I could only find the keys for 32 of my pieces. More research is needed to figure out how to look these up.

## Batch Renaming to Match Conventions

With the CLI and naming convention documented, I wrote a Python script to find and fix all naming violations. The script generated corrections and applied them in one batch—a task that would have taken hours manually.

## Ideas for forScore Integration Improvements

While building my CLI, I kept running into limitations with how forScore exposes its data and functionality. Here are some ideas for how forScore could make it easier for power users to integrate with their workflows:

### URL Scheme Enhancements

forScore already supports [URL schemes](https://forscore.co/developers/) for basic actions, but expanding these would unlock powerful automation possibilities:

- **List endpoints**: URLs that return JSON lists of scores, setlists, bookmarks
- **Edit endpoints**: URLs to modify metadata without opening the app UI
- **Sync tool triggers**: URLs to initiate cloud sync, library scans, or other maintenance tasks
- **Query support**: Filter and search via URL parameters

### Example Use Cases

With expanded URL support, users could:

- Build custom Shortcuts automations for library management
- Create web dashboards that display practice statistics
- Integrate with other music tools and databases
- Automate backup and sync workflows

## Conclusion

What started as "I should organize my sheet music" turned into:
- A [Rust CLI](https://github.com/danielhopkins/forscore-cli) for editing forScore's database
- A Python script for batch metadata operations
- Documentation for my naming conventions
- Ideas for how forScore could better support power users

The library is now consistent and searchable. More importantly, I have tools to keep it that way as I add new pieces.

The whole project was a good reminder that sometimes the "simple" organization task reveals interesting technical challenges—and that building the right tools pays dividends over time.

---

*Have you built tooling around forScore or other music apps? I'd love to hear about your approaches to organizing sheet music libraries.*
