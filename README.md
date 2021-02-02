# Hugo with Netlify CMS and Github Pages

An example of using Hugo with Netlify CMS and Github Pages. This allows you to use Netlify CMS to modify a local git repository and without relying on Netlify.com services. Hugo then generates a static site to be hosted on Githug Pages.

This repo is setup but if you want to do it to your own Hugo repo you need to:

In `config.toml` set:

`publishDir = "docs"`

Create `static/admin/config.yml` with the following:
```yml
backend:
  name: github

local_backend: true

media_folder: "static/uploads/"
public_folder: "/uploads"

collections:

  - name: "blog"
    label: "Blog"
    folder: "content/"
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
    fields:
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
</head>

<body>
    <!-- Include the script that builds the page and powers Netlify CMS -->
    <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
</body>

</html>
```

Now you need to run a git gateway for the JavaScript admin to talk to:

`npx netlify-cms-proxy-server`

Then in another terminal run:

`hugo serve` 

and go to:

`http://localhost:1313/admin`

Netlify CMS will write to the local git repository. To actually publish these changes to your Github Pages site you have run the Hugo static site generator locally, commit those changes, and push to Github:
- `hugo`
- `git add .`
- `git commit -m 'Changes'`
- `git push`

Your changes will be visible on the Github Pages hosted site.