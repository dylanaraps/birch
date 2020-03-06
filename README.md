# birch

An IRC client written in bash.

![scrot](https://user-images.githubusercontent.com/6799467/75714877-2793ab80-5cd5-11ea-8aad-92d7f245bdaf.jpg)

## Features

```
* Full power of readline for input and keybindings. 

  This is thanks to bash's `read -e`. Your `~/.inputrc` is also read 
  which allows for custom hotkeys (will later be extended to a birch 
  specific config file).

* Tab completion of nicks and channels. 
  
  Bash hardcodes `read -e` tab completion to the file based one. This 
  works fine for channel completion as each channel log is stored in 
  a file anyway. For nick completion, birch will create an empty file 
  for each nick in the channel. Hey, it works.

* Unique (or semi-unique) nick colors.

  All nicks are colored based on their length. The length is mapped
  to a color between 1 and 6 which is then used as input for 
  '\033[1;3<mapped_col>m'.
```

## Caveats (or limitations)

```
* Nick column is fixed and truncated to 10 columns wide.
  
  This is something fixable down the line. It merely serves to
  simplify the alignment of messages into two columns (nick and
  messages).

* Lines are word-wrapped to a fixed 60 columns.

  This is also fixable down the line though a lot more painful
  than the nick column issue. POSIX fold is used to achieve the
  word wrapping but doesn't take into account non-printable
  characters and unicode.

  What this means is that wrapping will always be a little _off_
  as escape sequences, IRC formatting and unicode will throw
  out all attempts at calculating the "visible" line length.

  It's an interesting problem to solve. I've made a myriad of
  attempts at writing a suitable function in bash though they're
  all too slow (as expected!).

* No automatic server reconnect.

  This should be fairly easy to fix though I need to figure out
  the best way of doing so.
```


## Interesting facts

```
* The input loop and listener loop can't communicate.

  Birch utilizes two loops to work. One for the input and 
  an additional loop to listen to incoming IRC messages.

  When something is run asynchronously in bash with '&', it
  runs in a subshell (a second bash process).

  What this means is that the async code can't communicate
  with the blocking code. Any variables set in the input loop
  once both loops are running won't be visible in the async
  listener loop to give an example.

  To work around this birch utilizes files for IPC between the
  two. The current channel is communicated by maintaining a 
  symlink and checking its target where needed.

  Example: .c -> '#current_channel'
```


## Usage

```sh
birch <args>

-s <host>
-c <channel>
-u <nick>
-p <server_password>
-U <server_username>
-P <port>
-x <cmd>

-h (help)
-v (version)
```

## Dependencies

- `bash 4+`
- POSIX compatible `fold`, `tail`, `touch`, `rm`, `ln`, `readlink`


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
