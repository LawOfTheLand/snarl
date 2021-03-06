snarl ![snarl](/snarl-headshot.png)
===================================
[![NPM version](	https://img.shields.io/npm/v/snarl.svg?style=flat-square)](https://www.npmjs.com/package/snarl)
[![Build Status](https://img.shields.io/travis/martindale/snarl.svg?branch=master&style=flat-square)](https://travis-ci.org/martindale/snarl)

Simple chat bot (currently for Slack), extensible with plugins.

Despite his name, snarl is just a fuzzy and friendly goofball.  He's been with
us for several years, since the beginning of [the Coding
Soundtrack](https://soundtrack.io) community.  He's an awesome automaton that
helps us with a great many things, so be nice to him.

Avatar for snarl is by [@yiyinglu](https://github.com/yiyinglu), who designed
the original avatars for tuntable.fm.

If you'd like to test him out, join us on [the Maki Slack](https://chat.maki.io), where snarl helps us develop [the Maki Framework](https://maki.io).

## Quick Start
You'll need to create a Bot User for your Slack organization, generally found at
`https://YOUR-TEAM-NAME.slack.com/services/new/bot`.  Once the integration is
added, you'll be given an API token: **place this token into
`config/index.json`**.  For more help, Slack has [great documentation on bot
users][slack-bots].

1. Install via `npm install snarl -g`, or simply clone<sup>1</sup> this repository and run `npm install` as usual.
2. Modify `config/index.json` to contain your Slack token (see paragraph above).
3. Execute `npm start` in the source directory, or `snarl` if you installed globally.

That's it.  You'll see snarl come online!  If you install snarl globally via
`npm install snarl -g`, you can also simply type `snarl` at any time (for example,
inside of a screen or a tmux session) to run the bot.

<small>1: if you want to make modifications, you should [fork it first][fork]!</small>

[slack-bots]: https://api.slack.com/bot-users
[fork]: https://github.com/martindale/snarl/fork

## Naming Your Bot
If you want to give snarl a different name, you can configure it via Slack (see
link above), or add a `name` property to `config/index.json`.

## Plugins
snarl supports plugins.  We've kept the default list short but fun.  We'd love
to see even more contributions!

Plugins for snarl can add commands or other functionality.  For example, the
included `karma` plugin lets snarl keep track of karma for various users.

### Using Plugins
Plugins can be autoloaded from either a single file in
`./plugins/plugin-name.js` or an NPM module named `snarl-plugin-name`.  To
autoload a plugin, add the plugin name to the plugins array in
`config/index.json`:

```json
{
  "name": "snarl",
  "plugins": ["erm", "karma"],
  "store": "data/store",
  "slack": {
    "token": "some-token-xxxooooo",
  },
}
```

...and simply call `autoload()`:

```js
var Snarl = require('snarl');
var snarl = new Snarl();

// autoload plugins found in `config/index.json`
snarl.autoload();

// start snarl, as normal
snarl.start();
```

To use another plugin, simply require it, as follows:

```js
var Snarl = require('snarl');
var snarl = new Snarl();

// import the karma plugin
var karma = require('./plugins/karma');

// use the karma plugin we required above
snarl.use(karma);

// start snarl, as normal
snarl.start();
```

### Included Plugins
The list of available plugins (via `./plugins/plugin-name`) is as follows:

- `karma`, which keeps track of user karma, as incremented by `@username++`.
- `facts`, which provides `!TopologyFacts` (mathematical topology facts), `!SmiffFacts` (facts about Will Smith), and `!InterstellaFacts` (facts about [Interstella 5555](https://en.wikipedia.org/wiki/Interstella_5555:_The_5tory_of_the_5ecret_5tar_5ystem))
- `meetups`, which responds with a simple message telling your community about in-person meetups.
- `erm`, which transforms the text of a user message into `ERMEGERD` speech using [martindale/erm](https://github.com/martindale/erm).
- `beer-lookup`, which provides `!brew <beerName>` to look up and describe a beer via [BreweryDB](http://www.brewerydb.com/).

### Other Plugins
- [snarl-eliza](https://github.com/martindale/snarl-eliza) is a simple AI using
the ELIZA self-help chatbot created by Joseph Weizenbaum between 1964 and 1966.

### Writing Plugins
To write a snarl plugin, create a new NPM module that exports a map of triggers
your bot will respond to.  You can use either a simple message string, or a
function that expects a callback:

```js
module.exports = {
  'test': 'Hello, world'
};
```

For more complex functionality, use the callback feature:

```js
module.exports = {
  'test': function(msg, done) {
    // simulate an asynchronous task...
    setTimeout(function() {
      // error is first parameter, response is second
      done(null, 'Your message was: ' + msg.text + '\nYour triggers were:' + msg.triggers);
    }, 1000);
  };
}
```

We ask that you publish your plugins via npm, name them `snarl-yourplugin`, and
add both the `snarl` and `slack` keywords to your `package.json`.

Thanks!  We hope you enjoy.
