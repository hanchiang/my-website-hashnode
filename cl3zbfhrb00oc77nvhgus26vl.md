## Hosting ghost blog as static website

Previously: [Why Blog with ghost platform?](https://www.yaphc.com/blog-ghost-platform/)
**2020 update:** _I have moved from ghost to WordPress, because I value the inclusiveness of the writing experience from WordPress and my chosen theme, instead of having to do some hackarounds for common features such as social links._
I discussed some of the merits and drawbacks of using the ghost platform from my own personal experience. In this post, I will talk about the approach that I use to bring my ghost website from a localhost development environment to a live website hosted on [Netlify](https://www.netlify.com/), a static web host.

# Why static website, and with ghost?

## Static website

First, a static website is a website that consist of just a bunch of `HTML` files, together with `CSS` and `JS` assets. Think of the olden days where websites display the same content to every user. Every time a new piece of content needs to be added to the website, they are manually written into the these files. When a user visits your website, the relevant files are directly served to them without the need for a server to generate them on the fly because there is no database.


**Static vs dynamic website**
![static vs dynamic website](https://cdn.hashnode.com/res/hashnode/image/upload/v1654351089113/PX_hK4QF1.jpeg align="left")


For many of us, we are looking for a website builder or system that allows us to create a blog and write posts, and that is a perfect usecase for a static website.

## Pros and cons of static website

### Pros

**1. Security**
A static website is generally safer than a dynamic website as there is no database to be cracked open. However, vulnerabilities do exist if 3rd party javascripts and dynamic features such as forms, payment gateway are used.

**2. Speed**
Serving a static website is generally faster than a dynamic one, since there is no server to dynamically generate the web page.

**3. Cheap**
There are numerous affordable options for hosting a static website, such as [AWS S3](https://aws.amazon.com/s3/), [Github Pages](https://pages.github.com/), [Firebase Hosting](https://firebase.google.com/docs/hosting/), [Google Cloud Storage](https://cloud.google.com/storage/), [Netlify](https://www.netlify.com/)

**4. Scalability**
No dynamic website can beat any static website in terms of scalability. Once again, there is no database and application server for the case of a static website, all the web host does is to spit out the web page to visitors.

### Cons

**1. Unintuitive workflow**
Adding content to the website requires you to dive into `HTML`, `CSS` and `JS`, which could be overwhelming for the business executives or anyone who hasn't seen or heard of them before.

## Why ghost

For the case of hosting ghost as a static website, it functions more like a [Static Site Generator(SSG)](https://www.staticgen.com/about) with a [headless CMS](https://headlesscms.org/about). Ghost offers a distraction-free writing experience with their simple and elegant admin panel, that is much simpler than wordpress. The flip side of the coin is that there are fewer features out of the box.

## Why netlify

As mentioned above, there are many options for hosting a static website. Here are the features Netlify offer:

-   A free web hosting plan
-   Continuous deployment with git
-   Automatic HTTPS setup
-   Add a custom domain with Netlify's DNS hosting
-   CDN to cache content around the world
-   DDoS protection
-   Load balancing

Wow, that is a lot of freebies right out of the box, for **free**! Don't you feel excited just by reading these? Next, let's run over the process of generating the static website and deploying it to netlify.

## The recipe

1.  Generate static website
    1.  Download an offline copy of the entire _localhost:2368_ website
    2.  Download `sitemap.xml` separately
    3.  Fix internal links to point to the live domain
    4.  Fix `css` and `js` assets links
    5.  Use the https protocol instead of http
    6.  Prettify URL (optional)
2.  Deploy to [netlify](https://www.netlify.com/), an incredible static web host, for free!

## Hosting static ghost website at netlify

First, we need to add a custom domain. Under **Settings** navigation menu, look for **Domain management**, and click on **Add custom domain**. Your domain will be added after a quick verification process.

**Add a custom domain**
![netlify add custom domain](https://cdn.hashnode.com/res/hashnode/image/upload/v1654351124633/domS6I0IK.jpeg align="left")


Now, enter the DNS panel of the primary domain.


**Enter DNS panel**
![netlify go to dns panel](https://cdn.hashnode.com/res/hashnode/image/upload/v1654351178548/3zoJgYxbm.jpeg align="left")


You will see a few Netlify nameservers. Add them to the custom DNS in your domain registrar.

**Netlify nameservers**
![netlify nameservers](https://cdn.hashnode.com/res/hashnode/image/upload/v1654351208456/HCNQ5myJs.jpeg align="left")


For example, I am using namecheap and this is what it should look like.


![namecheap custom dns](https://cdn.hashnode.com/res/hashnode/image/upload/v1654351249587/i79IqR7uo.jpeg align="left")

Namecheap custom DNS

## In closing

Ghost comes with an elegant user interface which allows us to write content peacefully. Hosting ghost as a static website is free and comes with other perks such as fast loading speed(SEO), more secure and able to handle huge traffic volumes if a post becomes viral overnight **Note**: The scripts to generate a static website can be found [here](https://github.com/hanchiang/ghost-convert-static-website). They are generously commented. Feel free to check it out and leave some feedback if you have tried them.