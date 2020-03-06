# birch

An IRC client written in bash.

![scrot](https://user-images.githubusercontent.com/6799467/75714877-2793ab80-5cd5-11ea-8aad-92d7f245bdaf.jpg)

## Usage

```sh
birch [-s host -u nick -p pass -P port -c chan -x cmd]
```

## Dependencies

- `bash 4+`
- POSIX compatible `fold`, `tail`, `touch`, `rm`, `ln`


## Keybindings

| combo                      | action                       |
| -------------------------- | ---------------------------- |
| `Ctrl` + `n`               | Next buffer                  |
| `Ctrl` + `p`               | Prev buffer                  |
| `Tab`                      | Complete nicks and channels  |
| ALL READLINE BINDINGS      | See: https://linux.die.net/man/3/readline  |


## Commands

Channels:

| command                    | action                  |
| -------------------------- | ----------------------- |
| `/join <channel>`          | Join a channel.         |
| `/part <channel>`          | Leave a channel.        |
| `/quit`                    | Quit out of birch.      |

Messages:

| command                    | action                  |
| -------------------------- | ----------------------- |
| `/msg <nick> <message>`    | Message a user.         |
| `/me <message>`            | Send an action.         |

Navigation:

| command                    | action                  |
| -------------------------- | ----------------------- |
| `/next`                    | Next buffer.            |
| `/prev`                    | Previous buffer.        |
| `/<num>`                   | Buffer by number.       |

Other:

| command                    | action                  |
| -------------------------- | ----------------------- |
| `/nick <nick>`             | Change nickname.        |
| `/names`                   | All nicks in channel.   |
| `/topic`                   | Channel topic.          |
| `/raw <args>`              | Send a raw IRC message. |
