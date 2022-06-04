## Building redis with CodeCrafters

[CodeCrafters](https://codecrafters.io/) is an amazing community built by [Paul Kuruvilla](https://twitter.com/rohitpaulk) that contains project challenges for real-world software systems that are used widely(redis, git, docker, SQLite).

Each project comes in the form of guided steps on what you will build and leaves enough room for you to acquire new knowledge and apply them.

As a follow-up to my previous post on [learning Rust](https://www.yaphc.com/learning-a-system-programming-language-rust), as well as my personal goals of working with technology at lower levels, I tried the [Redis challenge](https://app.codecrafters.io/courses/redis/overview) with Rust, which is at the beginner level, and had a blast with it.

## The learning experience

A starter project is provided to help you get started. Each time code is pushed to the repository, the CodeCrafters server will run your code against its own test cases and the output is streamed to your console.


![git-push-streaming-logs.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654320876525/pqTxSX9Tk.png align="left")

Here are the steps in the redis challenge.

**1. Set up**


![1.-setup-redis.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654320903519/npFedN4l3.png align="left")

**2. Bind to a port**


![2.-bind-to-a-port.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654320945947/VHI9VJmRs.png align="left")

**3. Respond to PING**

![3.-respond-to-ping.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654320984345/x9eBXzy3D.png align="left")

**4. Respond to multiple PINGs**

![4.-respond-to-multiple-pings.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654321011060/CVnvNO3P6.png align="left")

**5. Handle concurrent clients**

![5.-handle-concurrent-clients.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654321037625/HDpKd-vKD.png align="left")

**6. Implement ECHO**

![6.-implement-echo-command.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654321066479/goCl6gpS-.png align="left")

**7. Implement SET and GET**

![7.-implement-get-and-set-command.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654321086078/ZRtTopv_V.png align="left")

**8. SET with expiry**


![8.-set-with-expiry.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654321174270/Q7nvP_ApU.png align="left")

**Challenge complete**

After completing the first challenge in CodeCrafters, you will get a cool profile page that shows your progress.

![redis-challenge-complete.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654321194743/Ckbcok3Xp.png align="left")

As you can see, the challenge comes in a series of tasks that are built on top of the previous ones. Links to learning resources are provided, nothing more than that.

However, if you are stuck on a certain task, there is a discord community in which you can ask for help. The creator himself is very active.

The website also has a useful FAQ bot for general debugging.


![FAQ-chat-bot.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654321218390/6ckoZjtfW.png align="left")

FAQ bot

## Thoughts

I really enjoyed the learning experience that CodeCrafters provides as there is just enough material for me to start learning and building on my own. It is fun, and is the best way to learn a programming language - learn by doing.

The website is super friendly, easy to navigate, easy to get help via FAQ bot and discord.

Going through multiple courses is useful only up to a certain point because many of them hold your hands throughout every step, leaving little room for independent learning.

For people who "don't know what to build", including me, this is a good place to start, and I cannot think of something better.

Overall, I highly recommend anyone who wants to learn a programming language and build a project at the same time to use CodeCrafters.

Here is my completed project on [github](https://github.com/hanchiang/codecrafters-redis-rust). Feel free to check it out and leave suggestions for improvements.