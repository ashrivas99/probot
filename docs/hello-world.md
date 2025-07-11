---
next: development
title: Hello World
---

# Hello world

A Probot app is just a [Node.js module](https://nodejs.org/api/modules.html) that exports a function:

```js
export default (app) => {
  // your code here
};
```

The `app` parameter is an instance of [`Probot`](https://probot.github.io/api/latest/classes/probot.Probot.html) and gives you access to all of the GitHub goodness.

`app.on` will listen for any [webhook events triggered by GitHub](/docs/webhooks/), which will notify you when anything interesting happens on GitHub that your app wants to know about.

```js
export default (app) => {
  app.on("issues.opened", async (context) => {
    // A new issue was opened, what should we do with it?
    context.log.info(context.payload);
  });
};
```

The [`context`](https://probot.github.io/api/latest/classes/context.Context.html) passed to the event handler includes everything about the event that was triggered, as well as some helpful properties for doing something useful in response to the event. `context.octokit` is an authenticated GitHub client that can be used to [make REST API and GraphQL calls](/docs/github-api), and allows you to do almost anything programmatically that you can do through a web browser on GitHub.

Here is an example of an autoresponder app that comments on opened issues:

```js
export default (app) => {
  app.on("issues.opened", async (context) => {
    // `context` extracts information from the event, which can be passed to
    // GitHub API calls. This will return:
    //   { owner: 'yourname', repo: 'yourrepo', number: 123, body: 'Hello World !}
    const params = context.issue({ body: "Hello World!" });

    // Post a comment on the issue
    return context.octokit.issues.createComment(params);
  });
};
```

To get started, you can use the instructions for [Developing an App](/docs/development/).

Don't know what to build? Browse the [list of ideas](https://github.com/probot/ideas/issues) from the community for inspiration.
