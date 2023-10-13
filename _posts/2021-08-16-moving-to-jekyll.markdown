---
layout: post
title:  "Moving to Jekyll!"
date:   2021-08-16 10:49:47 -0400
categories: jekyll blog
published: false
---
I originally planned to setup a blog module on the main Django app running [muskeg.dev](https://muskeg.dev). It felt like a good idea to build something from scratch, showcasing web dev skills. Including both design and db work.

Recently though, I started reading on headless CMS and Jamstack and thought it would be a good opportunity to play with new toys. I started comparing different systems and ended up choosing [Jekyll](https://github.com/jekyll/jekyll) as it seemed well suited for "blog" oriented projects, which is exactly what I had in mind for this subsection of my online portfolio.

I love markdown and thought it would be great to stop thinking about the infra for once and concentrate on content :)

## _tl;dr_

> I use [Netlify's free tier](https://app.netlify.com/signup) to auto-deploy a Jekyll blog living in this [GitHub repo](https://github.com/muskeg/blog)

## Jekyll

After comparing multiple headless CMS and SSG I decided to go with Jekyll. It seemed lightweight and the latest versions seemed to build sites quickly. I do not expect this site to be that big and complex which made build times a minor factor.

I really liked the blog-oriented setup and its Minima theme seemed like a really good starting point, needing very little theming to integrate to the existing website. And after playing with it for a couple days now I'm happy with my choice.

I started by setting up my repo on GitHub: [https://github.com/muskeg/blog](https://github.com/muskeg/blog). The repo is public, feel free to have a look.

From there it's easy to get started. Install ruby and dependencies:

```bash
sudo apt-get install ruby-full build-essential zlib1g-dev
```

And to install gems for my current user only:

```bash
mkdir ~/gems
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

And then get the Jekyll and Bundler gems

```bash
gem install jekyll bundler
```

With Jekyll installed, I went to my repo folder and just created the blog:

```bash
jekyll new blog
cd blog
bundle exec jekyll serve
```

And voilà. You're watching at the result right now!

## Netlify

At first I wanted to simply use GitHub Pages to deploy the site. This would be the logical option since the code is in a GitHub repository. I won't go in an extended comparison between Netlify and GitHub Pages but after testing both it appears Netlify can do everything GitHub Pages can do and then adds a set of functionalities.

### Faster (better) deployments

The biggest factor for me was the speed at which Netlify deploys your site. I waited between 10 and 20 times longer on GitHub before seeing my website published on a few occasions. I can understand why GitHub works that way and of course I won't be publishing my website that often but I admit it made the first deploys more annoying.

Netlify also gives you full deploy logs straight from the web console:

{% highlight log %}
12:26:48 AM: Build ready to start
12:26:50 AM: build-image version: c6001ed68662a13e5deb24abec2b46058c58248a
12:26:50 AM: build-image tag: v3.9.0
12:26:50 AM: buildbot version: e6b727854930d4a8dd70f1228e63b3eb6425ad1a

...snip...

12:27:07 AM: Site is live ✨
12:27:07 AM: Finished saving pip cache
12:27:07 AM: Started saving emacs cask dependencies
12:27:07 AM: Finished saving emacs cask dependencies
12:27:07 AM: Started saving maven dependencies
12:27:07 AM: Finished saving maven dependencies
12:27:07 AM: Started saving boot dependencies
12:27:07 AM: Finished saving boot dependencies
12:27:07 AM: Started saving rust rustup cache
12:27:07 AM: Finished saving rust rustup cache
12:27:07 AM: Started saving go dependencies
12:27:07 AM: Finished saving go dependencies
12:27:07 AM: Build script success
12:27:23 AM: Finished processing build request in 32.607089782s
{% endhighlight %}

</p>
</details>

33 seconds to build and deploy the site? Far from the "up to 20 minutes" GitHub Pages announces.

### CI/CD, plugins, etc

Netlify's ecosystem also includes many plugins and improved continuous deployments. As I run the majority of my web services behind Cloudflare, one of the first plugins I installed and configured was the [purge-cloudflare-on-deploy](https://github.com/chrism2671/netlify-purge-cloudflare-on-deploy) plugin, which does exactly what it does. The build & deploy environment allows you to quickly set environment variables used in the deployments (like an API key to purge a cache).

In short, I found getting up and running on Netlify to be easier, faster and better than GitHub Pages.

## Custom domain name (and Cloudflare)

People who know me know that I love Cloudflare. Even with their free tier, you gain many benefits from running services on their network: privacy proxy, fast DNS, page rules, firewall and serverless functions. Using their DNS and SSL management made setting up a custom domain name a breeze.

Simply add the domain in Netlify:
![domain management](/assets/img/2021-08-16-movig-to-jekyll/domain-management.png)

Add a CNAME in Cloudflare's DNS:
![Cloudflare DNS](/assets/img/2021-08-16-movig-to-jekyll/cloudflare-dns.png)

And finally, because Netlify already manages SSL encryption on their side, you can use Cloudflare's Full (strict) SSL settings and you're good to go:
![Cloudflare SSL Full (strict)](/assets/img/2021-08-16-movig-to-jekyll/cloudflare-ssl-full-strict.png)

## Wrap up

I'm now using a shiny new toy to showcase my personal dev and artsy (and both!) projects.

Buzzwords alert: A Jekyll-built blog auto-deployed to Netlify using a GitHub Continuous Deployment pipeline served through Cloudflare's global CDN with privacy proxy and full SSL management.

I'm setting a reminder in 6 months to come back and re-review my experience of the platform.

<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
