---
layout: post
title: How to create your blog with github pages
date: 2023-01-03
---

# Create the repository
You want to create a blog using [Github pages][github-pages] and [Jekyll][jekyll].
These few lines will help you set it up and have it running in a few minutes. 

In [Github][github] create a [new repository][github-new] for your site. Let's call it `<your-blog>`.

Unless you have a GitHub Pro account, you must keep the repository Public. Make sure you choose the right license for you. 

In Settings, under **Build and deployment**, choose:
- Source: `Deploy from a branch`
- Branch: select `main`, leave `/(root)`, and click Save

Now you are ready to clone your repository:

```
git clone https://github.com/apicco/<your-blog>.git
```

Enter your repository folder, which should be empty with the exception of the LICENCE and README.md files, if you chose to have them (you can empty the directory with `git rm -rf` if you need).

# Deploy Jekyll
If you did not install [Jekyll][jekyll] yet, do it: `gem install bundler jekyll` (more infos [here][jekyll-install]).

Enter you repository directory and type
```
jekyll new --skip-bundle
```

This generates a number of files, which include a Gemfile (a list of gems used by yoursite) and your site configuration (\_config.yml).
To use Jekyll with Github pages you need to edit the Gemfile and

- comment `gem "jekyll"`
- uncomment `gem "github-pages", group: :jekyll_plugins`

Then, in your repository folder type
```
bundle install
bundle update github-pages
```

# Edit your site
You can now start to edit your site and add content. You can configure your site by editing you \_config.yml file. Here, you can define important variables, such as your site name, your email address, and the Jekyll theme of your choice (the default is [minima][minima] ). 

You can also personalise the theme by editing its layout. [minima][minima] comes with a few layout files. These layouts are stored in a hidden folder, whose path you can find by typing 
```
bundle info --path minima
``` 

If you wish to modify the layout of a page, create a `_layout` folder in your repository, copy the layout file that you wish to modify into this folder, and edit it. Layouts in the `_layouts` folder overwrite the default layouts.

Finally, you can edit your first post by creating a .markdown file in `_posts`. The file must be named with a date and a title (YYYY-MM-DD-my-title.markdown) and contains an header initiated and terminated by the `---` which specifies a few poperties such as title and layout.
```
---
layout: post
title: my beautiful title
date: 2023-01-01
---

**Hello World**, this is my firts post!
```
Before finishing, do not forget all the files to your git repository, commit them, and push them if you are happy about them.
```
git add .
git commit -m 'you commit message'
git push
```

# Create a DNS

Finally, you want to create a custom domain that points to your GitHub page. First, in the settings of your github repository _your-blog_ select `Pages` in the left menu under **Code and automation**. 
Under **Custom domain** enter your custom domain name and hit save. Below, tick **Enforce HTTPS**. If you want to set up a www subdomain, go to your DNS provider and enter the followings:
- Host name: www
- Type: CNAME
- TTL: 3600
- Data: `<yourusername>`.github.io/`<your-blog>`

It might take up to 24 hours for the configuration to propagate and be effective. Be patient!
You can simply check if the DNS is working by typing:
```
dig www.<your-domain-name> +nostats +nocomments +nocmd
```

If you want to set up a different domain, check the [Supported custom domains here][github-dns].

[github-pages]: https://pages.github.com
[github]: https://github.com
[github-new]: https://github.com/new
[github-dns]: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages#supported-custom-domains
[jekyll]: https://jekyllrb.com
[jekyll-install]: https://jekyllrb.com/docs/
[minima]: https://github.com/jekyll/minima#minima
