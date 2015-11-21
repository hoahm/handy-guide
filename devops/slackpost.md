This guide will show you how to add a slackbot command to shell. It's useful for system alert, ...

# Add Slack integration

Go to Slack integration, add new Slackbot. Then test if Slackbot work correctly.

    curl -i \
         -H "Content-Type: application/json" \
         -X POST "<slackbot-url>" \
         -d '{ "channel": "<slack-channel>", "<slack-username>": "system-alert", "icon_emoji": "ghost", "attachments": [ "color": "danger", "title": "test test", "text": "haha" ] }'

# Add slackpost command to system

Open text editor, and add the following file: 

    $ vi /usr/local/bin/slackpost

    #!/bin/bash

    # Usage: slackpost "<webhook_url>" "<channel>" "<username>" "<message>"

    # ------------
    webhook_url=$1
    if [[ $webhook_url == "" ]]
    then
            echo "No webhook_url specified"
            exit 1
    fi

    # ------------
    shift
    channel=$1
    if [[ $channel == "" ]]
    then
            echo "No channel specified"
            exit 1
    fi

    # ------------
    shift
    username=$1
    if [[ $username == "" ]]
    then
            echo "No username specified"
            exit 1
    fi

    # ------------
    shift

    text=$*

    if [[ $text == "" ]]
    then
            echo "No text specified"
            exit 1
    fi

    escapedText=$(echo $text | sed 's/"/\"/g' | sed "s/'/\'/g" )

    json="{\"channel\": \"$channel\", \"username\":\"$username\", \"icon_emoji\":\"ghost\", \"attachments\":[{\"color\":\"danger\" , \"text\": \"$escapedText\"}]}"

    curl -s -d "payload=$json" "$webhook_url"

Then, add execute permission

    $ chmod +x /usr/local/bin/slackpost

# Test if it work

    $ /usr/local/bin/slackpost "<slackbot-url>" "<slack-channel>" "<slack-username>" "Test test test"
