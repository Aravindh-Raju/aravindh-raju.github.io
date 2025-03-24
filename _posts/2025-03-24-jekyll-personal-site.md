---
title: "Setting Up a Jekyll Personal Website with Minimal Mistakes Theme"
categories:
  - Blog
tags:
  - Tech
  - Jekyll
---

Hey all! In this guide, we will walk through setting up a Jekyll-based personal website using the Minimal Mistakes theme. We will cover installation, customization, enabling key features like blogs and categories, deploying to GitHub Pages, and even setting up a custom GoDaddy domain.


## Prerequisites

Before getting started, ensure you have the following installed:

- [Ruby](https://www.ruby-lang.org/en/downloads/)
- [Bundler](https://bundler.io/)
- [Jekyll](https://jekyllrb.com/docs/installation/)
- [Git](https://git-scm.com/downloads)
- [GitHub Account](https://github.com/Aravindh-Raju)

## Step 1: Fork the Minimal Mistakes Starter Repository

Minimal Mistakes is a popular Jekyll theme. The easiest way to start is by forking its starter repository:

1. Go to [Minimal Mistakes Starter](https://github.com/mmistakes/mm-github-pages-starter.git)
2. Click the **Fork** button (top right corner)
3. Rename your forked repository if needed

## Step 2: Clone and Set Up the Project

```sh
# Clone your forked repository
$ git clone https://github.com/YOUR-USERNAME/minimal-mistakes
$ cd minimal-mistakes

# Install dependencies
$ bundle install
```

## Step 3: Customize Your Website

### Update `_config.yml`

Modify `_config.yml` to reflect your personal information:

```yaml
title: "Your Name"
url: "https://yourusername.github.io"
theme: minimal-mistakes-jekyll
```

### Modify `_data/navigation.yml`

Update navigation links:

```yaml
main:
  - title: "Home"
    url: /
  - title: "Blog"
    url: /year-archive/
  - title: "About"
    url: /about/
  - title: "Contact"
    url: /contact/
  - title: "Categories"
    url: /categories/
  - title: "Search"
    url: /search/
```

## Step 4: Enable Features

### Blog
Minimal Mistakes includes a **blog** section where you can post articles. To create a new post, add a Markdown file inside `_posts/` with the following format:

```md
---
title: "My First Blog Post"
date: 2025-03-24
tags: [jekyll, minimal-mistakes]
categories: [blog]
---

This is the content of your first post!
```

### About Page
Create an `about.md` file in the root directory:

```md
---
layout: single
title: "About Me"
permalink: /about/
---

Write about yourself here!
```

### Categories & Tags
Minimal Mistakes supports **categories** and **tags**. Ensure you have `_pages/categories.md`:

```md
---
layout: categories
title: "Categories"
permalink: /categories/
---
```

### Search
Enable **search** by adding `_pages/search.md`:

```md
---
layout: search
title: "Search"
permalink: /search/
---
```

Then, ensure `search: true` is enabled in `_config.yml`.

## Step 5: Run Jekyll Locally

To preview the site before deploying:

```sh
$ bundle exec jekyll serve
```

Then open `http://localhost:4000` in your browser.

## Step 6: Deploy to GitHub Pages

### Enable GitHub Pages

1. Go to your repository on GitHub
2. Navigate to **Settings > Pages**
3. Under **Source**, select `gh-pages` (or `main` if applicable)
4. Click **Save**

### Push Changes to GitHub

```sh
$ git add .
$ git commit -m "Initial commit"
$ git push origin main
```

Your website should be live at `https://yourusername.github.io/` after a few minutes.

## Bonus: Use a GoDaddy Custom Domain

### Step 1: Buy a Domain from GoDaddy

1. Go to [GoDaddy](https://www.godaddy.com/)
2. Purchase a domain (e.g., `yourdomain.com`)

### Step 2: Configure GitHub Pages with GoDaddy

1. In your GitHub repository, create a `CNAME` file in the root directory:
   ```sh
   echo "yourdomain.com" > CNAME
   git add CNAME
   git commit -m "Add custom domain"
   git push origin main
   ```
2. Go to GoDaddy DNS settings
3. Add the following **A Records** (remove any existing A records):
   ```
   Type: A
   Name: @
   Value: 185.199.108.153
   Type: A
   Name: @
   Value: 185.199.109.153
   Type: A
   Name: @
   Value: 185.199.110.153
   Type: A
   Name: @
   Value: 185.199.111.153
   ```
4. Add a **CNAME Record**:
   ```
   Type: CNAME
   Name: www
   Value: yourusername.github.io
   ```

Your custom domain should be working within 24 hours!

---

That's it! Your Jekyll site is now live on GitHub Pages with a custom domain and essential features like a blog, about page, categories, and search.
