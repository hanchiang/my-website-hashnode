# Hashnode: A home for my blog

A few days ago, I discovered [Hashnode](https://hashnode.com), a blogging platform and community for tech folks to share knowledge and ideas.

From a quick glance at the homepage, it seems like the dream platform for tech writers. All you have to do is to focus on writing content, while Hashnode takes care of everything else:

* Own your content
    
* No ads or paywall
    
* Reach a wide audience
    
* Built-in analytics and various integrations
    
* Easy authoring with markdown
    
* SEO
    
* Custom CSS
    
* Backup with GitHub 😎
    
* Article series 🤩
    
* Built-in newsletter 🤩
    
* Bonus: Built on Next.js, powered by Vercel 😎
    

These alone piqued my interest, and I signed up for an account to give it a try.

## History of my website

**Wordpress** I created my website sometime in 2016. I chose WordPress, self-hosted on Digital Ocean because I wanted to focus on writing only while retaining full ownership of my content.

**JAMstack** Shortly after, [JAMstack](https://jamstack.org/) became a popular way of managing blogs because there are no more servers involved in the stack, eliminating **costs**, and improving the **performance** and **security** of websites.

![google trends jamstack.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654415043425/lCmA7dSFs.png align="left")

So, I experimented with Static Site Generators(SSG) like [Hugo](https://gohugo.io/) which is based on Go, [Gatsby](https://www.gatsbyjs.com/) which is based on React. The developer in me found it fun to have full control in deciding how the content is stored and shaping the user experience.

However, the excitement died off quickly as I drifted away from my initial goal and found myself spending more and more time customizing layouts, adding social sharing integrations, SEO, analytics, image compression, etc

**Ghost** So, I switched to self-hosting [Ghost](https://yaphc.com/blog-ghost-platform). It offers a clean writing experience, with cards for rich media and great out-of-the-box SEO.

Later on, I experimented with downloading a local copy of the website and [hosting it as a static site](https://yaphc.com/ghost-blog-static-website) on Netlify, in the spirit of JAMstack.

It was almost perfect, except I found myself having to dive into customising the theme to add basic features like social links, breaking my rule of wanting to focus on writing again.

**Back to wordpress**

So, I moved my website back to wordpress and lived with the plugin life.

**To Hashnode**

Finally, my website ended up on Hashnode. Hashnode solves all the problems that I have faced previously and really allows me to focus just on writing while allowing me to own my content.

I used this [guide](https://kumarashwinhubert.com/how-to-migrate-your-blog-from-wordpress-to-hashnode) to migrate my website from Wordpress to Hashnode. It is simple, except for image handling - I have to manually upload them one by one. 😢

## A tour of Hashnode

### Home feed

The home feed comes in a 3 columns format that is commonly used in writing platforms, similar to [DEV](https://dev.to/). A navigation bar on the left, a main content block in the middle, and a utility column on the right.

![hashnode home feed.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654418656381/G74KQzGIn.png align="left")

### Editor

![hashnode editor.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654419583773/XPYpYZJ26.png align="left")

The editor offers a clean, distraction-free space to write your content, similar to what Ghost offered. Standard content tools such as links, code snippets, and images are supported.

**Settings flyout**

![hashnode editor flyout.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654419822450/kMT0xFJwcF.png align="left")

Clicking the "**...**" and "**Settings**" on the top right will bring out the settings flyout on the right. This is where you can adjust various settings to prepare for publishing. In particular, I really like the ability to auto-generated table of contents.

![hashnode editor flyout 2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654419865418/5s88_923I.png align="left")

Skipping ahead, you can also schedule your article, backdate an article, hide comments, hide article from the community.

Hashnode does a great job of leveraging the built-in analytics for you to choose the best day to publish your post. 👍

### Dashboard

![hashnode dashboard top view.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654424850009/GNTS7Biya.png align="left")

![hashnode dashboard bottom view.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654424883367/bMPCgNmmg.png align="left")

On the dashboard page, there is a general navigation bar on top, a call-to-action hero section, and the main navigation bar on the left which is grouped according to their functions.

This page is one of my favourite parts of Hashnode. It has all the essential batteries included in a content-sharing platform.

For authoring, I can write drafts and articles, create a static page, create a **series** and add articles under it.

It has great built-in **analytics**, with an even greater advanced analytics page which is currently in beta.

I can embed elements from various platforms by defining them as **widgets**, and integrate them with services like google analytics, hotjar, [web monetisation](https://webmonetization.org/). There is even a **newsletter** service out of the box! 🤩

I can also set a **custom domain**, **backup** content to Github, export data, and more.

### Analytics

Now, let's look at the amazing analytics page.

![hashnode dashboard analytics 1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654426014148/lXBsLU_NC.png align="left")

![hashnode dashboard analytics 2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654430472852/wrb3oXqyC.png align="left")

At the top is a summary of the number of views over a period of time. As I have just started on Hashnode yesterday, there is data only for 4 June 2022. Scrolling down, you can see the number of views, comments, and reactions to your articles in reverse chronological order.

#### Advanced analytics

This is where analytics gets even more awesome.

![hashnode dashboard analytics beta 1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654430847152/mEsm3S6xw.png align="left")

![hashnode dashboard analytics beta 2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654430905376/cCy3FYE4Y.png align="left")

![hashnode dashboard analytics beta 3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654430918545/uKcXUc7kW.png align="left")

![hashnode dashboard analytics beta 4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654430931764/1O7p7sjPN.png align="left")

You can see the number of visitors within a date range, by page, traffic source, browser, device, and operating system. Although it is not a full-fledged analytics service like Google Analytics, the important data points are presented in a simple UI and are easy to understand.

### Article view

![hashnode post view text selection sharer.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654435280623/3dOo7zGpX.png align="left")

![hashnode post view sticky table of content.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654435577511/AnLUva5RT.png align="left")

The reading experience is great too. The article page has 2 columns: A main content block that is centered, and a right sticky bar for reactions, and social sharing.

The table of content is displayed neatly at the top, indent according to the headings.

As you scroll down, the table of content sticks to the top of the page, which allows you to easily navigate to a particular section. No more scrolling up and down. A win for content discovery. 👍 This can be a huge time saver if the article is long.

There is also a cool utility that allows you to select some text and share it on social media.

### Run out of ideas? Request for articles comes to the rescue!

![hashnode request for articles.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654432103212/TgV1I_DOy.png align="left")

One of the biggest problems writers face is brainstorming for ideas.

Hashnode has this useful feature called [Request For Articles(RFA)](https://hashnode.com/rfa). It is a page where people request articles for various topics and writers can use this page for content ideas. What is better is that after you have written an article for a particular RFA, you can add the link to your article and it will be attached to the RFA.

This crowdsourcing of content is a great way to generate interesting ideas and keep the engagement level within the community high.

### Documentation

Every developer appreciates a well-written [documentation](https://support.hashnode.com/docs). The documentation is well organised, making it easy to search for anything you are looking for. Again, a plus point for content discovery. 👍

## What could be better

### Editor preview page

If you scroll back up to the [Editor](#heading-editor) section, you can see that there is a preview tab that allows you to see what your article looks like.

The preview is rendered in the same content block as the article, resulting in excessive scrolling up and down to switch between writing mode and preview mode.

It would be great to have the preview mode as a **flyout** instead of being rendered in the same content block as the article so that writers won't have to scroll all the way up to switch mode, scroll down to look at the rendered content, and repeat them over and over again.

### Published/updated content not displayed when the back button is clicked

I noticed that after updating an article, which brings me to the article itself, I hit the back button to return to the editor page, and the changes that were made did not show up. Instead, I see the old content as before.

After refreshing the page, the changes showed up.

I found out about this when I updated an article, such as checking the auto-generate table of content, returning to the editor page, making some other changes, and realised that the table of content disappeared.

This may not a big issue but could be frustrating if a huge update was made, only to be overwritten by another small update.

### Newsletter ended up in junk mail

I tried out the newsletter service, which is really simple to use. All I have to do is to flip a toggle, and the newsletter subscription box shows up on the website.

However, the newsletter ended up in the junk mail for hotmail. 😢 For Gmail, it shows up in the inbox as expected.

## Summary

After just 1 day of using Hashnode, I can say that I really enjoy using it. It takes care of everything, allowing me to focus on just the writing part.

Note that at this point of writing, Hashnode has been running for around 2 years, and has raised $6.7M in the [series A round on 18 August 2021](https://www.crunchbase.com/organization/hashnode/company_financials).

As of now, they are still in the growth phase in which the focus is on **building and expanding** the core product, not on monetisation.

After the platform has reached a certain level of maturity, the focus will shift toward **profitability**.

Let's see whether they will continue to support all the features that were already built or gate some of them behind a paywall.

This is important with the slowdown in the global economy, as interest rates are rising to combat inflation, leading to a general reduction in spending, including startup fundraising.

I hope that the team will continue to do well like they have been doing so far, and use this platform for as long as possible.

## Join me

If you like what you see about Hashnode so far, you can sign up [referral link](https://hashnode.com/@hanchiang/joinme).

I do not earn a commission for referrals, but I will be able to qualify for the [ambassador](https://hashnode.com/ambassador) role that provides perks like access to beta features, converting articles into audio, etc.