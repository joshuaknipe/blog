---
title: "Setting Up and Deploying This Blog"
date: 2024-11-25T09:29:49+01:00
topics: ["updates", "hugo", "web development"]
---

{{< lead >}}
**"How can I create my own website like this one (without paying!)"** A detailed guide on how I set up this blog and deployed it for free on Cloudflare Pages.
{{< /lead >}}

In my [previous post](/blog/why-i-built-this-blog-with-hugo/), I shared why I chose Hugo as the framework for my personal blog. Now, let’s dive into the step-by-step process of setting up a blog with Hugo, from installation to publishing your first post. By the end of this guide, you’ll have a fully functional blog live on the internet.

## Installation

These instructions will get you up and running from a blank slate. If you already have a Hugo site, you can skip to the [Deployment to Cloudflare Pages](#deployment-to-cloudflare-pages) section.

### Install Hugo

If you haven’t used Hugo before, you will need to [install it onto your local machine](https://gohugo.io/getting-started/installing/). You can check if it’s installed correctly by running the command `hugo version` in your terminal.

Choose a directory where your blog will live and create a new Hugo site:
```bash
hugo new site my-blog
cd my-blog
```

This creates a basic folder structure for your blog:
- `content/`: Where your blog posts live
- `layouts/`: Custom templates
- `themes/`: For theming your site
- `static/`: Static assets like images and CSS

### Install a Theme

With the skeleton Hugo site created, you can now choose and install a theme. I personally use the [Congo theme](https://jpanther.github.io/congo/), but you can find many other options on the [Hugo Themes website](https://themes.gohugo.io/).
To install any theme, you have 3 options:
1. Use it as a Hugo module
2. Add the theme as a Git submodule
3. Download the theme as a zip file and extract it in the `themes/` directory

While the first option is the quickest and easiest for keeping the theme up-to-date, the second and third options allow for more advanced customisation later on (as you'll see in a follow up post). I went for the third option to make my Git repository easier to manage. The link to download the Congo theme is [here](https://github.com/jpanther/congo/releases/tag/v2.9.0).

In your project folder, delete the `config.toml` file that was generated by Hugo. Copy the `*.toml` config files from the theme into your `config/_default/` folder. This will ensure you have all the correct theme settings and will enable you to easily customise the theme to your needs.

{{< alert "circle-inf" >}}
**Note:** If you used options 2 or 3 to install the theme, you must add the line theme = "congo" to the top of your config.toml file.
{{< /alert >}}

### Basic Configuration

Before creating your first post, you'll want to adjust some settings in the `*.toml` files. This includes things like the base URL, language settings and the default theme colour. See the [Hugo documentation](https://gohugo.io/getting-started/configuration/) and [Congo documentation](https://jpanther.github.io/congo/docs/configuration/) (which is extremely comprehensive) for more information.

{{< showcode toml "linenos=false" "config/_default/languages.en.toml" >}}
title = "Vault of Josh"

[params.author]
  name = "Joshua Knipe"
  image = "img/profile.jpg"
  headline = "Developer"
  bio = "A little bit about me" 
  links = [
    { github = "https://github.com/joshuaknipe" }
]
...
{{< /showcode >}}

## Create Your First Post

With the site configured, you can now create your first post.
```bash
hugo new content posts/my-first-post.md
```

This creates a new Markdown file in the `content/posts` directory. Open it and add content as you see fit.

```markdown
---  
title: "My First Post"  
date: 2024-11-25T12:00:00  
draft: true  
---
Welcome to my blog! This is my first post using Hugo.
```

To see your blog in action, run Hugo's development server (and notice how quickly it rebuilds the site on every change):
```bash
hugo server  
```
Open your browser and visit `http://localhost:1313/` to preview your blog.
Once satisfied, mark the post as ready by setting draft to false in the frontmatter.


## Deployment to Cloudflare Pages

### Why Cloudflare Pages?

Using [**Cloudflare Pages**](https://pages.cloudflare.com/) to host a Hugo blog offers several benefits, especially if you're looking for a reliable, fast, and easy-to-manage platform. Here's why it was my choice:  
- **Free Hosting:** generous free-tier hosting, including unlimited requests and bandwidth.
- **Easy Integration with Git:** Cloudflare Pages automatically integrates with Git repositories (GitHub, GitLab, Bitbucket). Every time you push changes, it triggers a build and deploys your site, enabling a seamless CI/CD workflow.  
- **Global Content Delivery:** Cloudflare Pages automatically serves your site through Cloudflare’s global CDN. This ensures low latency and fast load times for users regardless of their location.
- **Automatic HTTPS:** No need to worry about purchasing or renewing SSL certificates. Cloudflare Pages provides free SSL/TLS for your site, ensuring secure connections with minimal setup.
- **Support for Custom Domains:** You can easily integrate your own domain and manage DNS directly within Cloudflare's dashboard.
- **No Vendor Lock-in:** Since your site is entirely built with Hugo, and the deployment workflow is Git-based, you can move to another host very easily.
- **Zero Build Time for Preview Links:** For every commit or pull request, Cloudflare Pages generates a unique preview link. This allows you to test and share changes before merging them into the main branch. 

### Setting up Cloudflare Pages

1. Commit your changes:  
   ```bash  
   git add .  
   git commit -m "Initial commit"  
   ```  

2. Push your repository to a platform like GitHub:  
   ```bash  
   git remote add origin https://github.com/yourusername/my-hugo-blog.git  
   git push -u origin main  
   ``` 

3. Go to the [Cloudflare dashboard](https://dash.cloudflare.com/) and create a new project:
   - In Account Home, select `Workers & Pages` > `Create application` > `Pages` > `Connect to Git`.
   - Select the Git repository that you pushed your Hugo site to.  
   - Configure the build settings:  
     - Production branch: `main`  
     - Build command: `hugo`
     - Build directory: `public`

4. Deploy the project:
    - Select `Save and Deploy`
    - After deploying your site, you will receive a unique subdomain for your project on `*.pages.dev`. Your blog is now live (albeit at a forgettable address!).
    - (Optional) Add a custom domain by selecting `Workers & Pages` > `blog` > `Custom domains` > `Set up a custom domain`.


## Conclusion

You now have a fully functional Hugo blog deployed on Cloudflare Pages! With Hugo’s speed and flexibility and Cloudflare’s robust hosting, you’re set to deliver a blazing-fast experience to your readers. For updates, simply commit and push your changes to your Git repository — Cloudflare will handle the rest.

The next post will cover how some of the customisations I made to the style and functionality of this website.