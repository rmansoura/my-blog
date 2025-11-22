# GitHub Pages Learning Session - Complete Guide

**Date:** November 22, 2024  
**GitHub Username:** rmansoura  
**Repository:** my-github-blog

---

## Table of Contents

1. [What is GitHub Pages?](#what-is-github-pages)
2. [GitHub CLI Basics](#github-cli-basics)
3. [Jekyll Blog Structure](#jekyll-blog-structure)
4. [Understanding gh api Command](#understanding-gh-api-command)
5. [Creating Your Blog](#creating-your-blog)
6. [Markdown Reference](#markdown-reference)
7. [Next Steps](#next-steps)

---

## What is GitHub Pages?

GitHub Pages is a **free hosting service** that turns your GitHub repository into a live website.

**Perfect for:**
- Personal portfolios
- Project documentation
- Simple blogs
- Landing pages

**Key Concept:** Your code lives in a GitHub repository, and GitHub automatically publishes it as a website.

---

## GitHub CLI Basics

### Repository Management

```bash
# Create a new repository
gh repo create repo-name --public --clone --add-readme

# Delete a repository
gh repo delete owner/repo-name

# Delete without confirmation
gh repo delete owner/repo-name --yes

# List your repositories
gh repo list

# View repository in browser
gh repo view --web
```

### Repository Settings

```bash
# Edit repository settings
gh repo edit --description "My awesome project"
gh repo edit --visibility private

# Enable/disable features
gh repo edit --enable-issues
gh repo edit --disable-wiki

# Browse to settings pages
gh browse -- /settings              # General settings
gh browse -- /settings/pages        # Pages settings
gh browse -- /settings/branches     # Branch protection
```

### Working with Issues and PRs

```bash
# Create an issue
gh issue create --title "Bug fix" --body "Description"

# Create a pull request
gh pr create --title "New feature"

# Merge a PR
gh pr merge 123
```

---

## Jekyll Blog Structure

### Complete File Structure

```
your-repo/
├── _config.yml                           ← Blog configuration
├── index.md                              ← Your home page
└── _posts/                               ← Folder for all blog posts
    ├── 2024-11-22-my-first-post.md      ← A blog post
    ├── 2024-11-21-second-post.md        ← Another blog post
    └── 2024-11-20-third-post.md         ← Another blog post
```

---

### 1. `_config.yml` - Blog Settings

**Location:** Root of repository

**Basic Configuration:**
```yaml
title: My Awesome Blog
description: Where I share my thoughts and learning
theme: minima
```

**Extended Configuration:**
```yaml
title: My Awesome Blog
description: Where I share my thoughts and learning
theme: minima

author: Your Name
email: you@example.com
github_username: yourusername
twitter_username: yourusername

# How many posts to show per page
paginate: 5
```

**What Each Line Does:**
- `title:` - Your blog's name (appears at top of every page)
- `description:` - Your blog's tagline/subtitle
- `theme:` - The design/style (minima, cayman, slate, midnight, architect)
- `author:` - Your name
- `email:` - Contact email (optional)

---

### 2. `index.md` - Home Page

**Location:** Root of repository

**Minimal Version:**
```markdown
---
layout: home
---
```

**With Welcome Message:**
```markdown
---
layout: home
---

Welcome to my blog! I write about learning and technology.
```

**What `layout: home` Does:**
- Automatically lists all blog posts
- Shows post titles, dates, and excerpts
- Makes titles clickable links
- Sorts posts by date (newest first)

---

### 3. Blog Posts in `_posts/`

**Important Rules:**

1. **Folder name MUST be:** `_posts`
2. **Filename format MUST be:** `YYYY-MM-DD-title-of-post.md`

**Valid Filenames:**
- ✅ `2024-11-22-hello-world.md`
- ✅ `2024-01-05-learning-jekyll.md`
- ❌ `my-post.md` (no date)
- ❌ `11-22-2024-hello.md` (wrong date order)
- ❌ `2024-11-22 hello world.md` (spaces instead of dashes)

**Post Structure:**

```markdown
---
layout: post
title: "My First Blog Post"
date: 2024-11-22
---

# Welcome to My Blog

This is my first post. I'm learning about Jekyll!

## What I Learned Today

- Jekyll uses Markdown
- Posts go in the `_posts` folder
- Filenames need dates

**That's it for today!**
```

**Two Required Sections:**

1. **Front Matter** (between `---` marks):
   - `layout: post` - Required for blog posts
   - `title: "Your Title"` - Post title (use quotes)
   - `date: YYYY-MM-DD` - Publication date

2. **Content** (after second `---`):
   - Your blog post written in Markdown
   - Can include headings, lists, links, images, code, etc.

---

## Understanding gh api Command

### What is `gh api`?

The `gh api` command lets you directly interact with GitHub's REST API.

### Complete Command Breakdown

```bash
gh api repos/rmansoura/my-github-blog/pages -X POST -f source[branch]=main -f source[path]=/
```

**Breaking It Down:**

| Part | Meaning |
|------|---------|
| `gh api` | Make an API call |
| `repos/rmansoura/my-github-blog/pages` | API endpoint (what to access) |
| `-X POST` | HTTP method (create something) |
| `-f source[branch]=main` | Field: use main branch |
| `-f source[path]=/` | Field: serve from root directory |

**Translation:** "Hey GitHub API, create a Pages site for `rmansoura/my-github-blog`, using the `main` branch and serving files from the root directory."

---

### HTTP Methods

```bash
GET     # Read/retrieve data (default, no -X needed)
POST    # Create something new
PATCH   # Update existing data
DELETE  # Delete something
```

---

### Common API Endpoints

```bash
# Enable GitHub Pages
gh api repos/{owner}/{repo}/pages -X POST -f source[branch]=main -f source[path]=/

# Check Pages status
gh api repos/{owner}/{repo}/pages

# Disable Pages
gh api repos/{owner}/{repo}/pages -X DELETE

# View repository info
gh api repos/{owner}/{repo}

# List your repositories
gh api user/repos

# Update repository description
gh api repos/{owner}/{repo} -X PATCH -f description="New description"
```

---

### The `-f` Flag (Sending Data)

```bash
-f key=value          # Send a string field
-F key=@file.txt      # Send file contents
-f 'key=value'        # Use quotes if value has spaces
```

**Examples:**
```bash
# Single field
gh api repos/owner/repo -X PATCH -f description="My new description"

# Multiple fields
gh api repos/owner/repo/pages -X POST -f source[branch]=main -f source[path]=/

# With spaces in value
gh api repos/owner/repo -X PATCH -f description="This has spaces"
```

---

### API Response Example

```bash
gh api repos/rmansoura/my-github-blog/pages
```

**Output (JSON):**
```json
{
  "url": "https://api.github.com/repos/rmansoura/my-github-blog/pages",
  "status": "built",
  "html_url": "https://rmansoura.github.io/my-github-blog/",
  "source": {
    "branch": "main",
    "path": "/"
  }
}
```

---

## Creating Your Blog

### Step-by-Step Commands

**1. Create Repository:**
```bash
gh repo create my-github-blog --public --clone --add-readme --description "Learning GitHub Pages and Jekyll"
```

**2. Navigate to Repository:**
```bash
cd my-github-blog
```

**3. Create `_config.yml`:**
```bash
echo "title: My Learning Blog
description: Sharing my journey learning GitHub Pages
theme: minima" > _config.yml
```

**4. Create `index.md`:**
```bash
echo "---
layout: home
---

Welcome to my blog! I'm learning GitHub Pages and Jekyll." > index.md
```

**5. Create `_posts` Folder:**
```bash
mkdir _posts
```

**6. Create First Blog Post:**
```bash
echo "---
layout: post
title: \"My First Post\"
date: 2024-11-22
---

# Hello World!

This is my first blog post using GitHub Pages and Jekyll.

## What I learned today

- How to create a GitHub repo with gh CLI
- The structure of a Jekyll blog
- How to write in Markdown

**This is exciting!**" > _posts/2024-11-22-my-first-post.md
```

**7. Check Your Structure:**
```bash
ls -la
ls _posts
```

**8. Commit and Push:**
```bash
git add .
git commit -m "Add Jekyll blog structure and first post"
git push
```

**9. Enable GitHub Pages:**
```bash
gh api repos/rmansoura/my-github-blog/pages -X POST -f source[branch]=main -f source[path]=/
```

**10. Visit Your Live Site:**
```
https://rmansoura.github.io/my-github-blog/
```

Wait 1-2 minutes for GitHub to build your site.

---

### Git Warnings (Windows)

```
warning: in the working copy of '_config.yml', LF will be replaced by CRLF
```

**This is normal on Windows!** Git is converting line endings. It doesn't break anything.

**To silence these warnings (optional):**
```bash
git config --global core.autocrlf true
```

---

## Markdown Reference

### Basic Syntax

```markdown
# Heading 1
## Heading 2
### Heading 3

**bold text**
*italic text*
***bold and italic***

- Bullet point
- Another point
  - Nested point

1. Numbered list
2. Second item
3. Third item

[Link text](https://example.com)

![Image alt text](image.jpg)

`inline code`

```
code block
```

> This is a quote
> It can span multiple lines

---

Horizontal line (three dashes)
```

### Advanced Markdown

```markdown
# Links
[Link with title](https://example.com "Hover title")
[Link to heading](#heading-name)

# Images
![Alt text](url "Optional title")

# Tables
| Header 1 | Header 2 |
|----------|----------|
| Cell 1   | Cell 2   |
| Cell 3   | Cell 4   |

# Task Lists
- [x] Completed task
- [ ] Incomplete task

# Code with Syntax Highlighting
```python
def hello():
    print("Hello World")
```

# Footnotes
Here's a sentence with a footnote[^1].

[^1]: This is the footnote.
```

---

## How It All Works Together

### The Flow

```
You write → Markdown (.md files)
     ↓
Jekyll converts → HTML
     ↓
GitHub Pages serves → Live website
```

### File Relationships

1. **`_config.yml`** sets your blog's name, description, and theme
2. **`index.md`** is your home page that automatically lists all posts
3. **`_posts/*.md`** are your individual blog posts
4. Jekyll reads all three and generates a complete website
5. GitHub Pages hosts the generated HTML files

### What Jekyll Does Automatically

- Converts Markdown to HTML
- Applies your chosen theme
- Creates navigation between posts
- Generates post listings
- Adds dates and metadata
- Creates RSS feeds
- Builds a complete website structure

---

## Next Steps

### Adding More Posts

Create a new post:
```bash
echo "---
layout: post
title: \"My Second Post\"
date: 2024-11-23
---

# Day Two

Today I successfully published my first blog!

## What's next?

- Learn more about Jekyll themes
- Customize my blog
- Write more posts

Stay tuned!" > _posts/2024-11-23-my-second-post.md

git add .
git commit -m "Add second blog post"
git push
```

### Customizing Your Blog

**Change Theme:**
Edit `_config.yml`:
```yaml
theme: cayman  # or slate, midnight, architect
```

**Add Author Info:**
```yaml
author:
  name: Your Name
  email: you@example.com
social:
  github: rmansoura
  twitter: yourusername
```

### Adding Pages (Not Posts)

Create additional pages at the root level:

**about.md:**
```markdown
---
layout: page
title: About
permalink: /about/
---

# About Me

I'm learning web development and sharing my journey.
```

**contact.md:**
```markdown
---
layout: page
title: Contact
permalink: /contact/
---

# Get In Touch

Email me at: your@email.com
```

### Adding Images to Posts

1. Create an `assets` or `images` folder
2. Add your images
3. Reference in posts:

```markdown
![Description](assets/my-image.jpg)
```

### Learning Resources

- **Jekyll Documentation:** https://jekyllrb.com/docs/
- **GitHub Pages Docs:** https://docs.github.com/pages
- **Markdown Guide:** https://www.markdownguide.org/
- **GitHub API Docs:** https://docs.github.com/en/rest

---

## Quick Reference Commands

### Repository Management
```bash
gh repo create <name> --public --clone
gh repo delete <owner>/<repo>
gh repo list
gh repo view --web
```

### Git Workflow
```bash
git add .
git commit -m "message"
git push
git status
git log
```

### GitHub Pages
```bash
# Enable Pages
gh api repos/<owner>/<repo>/pages -X POST -f source[branch]=main -f source[path]=/

# Check Pages status
gh api repos/<owner>/<repo>/pages

# Disable Pages
gh api repos/<owner>/<repo>/pages -X DELETE

# Open Pages settings
gh browse -- /settings/pages
```

### File Operations
```bash
mkdir folder_name
touch file_name
cat file_name
ls -la
pwd
cd folder_name
```

---

## Troubleshooting

### Site Not Updating?

1. Wait 1-2 minutes after pushing
2. Hard refresh browser (Ctrl + F5)
3. Check Pages status:
   ```bash
   gh api repos/rmansoura/my-github-blog/pages
   ```
4. Look for `"status": "built"` in output

### Post Not Appearing?

1. Check filename format: `YYYY-MM-DD-title.md`
2. Verify front matter has `layout: post`
3. Ensure file is in `_posts/` folder
4. Check date isn't in the future

### Theme Not Working?

1. Check spelling in `_config.yml`
2. Make sure theme is supported by GitHub Pages
3. Try `theme: minima` (always works)

---

## What You Accomplished

✅ Created a GitHub repository using CLI  
✅ Built Jekyll blog structure  
✅ Wrote your first blog post in Markdown  
✅ Pushed to GitHub  
✅ Enabled GitHub Pages via API  
✅ Published a live website!  

**Your Live Blog:** https://rmansoura.github.io/my-github-blog/

---

## Notes and Observations

- LF/CRLF warnings on Windows are normal and can be ignored
- GitHub Pages builds take 1-2 minutes
- Jekyll automatically converts Markdown to HTML
- The `_posts` folder name is required (exact spelling)
- Post filenames must include dates in YYYY-MM-DD format
- Front matter must be between `---` marks
- Changes require `git push` to go live

---

**Last Updated:** November 22, 2024  
**Status:** Blog successfully created and published ✅