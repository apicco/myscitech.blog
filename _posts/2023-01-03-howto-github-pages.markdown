---
layout: post
title: How to create your blog with GitHub Pages
date: 2023-01-03
published: true
excerpt: "Create your own blog with GitHub Pages and register a domain to point to it, without interfering with your github.io page and other repository pages."
---

# Create the repository
You want to create a blog using [GitHub Pages][github-pages] and [Jekyll][jekyll].
These lines will help you set it up and have it running in a few minutes. 

In [GitHub][github], create a [new repository][github-new] for your site. Let's call it `<your-repository-name>`.

Make sure you choose the proper license for you. 

Now you are ready to clone your repository:

```
git clone https://github.com/<your-user-name>/<your-repository-name>.git
```

Enter your repository folder, which should be empty except for the LICENCE and README.md files if you choose to have them (you can empty the directory with `git rm -rf` if you need).

# Deploy Jekyll

If you still need to install [Jekyll][jekyll], do it: `gem install bundler jekyll` (more info [here][jekyll-install]).

Enter your repository directory and type
```
jekyll new --skip-bundle .
```

This command generates a number of files, including a Gemfile, which is a list of gems used by your site.
To use Jekyll with GitHub Pages, you need to edit the Gemfile and

- comment the line starting with `gem "jekyll"`
- uncomment `gem "github-pages", group: :jekyll_plugins`

Then, in your repository folder type
```
bundle install
bundle update github-pages
```

You can test the look of your site by running a local server
```
bundle exec jekyll server
```
You can then access the local server with a web browser at `localhost:4000`

```
open -a safari http://localhost:4000'
```

# Edit your site
You can now start to edit your site and add content. You can configure your site by editing your `_config.yml` file. Here, you can define important variables, such as your site name, email address, and the Jekyll theme (the default is [minima][minima] ). In your `_config.yml` file, make sure to have the followings:
```
baseurl: "/" # the subpath of your site, put /<your-repository-name> if you do not have a custom domain 
url: "<your-domain-name>" # the base hostname & protocol for your site, e.g. https://myscitech.blog or <your-user-name>`.github.io/`<your-repository-name>, if you do not have a custom domain
```

You can also personalise the theme by editing its layout. [minima][minima] comes with a few layout files. These layouts are stored in a hidden folder, whose path you can find by typing 
```
bundle info --path minima
``` 

If you want to modify the layout of a page, create a `_layouts` folder in your repository, copy the layout file that you wish to change into this folder, and edit it. Layouts in the `_layouts` folder overwrite the default layouts.

Finally, you can edit your first post by creating a .markdown file in `_posts`. The file must be named with a date and a title (YYYY-MM-DD-my-title.markdown) and contains a header initiated and terminated by the `---`, which specifies a few properties such as title and layout. This hearder is a [YAML front matter][yaml-front-matter].
```
---
layout: post
title: my beautiful title
date: 2023-01-01
---

**Hello World**, this is my first post!
```
Before finishing, do not forget all the files to your git repository, commit them, and push them if you are happy about them.
```
git add .
git commit -m 'you commit message'
git push
```

# Publish your github page 

In Settings, under **Code and automation** in the right hand menu, chose `Pages`, and then **Build and deployment**, choose:
- Source: `Deploy from a branch`
- Branch: select `main`, leave `/(root)`, and click Save

The page will be accessible under 
```
<your-user-name>.github.io/<your-repository-name>
```

# Create a DNS

Finally, you want to create a custom domain that points to your GitHub page. First, in the settings of your github repository `<your-repository-name>`, select `Pages` in the left menu under **Code and automation**. 
Under **Custom domain**, enter your domain name `<your-domain-name>`, for example [myscitech.blog][this-blog], and hit save. Below, tick **Enforce HTTPS**. 

To set up an [Apex domain][github-pages-apex], go to your DNS provider and enter the followings:
- Host name: `leave it blank`
- Type: A
- TTL: 3600
- Data: 
```
        185.199.108.153
        185.199.109.153
        185.199.110.153
        185.199.111.153
```
(you can add multiple IP with the `+` button).

Add also a `www` subdomain:
- Host name: www
- Type: CNAME
- TTL: 3600
- Data: `<your-domain-name>` (for example, [myscitech.blog][this-blog])

Note that if you set your `www` subdomain to point to `<your-user-name>`.github.io/`<your-repository-name>` it will not work because you are pointing to a repository name and not to `<your-user-name>`.github.io.

It might take up to 24 hours for the configuration to propagate and be effective. Be patient!
You can check if the DNS is working by typing:
```
dig <your-domain-name> +nostats +nocomments +nocmd
dig www.<your-domain-name> +nostats +nocomments +nocmd
```

Check the supported custom domains [here][github-dns] if you want to set up a different domain.

# Link to Google Analytics

To track the visits to your site, register your site URL with [Google Analytics][google-analytics]. You will obtain a `measurement ID`.

Add your measurement ID to your `_config.yml` file:

```
# Google Analytics:
google_analytics: <your-measurement-ID>
```
[Minima][minima] comes with a `google-analytics.html` in its default `_includes` (whose path, remember, you can access with `bundle info --path minima`). That did not work for me. 
Instead, search and copy the Google Analytics `gtag.js` code from [Google Analytics][google-analytics]:
```
admin (in the lower left corner of the Google Analytics page) > Data Streams > (your stream) > View tagging instructions (it is at the very bottom of the page) > Install manually
```
The code should look like this: 
{%raw%}
```
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id={{ site.google_analytics }}"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', '{{ site.google_analytics }}');
</script>
```
{%endraw%}
Note that here I replaced `<your-measurement-ID>` with {%raw%}`{{ site.google_analytics }}`{%endraw%} in two places. 
This code will thus load `<your-measurement-ID>` from your `_config.yml` file.

Paste the code in a new `google-analytics.html` file in your `\_includes` (which thus overwrites the default from [minima][minima]). 
You must name your file as `google-analytics.html` because the default `head.html` in the [minima][minima] searches for such a file:
{%raw%}
```
<head>
  ...
  ...
  {%- if jekyll.environment == 'production' and site.google_analytics -%}
    {%- include google-analytics.html -%}
  {%- endif -%}
</head>
```
{%endraw%}

Note also that the `head.html` searches for your `site.google_analytics` variable, which you stored in `_config.yml`. 
The 'production' option is to avoid Google Analytics counting the site visits on localhost. 

Once done, wait for one day or two for Google Analytics to link to your site.

[github-pages]: https://pages.github.com
[github-pages-apex]: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages#using-an-apex-domain-for-your-github-pages-site
[github]: https://github.com
[github-new]: https://github.com/new
[github-dns]: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages#supported-custom-domains
[jekyll]: https://jekyllrb.com
[jekyll-install]: https://jekyllrb.com/docs/
[minima]: https://github.com/jekyll/minima#minima
[this-blog]: https://myscitech.blog
[yaml-front-matter]: https://jekyllrb.com/docs/front-matter/
[google-analytics]: https://analytics.google.com
