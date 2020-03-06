---
layout: single
title:
excerpt: "Reliable E2E testing"
header:
  show_overlay_excerpt: true
  overlay_image: /assets/images/blog_media/cypress/cypress_splash.png
  overlay_filter: 0.2
related: true
read_time: true
---

I don't usually blog about work. For the most part, there's no need for that, really.

All of my full time jobs and consultancy gigs have ben under some sort of NDA, so by the time the restriction is lifted, sharing meaningful stories or experiences would make little sense.

Besides, there's no point in trying to reinvent the wheel - there's an abundance of well established QA resources out there: Ministry of Testing, 5blogs, all sorts of Medium channels to name a few.

So why reiterate theory, best practices, and random code snippets to just keep up with an idea.

However, I recently finished a test suite for a client using a relatively new testing framework, and wanted to share some general impressions after spending time with Cypress.

# Cypress in a nutshell

Cypress is a modern JS based testing framework that has received quite the popularity over the last year. Weekly www.npmjs.com stats show consistent download activity, and the project maintainers push regular updates along with fixes. In fact, they released version 4.1.0 a few days ago!

The framework comes bundled with almost everything you'd need, and initial configuration is already taken care of. Having everything wired up means you can have your first test up and running in just a few minutes.

# Installation

Assuming you have NPM, just `npm install cypress --save-dev` in your project folder to get the latest stable version. There's also a standalone desktop app available on https://download.cypress.io/desktop if you just want to give the framework a try.

Installing Cypress as a dependency would ensure you won't have issues opening it from within your project folder. I first tried the downloadable version on a different project, and it would not play nicely as a shared resource.

# What's in the box

The biggest selling point of Cypress is that it's an all in one packaged deal, and it already comes with the main testing components preconfigured and ready to use:

- test runner GUI (it's Electron based, but doesn't suck)
- CLI (automatically fires tests in headless mode)
- browser (Chrome, but support for others is on the way)
- framework support (Mocha)
- assertion support (Chai)

# What it looks like - GUI and CLI

The GUI of Cypress looks something like this:

![cypress_ui]({{ "/assets/images/blog_media/cypress/cypress_ui.png" | absolute_url }})

The left hand side shows all of the specs queued up, and you get nice color coded updates while tests are being ran. The toolbar on top is self explanatory, and it also includes an interactive selector you can use to locate elements more easily. Cypress also takes a snapshot of the DOM at every step, so you can rewind tests back to any previous state. Neat!

The rest of the window houses a full Chrome browser you can inspect and play around with. An interesting thing I noticed is that Chrome doesn't fully quit in between tests, so setups and teardowns take almost no time.

`npx cypress run` starts Cypress from the command line in headless mode - no extra configuration needed. Tests would run as expected, and all relevant results shown shown in console output. Failures would automatically generate screnshots and videos, but make sure to `.gitignore` them before pushing to a remote branch.

# DSL

The Cypress object `cy` is the heart of every test. You mostly interact with it to have tests load a url `cy.visit(url)`, get elements `cy.get(selector)`, as well as all of the page interactions you can think of - `cy.type(val)`, `cy.type({enter})`, `cy.click()`, `cy.check()`, `cy.invoke('val', valAmount).trigger('input')` (this moves sliders) - you get the idea.

A full list of all supported commands can be found on [the official guide](https://docs.cypress.io/api/commands/type.html#Syntax), and [this repo](https://github.com/cypress-io/cypress-example-recipes) has enough example tests to get you through most of the common cases.

As for assertions, Cypress supports Chai matchers, so you can append things like `.should('have.text', 'foo')`, `.should('have.class', '-approved')`, `expect($el).to.be.visible`, `.to.be.selcted` to anything you've selected or yielded. Official cheat sheet [here](https://docs.cypress.io/guides/references/assertions.html).

# A few gotchas

The team behind Cypress has drastic measures to some of the common testing areas.

Based on this [best practices guide](https://docs.cypress.io/guides/references/best-practices.html), they advise avoiding scenarios such as front end user logins, using conditionals, and small modular tests to name a few.

### Page objects vs actions

Cypress philosophy suggests to not rely on old automation models such as page objects and personas.

In short, the page object approach aims to structure test code by segmenting it into classes that follow the outline of your application.

Instead of having long and bulky test scripts, you'd have an assortment of slimmed classes that only know about the areas of the app they're delegated to interact with.

Cypress takes a more flexible approach by using actions - small, shared functions that group interactions based on what they do, not which part of the app they can work on.

{% gist b81f4f90871caa3bd027fceea0493961 %}

Say you're testing an online shop. Using the snippet above lets you call `cy.clearOrder()` at any point and from any test - `commands.js` is automatically imported into every spec file.

NB: You can roll as many custom commands as you'd like, but using `Cypress.Commands.add` expects your code to bind to the main `cy` object. Other helper functions can be added using separate helper files, then imported wherever necessary.  

### Mocking and stubbing

Cypress provides you with `cy.server()` and `cy.route()` functions that allow for control and manipulation of network traffic, so you can stub, route, and cheat your way through certain setups.

Moreover, there is a very handy library for VCR-ing external API dependencies called [cypress-autorecord](https://www.npmjs.com/package/cypress-autorecord).
The repo goes into more setup details, but it's as simple as installing the plugin, pasting a few configuration lines in `index.js`, and calling `autoRecord();` in spec files.

Auto record is not perfect and I did bump into a major issue with it, but managed to fix it relatively quickly. Apparently, because of the way it's implemented, auto recording multiple tests that all try to load the same URL throws an exception. But regardless of that, I managed to stub all external network traffic and reduce test execution time thre fold in half an hour.

### Asserting async values

Another interesting hurdle we had to overcome for the project under test related to core values returned to the front end. Some of them required multiple calculations, so they'd initially be rendered and then updated with their final values. This was getting progressively hard to track down, mainly because of the single page nature of the app. It was very hard to pinpoint why trickling updates were happening, and it all seemed very random, leading me to assume delayed API communication.

This proved hard for Cypress to assert, because it would pick the initial value and not seeing the final one show a bit later, automatically fail the whole test.

My initial take on this was to to try and throttle every action, so there's ample time for back end communication to catch up. Well, this mostly did the trick, except for the fact that tests would take double the time to run. Two minutes to run the whole test suite might not sound a lot for Ruby or Python stacks, but Cypress should be able to handle everything much faster.

{% gist a0a2b0847672a82bf4d95fbe9a85da47 %}

The second solution was much more simple and was achieved by bumping the default command timeout value like so:

{% gist d1237ec6ec83b4c6fb6d0a24bf29f987 %}

After digging through documentation, I noticed that Cypress is designed in a way that would not automatically fail a test if it's unable to find an element or assert a value. Instead, it would wait for the full timeout duration to pass first. Doubling default wait time served as a great remedy - if server communication doesn't slow calculations down, tests run just as fast. If the app takes more time to return the final calculated values, Cypress would wait for a few more seconds, pick the updated value, pass the test, then move on.

# Closing thoughts

A core philosophy of Cypress is that testing frameworks should be engaging and easy to work with, so they can be utilized by both developers and testers. Well, they certainly succeeded in doing so. Cypress is easy to set up, easy to pick up even with little programming knowledge. It also takes a lot of the setup burden away, so you can spend more time coding and less time configuring.

While reading up on the framework, I found a cool introductory video featuring two of my all time favorite people - Angie Jones and Jason Lengstorf. I've met with both of them separately at conferences, and watching them pair on tests was a delight. Enjoy!

{% include video id="NPnCz-hVQ-s" provider="youtube" %}
