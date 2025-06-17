# Math Notes

A Jekyll-powered collection of mathematical notes and topics.

## Deployment

This site is designed to be deployed on GitHub Pages. To deploy:

1. Push changes to the `main` branch
2. Ensure GitHub Pages is enabled in repository settings
3. The site will be available at `https://yourusername.github.io/math-draft/`

## Local Development

To run locally:

```bash
bundle install
bundle exec jekyll serve
```

## Structure

- `topics/` - Individual topic pages
- `_includes/` - Reusable template components
- `_config.yml` - Site configuration
- `index.md` - Homepage

## Adding New Topics

1. Create a new `.md` file in the `topics/` directory
2. Add proper front matter with `layout`, `title`, and `permalink`
3. The topic will automatically appear in the topics index