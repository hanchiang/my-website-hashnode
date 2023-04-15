---
title: "Challenges in designing a great user onboarding experience"
datePublished: Sun May 31 2020 12:10:43 GMT+0000 (Coordinated Universal Time)
cuid: cl3zbfhtx00s15rnv32uc55of
slug: challenges-in-designing-user-onboarding-experience
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654335168671/5Lg-emV9z.jpg
tags: user-experience, work, user-onboarding

---

Designing a great onboarding, sign in experience is not easy because everyone is different and behaves differently. However, doing it well can put a smile on users' faces.
For most B2C applications, onboarding should be quick, intuitive, and easy to navigate. The key is to offer a high-value, frictionless experience with minimal effort.

I came across an article that describes [common problems with login and its solutions](https://uxplanet.org/most-common-log-in-problems-and-solutions-163a6209c0eb), and another that talks about the [rules for sign in experience](https://uxdesign.cc/15-rules-of-user-sign-in-experience-ae9011d04ee3). Both contain useful information to consider when designing the entire onboarding and sign-in flow.

Authentication and identity is one of the first few features I worked on. At its basic level, it allows users to register an account, sign in, link accounts, and manage their profile information.


![sign-in-schema.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654341865128/UbLxMBd7b.png align="left")

There are a few areas that I found to be more challenging, which requires a good understanding of the expected user behaviours and experience before designing the solution.

I may have skipped a few steps, which ultimately resulted in these discoveries. 😯

First, let's list down the user requirements:

1.  As a user, I want to be able to sign in with different methods according to my preferences and specific needs
2.  As a user, I want to be able to set a password on my account for security purposes
3.  As a user, I want the system to provide helpful advice to rectify errors when I am unable to sign in

## Multiple sign in methods

Allowing users to sign in with different ways is a very common pattern in consumer applications. This is useful because it provides **flexibility** to how they want to sign in to the service.

Some people do not like to type in email and password just to register an account, while other more **privacy-conscious** users do not want their social account's information to be accessed.

However, with flexibility comes complexity as there are more paths a user can take to achieve a goal, sometimes causing conflicts.

## Email is taken: Same or different user?

Suppose a user signs up with [e@e.com](mailto:e@e.com), then later another user(could be the same person as before) signs up with Google using the same email [e@e.com](mailto:e@e.com).

A question comes to mind:

> Should the system recognise this as 2 different users and therefore create a new user account or 1 user with two sign-in methods?

**Useful error message**

My team decided to reject the Google sign-up using an email that exists in the system, and show a **helpful message** that tells the user to sign in with email. The reason is to prevent the same person from accidentally creating multiple accounts when their actual intention might be to sign in with a method that they thought they have previously linked.

**What about email impersonation?**

If a random person were to register an account with my email, I will be unable to register an account using my own email, and that is not cool.

Also, different social account providers have their own rules regarding email verification. Facebook requires email verification, while google does not. So, it is safe to assume that a user signing up with Facebook is the rightful owner of that email, but the same can't be said for Google.

This is something that an **email verification** flow takes care of easily. A new user account requires verification in order to perform the main business functions, and every business serious about their work will have a form of verification.

On top of that, we have an email uniqueness constraint that requires the email of new signups to be **unique across all sign-in methods**. This adds another layer of safeguard to deter rogue users.

## Account linking

Referring back to the issue of differentiating whether 2 sign-ups using the same email should be considered as a separate account or the same account with 2 sign-in methods, we decided **not to do automatic account linking** because the users' expectation is to link the account in their profile themselves.

> We believe that an explicit over implicit approach presents a more intuitive experience.

## Email usage and modification

Using the same email for authenticating and marketing communications purposes **simplifies** usage from a user's perspective and maintenance from a development perspective.

If a user changes their sign-in email, they should subsequently receive all communications through that same email.

Another option is to use a **secondary** email as a communication channel. The benefit is the **separation of authentication and communication**. However, I am not convinced that the benefits outweigh the complexity of managing two emails.

## User signed out and forgot the sign-in method(s)

It is possible that some time down the road a user may be signed out of the service for some reasons:

-   Computer broke down and needed a complete re-installation of the operating system
-   Signed out of the service, intentionally or unintentionally.
-   The user is a power user and decides to clear browser storage

For services that only allow a single sign-in method, this is not an issue at all. For services that offer multiple sign-in methods, a user may need to go through multiple unsuccessful sign-in attempts before succeeding or giving up.

Although such situations may be rare, it still brings up an important and relevant user pain point

Everyone uses multiple services. Personally, I sign in to various applications with different methods. I cannot and don't want to remember the exact combinations for each one.

From my point of view, for services dealing with non-confidential information, I should stay signed in as long as possible. In the event that I am signed out for any reason, it should be easy for me to get back in again.

> _Services should be designed to_ [forgive user mistakes](https://uxdesign.cc/human-error-an-important-ingredient-in-great-designs-5cd1c278ba7f) _and offer a way to recover_

## Be more user-centric

**(Development) hats off**

Developers have the tendency to jump right into the action and develop cool features that no one really wants to use. I am guilty of that. 😅

Take off the developer hat for a moment and put yourself in a user's shoe, better yet see things from multiple user personas' perspectives to understand the pain points of different user segments and profiles.

**Read, discuss, iterate**

Great products emerge after multiple rounds of debate and improvements.

Having knowledge in topics such as human-computer interaction, visual design, behavioural sciences like psychology, anthropology, and cognitive science is invaluable in designing great products.

Some of my favourite sources of User Experience readings are [UX planet](https://uxplanet.org/), [UX collective](https://uxdesign.cc/), [Interaction Design Foundation](https://www.interaction-design.org/)

**Look at what others do**

Chances are, the problems we are trying to solve have been solved by great organisations with talented product and engineering teams.

Here are some examples:

**Klook: Signed up with social account, prompt to link email**
![klook add email address to link to account](https://cdn.hashnode.com/res/hashnode/image/upload/v1654344086107/rf6YoJQ95.png align="left")


**Pinterest: Link an account**
![pinterest add another account](https://cdn.hashnode.com/res/hashnode/image/upload/v1654344134948/GD8BY9GdH.png align="left")


**Airbnb: Sign up with social account, prompt to set a password**
![airbnb social sign up set password](https://cdn.hashnode.com/res/hashnode/image/upload/v1654344175129/POx7SnQ6Z.png align="left")    


Quora: Sign up with social account, set password
![quora social sign up set password](https://cdn.hashnode.com/res/hashnode/image/upload/v1654344219000/HAQKxDO5R.png align="left")

**Quora: Option to add another email**
![quora profile add another email](https://cdn.hashnode.com/res/hashnode/image/upload/v1654344300696/wYhBE4e9v.png align="left")

**Topbuzz: Link email account**
![top buzz connect email](https://cdn.hashnode.com/res/hashnode/image/upload/v1654344388026/AF0r2MxbR.png align="left")    

From sampling the onboarding experience of Airbnb, Quora, Klook, Pinterest, Topbuzz, it seems like allowing users to set a password, and optionally adding a new sign-in email, is a common approach.


## Summary

Creating great products that people enjoy using is difficult.

I vaguely remembered a phrase that the professor from my human-computer interaction course said:

> Remember, [users are dumb](https://uxplanet.org/are-users-really-stupid-92de6901e2be), so are designers

I could almost hear the gasp from everyone. But shortly after, we understood what the professor meant.

Users are "dumb" because everyone is different, and behaves differently. They know the problems that they are facing but not the solutions.

Designers are "dumb" because they **assume** they understand their users and know how to solve their problems. Designers who take a narrow perspective in their work and refuse to take feedback, tend to design products that do not solve users' problems, and may even worsen them.

By the same token, developers are "dumber" because in a typical team structure developers are the furthest away from the end-users, and have the least understanding of users' problems.

> Ultimately, creating a great product is not easy. It requires alignment of **intentions** and **assumptions**. No one gets it right the first time.

Finally saving the best for last, I recently discovered [Growth.Design](https://growth.design/), a website that teaches people how to build products that your users will love through design principles and psychology, as well as in-depth case studies of popular companies.

I believe anyone who is interested in building great products will benefit from using the website.