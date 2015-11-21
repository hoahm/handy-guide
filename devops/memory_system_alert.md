This guide show you how to implement script alerting when system memory is running out

# Add slackpost command

See this [guide](slackpost.md)

# Add system_alert script

Open text editor, and add the following file:

    $ vi /usr/local/bin/system_alert

    #!/bin/bash

    # Minimum available memory limit, MB
    THRESHOLD=400

    # Check time interval, sec
    INTERVAL=60

    # Slack webhook
    SLACK_WEBHOOK_URL="<slackbot-url>"

    # Slack channel 
    SLACK_CHANNEL="<slackbot-channel>"

    # Slack bot username
    SLACK_USERNAME="<server-name>"

    while :
    do

        free=$(free -m|awk '/^Mem:/{print $4}')
        buffers=$(free -m|awk '/^Mem:/{print $6}')
        cached=$(free -m|awk '/^Mem:/{print $7}')
        available=$(free -m | awk '/^-\/+/{print $4}')

        message="Free $free""MB"", buffers $buffers""MB"", cached $cached""MB"", available $available""MB"""

        if [ $available -lt $THRESHOLD ]
            then
            slackpost $SLACK_WEBHOOK_URL $SLACK_CHANNEL $SLACK_USERNAME "Memory is running out! $message"
        fi

        sleep $INTERVAL
    done

Then, add execute permission

    $ chmod +x /usr/local/bin/system_alert

# Run script system_alert in background

    $ /usr/local/bin/system_alert & 
