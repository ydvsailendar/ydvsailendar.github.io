# Shailendra Yadav — DevSecOps & SRE Portfolio

Static site ready for GitHub Pages or AWS Amplify. Includes portfolio sections, projects page, blog with markdown posts, and per-page favicon/avatar.

## Structure
- `index.html` — main landing page (hero, experience timeline, skills, featured project, blog list, contact).
- `projects.html` — full project list.
- `post.html` — blog post reader; open with `post.html?slug=<slug>`.
- `styles.css` — global styles and responsive layout.
- `posts/` — blog markdown (`*.md`).
- `assets/site/` — shared images (`profile.jpg`, `favicon.png`).
- `assets/blog/` — put blog-specific images here.

## Running locally
Open `index.html` in a browser. For fetch to work in some browsers, use a local server:
```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying to GitHub Pages
1) Commit/push to GitHub.  
2) Repo Settings → Pages → Source: Deploy from a branch. Branch: `main` (or default), folder: `/ (root)`.  
3) Visit the published URL: `https://<username>.github.io/<reponame>/` (or `https://<username>.github.io/` if the repo name matches your username).

## Adding a blog post
1) Create `posts/<slug>.md` with your content.  
2) Add the post metadata in:
   - `index.html` → `posts` array (slug, title, date, tags, summary).
   - `post.html` → `posts` map and `postContent` inline fallback.  
3) Access at `post.html?slug=<slug>`.

## Assets
- Update `assets/site/profile.jpg` to change the nav avatar and favicon source.
- Regenerate favicon if desired: create a square `assets/site/favicon.png`.

## Contact
`yadavshailendra0905@gmail.com` | `(+44) 7503117315`
