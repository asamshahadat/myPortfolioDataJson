# myPortfolioDataJson

Single source of truth for S M Shahadat Hossain's portfolio projects. Any site
(React, plain HTML, PHP, WordPress…) can fetch this JSON and display it.
Update `portfolio.json`, push, and every site picks up the change — no rebuild.

## URL

```
https://asamshahadat.github.io/myPortfolioDataJson/portfolio.json
```

Images live in `images/<project-folder>/` and are referenced by full URL inside
the JSON, so they work from any domain.

## Fields

| Field          | Type     | Notes                                              |
| -------------- | -------- | -------------------------------------------------- |
| `id`           | number   | Unique; higher = newer                             |
| `images`       | string[] | Full image URLs; first one is the cover            |
| `title`        | string   |                                                    |
| `description`  | string   |                                                    |
| `category`     | string   | `wordpress` or `frontend`                          |
| `industry`     | string   | Optional; primary industry                         |
| `industries`   | string[] | Optional; every industry the project belongs to. Filter with `p.industries \|\| [p.industry]` |
| `project_type` | string   |                                                    |
| `link`         | string   | Live URL; may be empty                             |

Adding new fields is safe. Renaming or removing fields breaks sites that use them.

## Adding a project

1. Put screenshots in `images/<project-folder>/1.png`, `2.png`, …
2. Add an entry at the top of `portfolio.json` with the next `id` and full URLs:
   `https://asamshahadat.github.io/myPortfolioDataJson/images/<project-folder>/1.png`
3. Commit and push.

## Usage

### HTML / JavaScript

```html
<div id="projects"></div>
<script>
  fetch('https://asamshahadat.github.io/myPortfolioDataJson/portfolio.json')
    .then(res => res.json())
    .then(projects => {
      document.getElementById('projects').innerHTML = projects.map(p => `
        <div class="card">
          <img src="${p.images[0]}" alt="${p.title}">
          <h3>${p.title}</h3>
          <p>${p.description}</p>
        </div>
      `).join('');
    });
</script>
```

### PHP

```php
<?php
$projects = json_decode(
    file_get_contents('https://asamshahadat.github.io/myPortfolioDataJson/portfolio.json'),
    true
);
foreach ($projects as $p) {
    echo '<h3>' . htmlspecialchars($p['title']) . '</h3>';
}
```
