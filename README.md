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
* Connection to the IRC server happens without external
  utilities.

  Bash support network connections via two virtual device
  directories it "creates" in '/dev/'. 

  '/dev/tcp/host/port' and '/dev/udp/host/port'.

  I don't exactly know _why_ these were implemented as
  it's a crazy feature for a shell to have. I haven't seen
  them widely used either.

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

* This was HARD.

  Trying to make the input prompt and async listener (which
  spits out messages to the terminal) play nice took a lot
  of work.

  As birch implements a TUI _manually_ we're dealing with bare
  escape sequences to tie everything together.

  When a message needs to be printed, the cursor must move from
  the input prompt to the output area, print the message and
  finally return back to the input prompt.

  All of this must happen just right to ensure that this series
  of cursor movements doesn't end up mangling the display of 
  the interface.

  Birch has gone through a lot of rewrites to get this perfect.
  Fun fact: The older revisions read input char by char and
  implemented a lot of readline by hand.
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

```
Ctrl+n - Next buffer.
Ctrl+p - Previous buffer.
Tab    - Completion of nicks and channels.

Further, all readline keybindings are available for use. See the
readline or bash manpages for a list of these. 

Keybindings to birch may also be set via a .inputrc file. Which
can be configured by setting `$BIRCH_INPUTRC`.

(BIRCH_INPUTRC=/path/to/birch-inputrc birch)
```


## Commands

```
Channels:

/join <channel>       - Join a channel.
/part <channel>       - Leave a channel.
/quit                 - Quit out of birch.

Messages: 

/msg <nick> <message> - Message a user.
/me  <message>        - Send an action.

Navigation:

/next                 - Next buffer.
/prev                 - Previous buffer.
/<num>                - Buffer by number (0 indexed).

Other:

/nick <nick>          - Change nickname.
/names                - Display all nicks in channel.
/topic                - Display channel topic.
/raw <args>           - Send a raw IRC message.
```


## Customization

```
(These are environment variables.)

# Set the formatting of the tab line's selected item.
# This defaults to reverse video.
BIRCH_STATUS='\e[7m'

# The path to a readline based .inputrc file to change
# birch's input settings. 
#
# See http://man7.org/linux/man-pages/man3/readline.3.html
BIRCH_INPUTRC=/path/to/file
```
