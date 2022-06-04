## Tracking user behaviour with Google Analytics and Google Tag Manager

Building a great product is important, but tracking its impact on end-users is even more important in order to determine its effectiveness.

Being a software developer means that I spend most of my time writing code to build and maintain features, and fix bugs that are uncovered along the way.

As much as I position myself to be a product engineer that delivers product features to solve user needs, there is little visibility of the impact on end-users, which is reasonable because the analytics part of the work will be typically handled by a product manager or analytics manager.

I wanted to gain a first-hand experience in setting up simple analytics to track custom events in order to understand user behaviour. What better tool to start than the ubiquitous [Google Analytics](https://analytics.google.com/analytics/web/)?

## Set up Google Analytics

First, create a Google Analytics(GA) account, property, view.

Then, create a GA **tracking code** under **property.** It starts with '**UA**'.

Finally, we just need to paste the provided code snippet in every web page that we want to track.

[![Google analytics tracking code](https://www.yaphc.com/wp-content/uploads/2021/03/GA-tracking-code-1024x399.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GA-tracking-code.png)

Where to find tracking code: Admin -> property -> tracking code

That's all!

By default, that piece of code only tracks page view, which is fine. But, we also want to track custom events too.

## Google Analytics with Single Page Application

Furthermore, if our website/webapp is a [Single Page Application](https://en.wikipedia.org/wiki/Single-page_application), only the first page load will trigger the page view event because GA event is only triggered when there is a full page load which sends a request to the GA server.

But for a SPA, subsequent page load after the first will not trigger a full page load.

So, we have to set up custom events to tell GA to track them.

Also, I have chosen to use **[Google Tag Manager(GTM)](https://marketingplatform.google.com/about/tag-manager/)** to manage my GA tags. GTM simplify the management of various Google and third party tags.

## Set up Google Tag Manager

**1. Create a GA settings variable**

This is used to configure GA settings across multiple GA tags.

Paste the GA tracking code in "**Tracking ID**"

[![Google Tag Manager variable settings](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-GA-settings-variable-1024x570.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-GA-settings-variable.png)

**2. Create a tag manager account in [GTM](https://tagmanager.google.com/#/home).**

Note down the tag ID which starts with '**GTM**'.

[![Google Tag Manager tags](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-tags-1024x362.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-tags.png)

I have 5 tags that tracks different events

**3. Create a tag configuration**

Create a tag configuration for **Google Marketing Platform**, and select the type of event to track(page view, custom event, etc).

[![Google Tag Manager tag configuration](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-search-tag-1024x526.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-search-tag.png)

**4. Create a tag trigger**

Create a trigger to let GTM know when to fire a particular event. Afterwards, we can use the **event name** to fire this event.

[![Google Tag Manager tag trigger](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-search-trigger-configuration-1024x437.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-search-trigger-configuration.png)

Trigger for event names starting with 'event-search'

**5. Set up event variable**

Create a **dataLayer** variable, which we will send from our SPA.

[![Google Tag Manager variable](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-search-variable-1024x503.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-search-variable.png)

The variable name is "eventSearch"

**6. Send custom event in SPA**

I use [Next.js](https://nextjs.org/) for my SPA, which is based on React.

There are different GTM libraries for React. I chose [react-gtm](https://github.com/alinemorelli/react-gtm).

After setting it up, I discovered another GTM library called [react-gtm-hook](https://github.com/elgorditosalsero/react-gtm-hook), which is based on React hooks and offers a slightly better developer experience. I may try it out in the future to see if it is better.

A side note: I noticed that the "**dataLayer**" data that gets sent to GA contains data from all preceding events, which is a little annoying. I am not sure if this is supposed to happen or I used it wrongly.

[![Google Tag Manager in React](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-helper.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-event-helper.png)

**7. Test it in preview**

Clicking on the "**preview**" button will bring up a new page that opens up the GA debugger that connects to your website.

[![Google tag manager overview](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-overview-1024x455.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-overview.png)

Click on preview at the top right corner

[![Google Tag Manager preview](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-preview-1024x408.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-preview.png)

Trigger the event in your website, make sure it appears in the preview window

**8. Publish the changes**

Remember to hit the **Submit** button. This can be a frustrating fix to the problem of "it doesn't work" because we forgot to publish the changes.

[![Google Tag Manager publish](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-overview-top-bar-1024x48.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GTM-overview-top-bar.png)

Workspace changes shows the number of unpublished changes

**9. Check GA real-time reports**

Finally, to make sure the custom event is set up properly, go to GA **real-time** reports -> **events**. Trigger some events on the website, which should appear in the real-time dashboard after a second or two.

[![Google analytics real-time events](https://www.yaphc.com/wp-content/uploads/2021/03/GA-realtime-events-1024x300.png)](https://www.yaphc.com/wp-content/uploads/2021/03/GA-realtime-events.png)

Events should show up in the real-time dashboard almost immediately

## Conclusion

This is all for the basic setup of tracking a SPA with Google Analytics via Google Tag Manager.

## Useful resources

Learn about Google analytics and Google Tag Manager: [Google analytics academy](https://analytics.google.com/analytics/academy/)