



Upload example templates to github
Create streaming downloader/unzipper for templater in cli program for JS app
Create "watcher" app that will a fs watcher and automatically rebuild with esbuild and publish
Look at https://shopify.dev/docs/apps/functions/language-support/javascript to see how Javy exposes everything to developers

Figure out how to expose custom functions into Javy so that the host functions can be added (it only has )

Read https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop to understand event loop better and why javy doesn't have support for it

Build a basic SDK and publish to NPM that just does the logging call all the way from JS to C#

Set up logger to write to SQLite when in development mode

Build example UI that can read the logs in realtime

Then extend the SDK to include other basic functions

Record demo of hot reloading development with realtime log viewers

See how Javy does function-runenr https://github.com/Shopify/function-runner