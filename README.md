# slackcat

slackcat is a command line tool that posts messages to [Slack].

    $ echo "hello" | slackcat

## Installation

If you have a working Go installation run `go get github.com/skattyadz/slackcat`.

There's also a debian package in the [MxM APT Repo]

## Configuration

You need to create an [Incoming Webhook integration][new-webhook].

You can then configure slackcat through a config file and/or environment variables.

### JSON File

```json
{
    "webhook_url":"https://hooks.slack.com/services/super/secret/token"
}
```

In `/etc/slackcat.conf`, `~/.slackcat.conf` or `./slackcat.conf`

Optional keys: `channel`, `username`, `icon_emoji`, `proxy`. See `slackcat-example.conf` for
a full example.


### Environment Variable

```bash
$ export SLACKCAT_WEBHOOK_URL=https://hooks.slack.com/services/super/secret/token
```

Will override file config.

Optional vars: `SLACKCAT_CHANNEL`, `SLACKCAT_USERNAME`, `SLACKCAT_ICON`

## Usage

slackcat will take each line from stdin and post it as a message to Slack:

    tail -F logfile | slackcat

Be aware that if a file outputs blank lines, this will result in a 500 error from slack. You can remedy this using
grep to filter out blank lines:

    tail -F logfile | grep --line-buffered -v '^\s*$' | slackcat

If you'd prefer to provide a message on the command line, you can:

    sleep 300; slackcat "done"

### Name
Default: `user@hostname`

    echo ":coffee:" | slackcat --name "coffeebot"

### Channel
Default: Slack Webhook default

    echo "don't forget the # sign" | slackcat --channel #test
    echo "message from slackbot" | slackcat --channel @skattyadz

### Icon ([emoji])

    echo "we're watching you" | slackcat --icon=:family:



[Slack]: http://slack.com/
[new-webhook]: https://my.slack.com/services/new/incoming-webhook
[emoji]: http://www.emoji-cheat-sheet.com/
[MxM APT repo]: http://apt.mxmdev.com/
