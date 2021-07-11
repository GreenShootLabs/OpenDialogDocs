---
title: The OpenDialog Application
author: Ronald Ashri
authorURL: http://twitter.com/ronald_istos
---

It's been just a bit over 2 months since we started OpenDialog development and a few days we tagged version 0.4 of [OpenDialog Core](https://github.com/opendialogai/core/releases/). This means that all the major components are in place and we can focus on improvements and fixes on specific aspects instead of across the board architecture changes. 

We've also put together a simpler way to try OpenDialog that saves you configuring and managing all the individual components. This OpendDialog app will be the starting point for OpenDialog application development moving forward. It will configure a couple of conversations for you, setup webchat and webchat testing page, setup Dgraph and populate it. Head to Github and download the [OpenDialog](https://github.com/opendialogai/opendialog) app. This is our starting point for OpenDialog chatbots. It uses [Lando](https://docs.devwithlando.io) to give you a development environment with all the components already setup so you can try out a conversation immediately.
 

There is still a lot to do to get to a 1.0 but we are already starting to see the benefits of some of the core architectural choices like the flexibility of the conversational markup language, the message markup and using Dgraph to store conversational state.


Our goal with OpenDialog is to built a conversation management platform that actively helps you build better conversations and smoothly integrates with your core NLP platform. It does that by providing all the tooling out of the box, providing a solid conceptual framework that both conversation designers and developers can use and by separating conversational flow from conversational content. 

Once that flexible platform is in place the ambition is to take this a step further, and make OpenDialog a pro-active participant in conversation design, giving advice about how to build better conversations and also making it simpler to test different versions of conversations so you can optimize to your need.  

## Ok - so how do I keep up?

We will be regularly sharing pieces of the work as it is ready for wider consumption. Follow us on [Twitter](https://twitter.com/greenshootlabs) or [LinkedIn](https://www.linkedin.com/company/greenshoot-labs/) or drop us an email at hello@greenshootlabs. We are more than happy to jump on a call and show you what we have so far and get comments. 

