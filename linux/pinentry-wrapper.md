# Pinentry wrapper script

The wrapper script reads an environment variable to decide if either a cli or graphical pinentry program should be used.

I started using this script when I wanted  [pass](https://www.passwordstore.org/) to work in qutebrowser.

## Create script

Create file `/usr/bin/pinentry-auto`:

```bash
#!/usr/bin/env bash

case $PINENTRY_USER_DATA in
  gtk) exec /usr/bin/pinentry-dmenu "$@" ;;
  none) exit 1 ;;
  *) exec /usr/bin/pinentry-tty "$@" ;;
esac
```

Make it executable: `sudo chmod ugo+x /usr/bin/pinentry-auto`

## Configure `gpg-agent`

Add the following line to `~/.gnupg/gpg-agent.conf`:

```
pinentry-program /usr/bin/pinentry-auto
```

## Usage

When starting a program set an environment variable to tell the gpg-agent to use a graphical pinentry program:

```
PINENTRY_USER_DATA=gtk qutebrowser
```

## Links

* https://github.com/qutebrowser/qutebrowser/blob/master/misc/userscripts/qute-pass
* https://www.gnupg.org/documentation/manuals/gnupg/GPG-Configuration.html#index-PINENTRY_005fUSER_005fDATA
* https://gnupg.org/documentation/manuals/gnupg-2.0/Agent-Options.html
