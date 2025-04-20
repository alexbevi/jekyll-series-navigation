# Jekyll Series Plugin

> Seamlessly manage and navigate multi-part blog series in Jekyll using simple front matter and a reusable include.

---

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Include Partial](#include-partial)
- [Customization](#customization)
- [Contributing](#contributing)
- [License](#license)

---

## Introduction

The **Jekyll Series Plugin** enables authors to group related posts into a series and present a clear, navigable UI for readers. It leverages Jekyll’s Cache API for performance, stores series metadata in front matter and `_config.yml`, and renders via a lightweight Liquid include that you can style to match your theme.

## Features

- **Automatic Series Mapping**: Collects and sorts all posts sharing a `series` front-matter key.
- **Cache‑Backed Generation**: Uses `Jekyll::Cache` to avoid reprocessing on every build.
- **Config‑Driven Metadata**: Define per-series `title`, `description`, and `title_link` under `series_nav` in `_config.yml`.
- **Simple Include**: A single `{% series_nav %}` tag renders your series navigation using an `_includes/series_nav.html` partial.
- **Prev/Current/Next Links**: Highlights the current post and shows adjacent posts in the series.
- **Position Indicator**: Displays “Article X of Y in this series.”

## Installation

1. **Copy the plugin file** into your project:
   Place `_plugins/series_nav.rb` at the root of your Jekyll site.

2. **Add the include partial**:
   Create a file at `_includes/series_nav.html` containing the markup for your series box (see [Include Partial](#include-partial)).

3. **Configure your series** in `_config.yml`:
   ```yaml
   series_nav:
     my-slug:
       title: "My Deep Dive Series"
       description: "An in-depth exploration of topic X."
       title_link: "/series/{{ page.series }}/"
   ```

4. **Mark your posts**:
   In each post’s front matter, add:
   ```yaml
   series: my-slug
   ```

5. **Render the navigation**:
   In your layout (e.g. `post.html`), insert:
   ```liquid
   {% series_nav %}
   ```

6. **Build** your site:
   ```bash
   bundle exec jekyll build
   ```

## Configuration

Under `series_nav` in `_config.yml`, create one key per series slug. Each slug can include:

- `title` (string): Display name of the series.
- `description` (string): A short HTML‑safe overview.
- `title_link` (string): URL (or Liquid template) for linking the series title.

```yaml
series_nav:
  my-slug:
    title: "My Deep Dive Series"
    description: "An in-depth exploration of topic X."
    title_link: "/series/{{ page.series }}/"
```

## Usage

1. **Front-Matter**: Add `series: your-slug` to each post in the series.
2. **Include Tag**: Place `{% series_nav %}` where you want the navigation UI to appear.
3. **Customize**: Modify `_includes/series_nav.html` to tweak HTML structure or CSS classes.

## Include Partial

Below is the default `_includes/series_nav.html`. Drop this into your project verbatim, or adapt to your theme:

```html
<blockquote class="prompt-info mb-6">
  <strong>
    Series:
    {% if page.series_title_link %}
      <a href="{{ page.series_title_link }}">{{ page.series_title }}</a>
    {% else %}
      {{ page.series_title }}
    {% endif %}
  </strong>

  {% if page.series_description %}
    <p>{{ page.series_description }}</p>
  {% endif %}

  <ul class="list-none space-y-1">
    {% if page.series_prev %}
      <li>← <a href="{{ page.series_prev.url }}">{{ page.series_prev.title }}</a></li>
    {% endif %}

    <li><strong>{{ page.title }}</strong></li>

    {% if page.series_next %}
      <li>→ <a href="{{ page.series_next.url }}">{{ page.series_next.title }}</a></li>
    {% endif %}
  </ul>

  <p>Article {{ page.series_index }} of {{ page.series_posts | size }} in this series.</p>
</blockquote>
```

This plugin was designed to work best with the [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy), however the partial can be adapted as needed to suit your blog's layout.

## Customization

- **Styling**: Change CSS classes in the include to match your theme.
- **HTML Structure**: Override the include by creating your own `_includes/series_nav.html`.
- **Advanced Data**: Extend the plugin to pull in images or external metadata via the generator.

## Contributing

1. Fork the repository.
2. Create a feature branch (e.g. `feature-new-option`).
3. Commit your changes with clear messages.
4. Submit a pull request against `main`.

Please follow the existing code style and include tests where applicable.

## License

This plugin is released under the MIT License. See [LICENSE](LICENSE) for details.