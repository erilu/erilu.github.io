Turning the Square Open Source Portal into a personal website
=========================

by Erick Lu

Here, I've forked the [repo](https://github.com/square/square.github.io) for the [Square Open Source Portal](https://square.github.io/) and have modified it to showcase some of my projects.

I like the Square Open Source Portal since it uses Pystache, which makes it really easy to update your website with your latest repos. By changing the repos listed in `repos.json` and running `generate.py`, your website is automatically updated with information pulled straight from GitHub. The layout is also clean and mobile-friendly.

Below are the steps I took to turn it into a personal website:

1. Fork the repo from https://github.com/square/square.github.io, and rename it to <yourGithubUsername>.github.io
2. Edit the `index.mustache` file:
    * replace `<title>Square Open Source</title>` with your name or your website title.
    * replace or remove `<link rel="shortcut icon" href="/assets/public/favicon.ico" />` with your own favicon.
    * replace or remove `<img alt="n Source " src="/images/logo.png" />` with your profile picture.
    * replace `<h1>Square Open Source</h1>` with your own site header.
    * replace `<a href="http://github.com/square">github.com/square</a>` with your own link(s).
    * replace `<p>As a company built on open source...</p>` with your own blurb.
    * replace `<a id="btn-view-full-menu" class="btn-view-full-menu" href="http://github.com/square">View full list of repositories</a>` with your own github link.
    * replace or remove `<section class="profile-modules" id="modules">` containing the twitter module.
    * replace or remove `<i class="icon-square-footer"></i>`
    * replace or remove `<li class="footer-navigation-item">` with your own footer navigation items.
3. Replace or remove images, logos, and sprites from the following directories:
    * assets/public/
    * assets/sprites
    * images/
    * repo_images/
4. Modify `generate.py`:
    * replace line 34 `repo = ghclient.repos.get(user='erilu', repo=name)` with your own username
    * remove code for custom repo generation (lines 48, 63-68)
    * if pygithub3 doesn't work for you, you may need to use pygithub instead. (from github import Github)
    * set up your own netrc if needed
5. Modify `repos.json`:
    * replace the Square repos with your own, with the format: "repo-name":["Category Name"]
    * the "Category Name" will be what shows up as a sub-header on the website.
6. Run `generate.py` by typing `python generate.py`.
    * This will automatically pull information from each of your repos listed in `repos.json`, and create the index.html for your website.
7. You're done! Your website should show up at your github.io link. If you want to further customize your website, you can edit index.mustache. If you want to change the repos/categories that are displayed, change `repos.json`.

The content below is from the original README.md file from the Square Open Source Portal repo.

-----------
Development
-----------

### Run the site locally
```bash
gem install bundler # first time only
bundle install # first time only
bundle exec jekyll serve
```


### Update list of repos:
```bash
pip install pystache requests pygithub3 # first time only
./generate.py
```

About the code
-----------
Due to the use of absolute URLs in CSS files that are (essentially) out of our
control, the easiest way to develop is by running with Jekyll.

Repositories are listed in the `repos.json` file as a map of repository names
to a list of their categories. Invoking the `generate.py` script will update
the `index.html` page with the latest repos by using the `index.mustache` file
as a template.

Repository data is pulled via the GitHub API (e.g., website). By default the
script performs unauthenticated requests, so it's easy to run up against
GitHub's limit of [60 unauthenticated requests per
hour](http://developer.github.com/v3/#rate-limiting). To make authenticated
requests and work around the rate-limiting, add an entry for api.github.com to
your ~/.netrc file, preferably with a Personal Access Token from
https://github.com/settings/tokens

    machine api.github.com
      login YourUsername
      password PersonalAccessToken

Images are loaded by convention from the `repo_images/` directory. Ensure the
name is the same as the repo name in the `repos.json` file and has a `.jpg`
extension. Currently all images are rotated 10 degrees counter-clockwise to
break up the overwhelming horizontal and vertical visual lines on the page.
