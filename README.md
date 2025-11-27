# boulder dan

Personal tech blog at [boulderhopkins.com](https://boulderhopkins.com).

## Tech Stack

- **Static Site Generator:** [Hugo](https://gohugo.io/)
- **Theme:** [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
- **Hosting:** [Vercel](https://vercel.com/)
- **DNS:** Cloudflare

## Local Development

```bash
# Install Hugo (macOS)
brew install hugo

# Run development server
hugo server -D

# Build for production
hugo
```

## Deployment

Pushes to `main` trigger automatic deployments via Vercel.

## Content

Blog posts are in `content/posts/` as Markdown files with YAML front matter.

## License

Content is copyright Dan Hopkins. Code is MIT licensed.
