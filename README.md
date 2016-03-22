# Installing Inbucket with Homebrew

Inbucket is not (yet) part of Homebrew master, so an extra step is required to
install the tap first.  Note that the launch at startup commands provided by
Homebrew are for running as the root user, which is not necessary.

## Installing

    brew tap jhillyerd/inbucket
    brew install --HEAD inbucket
    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/opt/inbucket/*.plist ~/Library/LaunchAgents/

## Starting

    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.inbucket.plist
    cat /usr/local/var/log/inbucket.log

Confirm Inbucket started, you should see `[INFO ]` statements, but no `[ERROR]` ones.

## Send Test Message

Run `telnet localhost 2500` and paste the following into your terminal:

    HELO localhost
    MAIL FROM:<a.wiki@github.com>
    RCPT TO:<friend@anywhere.com>
    DATA
    From: a.wiki@github.com
    To: friend@anywhere.com
    Subject: Test Message
    
    This is a test message from the command line.
    .
    QUIT

(you may need to press return to transmit the final QUIT)

Check that "friend" received your message: <http://localhost:9000/mailbox?name=friend>

## Shutdown

    launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.inbucket.plist

## Removal

    rm ~/Library/LaunchAgents/homebrew.mxcl.inbucket.plist
    brew uninstall inbucket
    rm /usr/local/etc/inbucket.conf /usr/local/var/log/inbucket.log
    rm -rf /usr/local/var/inbucket
