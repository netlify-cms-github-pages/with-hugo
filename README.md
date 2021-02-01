# Hugo with Netlify CMS and Github Pages

An example of using Hugo with Netlify CMS and Github Pages. This allows you to use Netlify CMS to modify a repo on Github and without Netlify Identity.

This repo is setup but if you want to do it to your own Hugo repo you need to:

Set `publishDir` to `docs` in `config.toml`

Create `static/admin/config.yml` with the following:
```yml
backend:
  name: github
  repo: <your github name and repo e.g. acme/test>
  branch: <the github repo e.g. main>
  #site_domain: <a site from netlify.com e.g. pensive-tortoise-62babc>

media_folder: "images/uploads"

collections:
  - name: "blog" # Used in routes, e.g., /admin/collections/blog
    label: "Blog" # Used in the UI
    folder: "content/blog/" # The path to the folder where the documents are stored
    create: true # Allow users to create new documents in this collection
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}" # Filename template, e.g., YYYY-MM-DD-title.md
    fields: # The fields for each document, usually in front matter
      - {label: "Title", name: "title", widget: "string"}
      - {label: "Publish Date", name: "date", widget: "datetime"}
      - {label: "Tags", name: "tags", widget: "list"}
      - {label: "Body", name: "body", widget: "markdown"}
```

Create `static/admin/index.html` with the following:
```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Content Manager</title>
    <!-- Include the script that enables Netlify Identity on this page. -->
    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
</head>

<body>
    <!-- Include the script that builds the page and powers Netlify CMS -->
    <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
</body>

</html>
```

Then you can run `hugo serve` and go to `http://localhost:1313/admin` where you will be prompted to login with Github.

Netlify CMS will push changes and new blog posts directly to the Github repository, not to your local repo. So to actually publish these changes to your Github Pages site you have to pull down those changes and rerun the Hugo static site generator on the updated repo. So run:
- `git pull`
- `hugo`
- `git add .`
- `git commit -m 'Changes'`
- `git push`

And shortly your changes will be visible on the Github Pages hosted site.