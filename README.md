# Krishna's Blog

A personal blog about code, data, and the ideas behind building systems.

## Running Locally

### Prerequisites

- **Ruby 3.0+** (install via Homebrew if needed: `brew install ruby`)

### Setup

1. Install dependencies:
   ```bash
   bundle config set --local path 'vendor/bundle'
   bundle install
   ```

2. Start the development server:
   ```bash
   bundle exec jekyll serve
   ```

   The site will be available at **http://127.0.0.1:4000/**

3. Files rebuild automatically when edited. Stop with `Ctrl+C`.

## Customization

- **Site settings**: Update `_config.yml` for title, description, author, and social links
- **Posts**: Add `.md` files to `_posts/` with front matter (see existing posts for examples)
- **Styling**: Edit `_sass/styles.scss` for appearance changes
- **Layouts**: Customize templates in `_layouts/`

## License

MIT
