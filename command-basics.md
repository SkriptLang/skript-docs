# The Basics of Custom Commands
Custom commands are one of the most essential and core parts of writing scripts, which is why it's invaluable to have a good grasp on how to make them. This page will go over the basics of writing a command. The other command tutorial, [this page](commands.html), will have less explanation but will cover more complex bits and pieces, including things this tutorial may skip over or leave out for brevity.

## The Bare Bones

A command is made up of, at minimum, two things. A **command name**, the thing you type to execute the command, and a **trigger**, the code that runs when the command is executed. Skript organizes these two as follows:

```applescript
command /your-command-name:
    trigger:
        # your code here!
```
*secret tip: the / is actually optional!*

We tell Skript that we're making a command by writing `command`, then follow it up with the name of the command. After that, we move to the next line, indent because we're inside the command now, and use `trigger:` to tell Skript we want to write some code next.

This may be surprising, because events don't use `trigger:`, nor do functions. The reason is due to the other fields a command can have, like aliases, cooldowns, permissions, and other stuff we'll get to shortly.

Once we're inside the `trigger` section (with a new line and another indentation), we can start writing code like normal. Let's make this command do something, like broadcast text!

```applescript
command /broadcast:
    trigger:
        broadcast "Hello world!"
```

This is now a completely valid command you can run in your own server. It's basically the simplest viable command possible. Let's see about adding some complexity.

## Arguments

Our command can send text, but it can only send one thing. It'd be very useful if we can give our command some information and it'd respond to that. Say, for example, like telling it what to broadcast.

This is where **arguments** come in. I'm sure you've used Minecraft commands before, but just as a refresher, these are bits of information you can give the command so it can do more stuff. Like teleport you to a set of coords, or a specific player.

In Skript, they look like this:

```applescript
<name:type = default value>
```
Now that's a lot to take in, so let's simplify it for now. If you want to know more about the other bits, check out the [command reference page](commands.mdurl).

```applescript
<type>
```

We just need to tell Skript what `type` we want our argument to be. There's a whole list of `type`s on the [Types page](types.htmlorwhatever), but be aware that only some work in commands. This is because the type has to be able to be parsed from a string. Don't worry if you don't know what this means, it's only partially relevant.

The main point here is that we need to tell Skript what we're looking for. We want a broadcast command, so we're interested in text, or strings:

```applescript
command /broadcast <text>:
    trigger:
        broadcast "Hello world!"
```

See how the arguments go right after the command name? It's like how you would write it if you were actually running the command!

But we're not doing anything with this new bit of input yet. No matter what we type when running the command, it still broadcasts `"Hello world!"`. This is where the [argument expression](argumentexpressionurlhere) comes in: 

```
[the] last arg[ument]
[the] arg[ument](-| )<(\d+)>
[the] <(\d*1)st|(\d*2)nd|(\d*3)rd|(\d*[4-90])th> arg[ument][s]
[(all [[of] the]|the)] arg[ument][s]
[the] %*type%( |-)arg[ument][( |-)<\d+>]
[the] arg[ument]( |-)%*type%[( |-)<\d+>]
```

This jumble of syntax is the many ways to reference an argument inside a command. Here's a few examples:

```applescript
the 1st argument
arg-1
arg 1
arg-text
text argument 1
last arg
```

So let's use one of these. I'm going to use `arg 1` as it's pretty easy to write, it's clear, and it can't be confused with subtraction like `arg-1` can.

```applescript
command /broadcast <text>:
    trigger:
        broadcast arg 1
```

Now our command broadcasts whatever we want! However, anyone can use this command, and we don't want our users broadcasting whatever they want. Let's add some restrictions.

## Permissions and Executable By

Commands are a lot more than just names, arguments, and triggers. They have a whole host of other fields you can use to add more features and functionality. We'll start with the two that determine who can run the command and who can't.

Firstly, permissions. This is a very simple field that is just a permission the player needs to run the command.

```applescript
command /broadcast <text>:
    permission: server.broadcast
    trigger:
        broadcast arg 1
```

Now any player with the permission can run the command and any player without it can not. But let's be even more secure. Let's say we only want the console to be able to run this command. This is what the `executable by` field is for. It can be set to `players`, `console`, or `players and console`. If you leave it out, both players and console can run it.

```applescript
command /broadcast <text>:
    permission: server.broadcast
    executable by: console
    trigger:
        broadcast arg 1
```

Most of the time, executable by isn't very necessary, but it's good practice to exclude the console if you're ever doing something that involves the `player` instead of just the `sender`. Speaking of the sender, let's talk about the command sender!

## Command Sender

There's a bit of information that every command gets for free, even without arguments. This is the `command sender`, which can either be a `player` or `console`. You can reference it with `command sender` or `sender`:

```applescript
command /broadcast <text>:
    trigger:
        broadcast "%sender% says %arg 1%"
        if player is set:
            broadcast "sender is %player%, a player!"
        else:
            broadcast "sender must be %console%!"
```

If you know it's a player, you can use `player` or `event-player`, and if you know it's console, well, you can just use `console`. If it was executed by console, `player` will not be set, which is why using `executable by` is good practice!

## Aliases

I don't have a good transition for this section.

Aliases are alternate names for your command. Think `/tp` and `/teleport`. Skript gives you a field to tell it what aliases you want for your command:

```applescript
command /broadcast <text>:
    permission: server.broadcast
    aliases: /announce, yell
    trigger:
        broadcast arg 1
```

However, Skript does not actually register commands under these names, so they won't show up in tab complete, and if another plugin already has that command name, your command won't overwrite the existing one! If you run into these issues, consider writing a small dummy command that just redirects to your main one:

```applescript
command /yell <text>:
    permission: server.broadcast
    trigger:
        make sender execute command "/broadcast %arg 1%"
```

## Cooldowns

One of the *coolest* things about Skript commands is their built-in cooldowns. I'll only give a very basic example here, but you can see the [main command tutorial](commands.md) for all the options. Let's say we only want players to broadcast things every five minutes. We can simply use the `cooldown` field:

```applescript
command /broadcast <text>:
    cooldown: 5 minutes
    trigger:
        broadcast arg 1
```

Perfect! That's all we need to do! However, the message is simply "You are using this command too often." Let's tell our players how much longer they have to wait. We can do this with the `cooldown message` field and the `remaining time` expression:

```applescript
command /broadcast <text>:
    cooldown: 5 minutes
    cooldown message: You must wait %remaining time% before broadcasting again!
    trigger:
        broadcast arg 1
```



