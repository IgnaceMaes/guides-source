In addition to configuring your app itself, you can also configure Ember CLI.
These configurations can be made by adding them to the `.ember-cli` file in your application's root. Many can also be made by passing them as arguments to the command line program.

For example, a common desire is to change the port that Ember CLI serves the app from. It's possible to pass the port number from the command line with `ember server --port 8080`, if you want to pass the port to your `npm start` script you would pass it with an extra `--` like this: `npm start -- --port 8080`. To make this configuration permanent, edit your `.ember-cli` file like so:

```json
{
  "port": 8080
}
```

For a full list of command line options, run `ember help`.

More information on configuring the Ember CLI can be found in the [CLI Guides](https://cli.emberjs.com/release/appendix/configuration/).
