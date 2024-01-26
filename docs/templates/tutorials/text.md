## Text (String)

Skript allows you to write pieces of text (programmers usually call them strings) in the scripts.
This is done by putting the text inside double quotes, as follows:
```code
"this is text"
```
Simple, isn't it? If an effect, expression, condition, trigger or function accepts something of type text or string,
 you can use this format to write it right there!

## Formatting Text

But isn't just text a bit boring?
Worry not, as Minecraft has support for colors, styles and other formatting options in chat.
Most of the options also work with item and entity names.

## Colors
Minecraft has 16 pre-set color codes to be used in text. Skript supports them in two different ways:
```
Color name tags, for example <red> Minecraft color codes, like §c; using & works, too
```
Here's a table of all colors, including both Skript names and color codes:
<table>
    <tr style="font-weight: 700">
        <th>Color</th>
        <th>Code</th>
        <th>Name</th>
        <th>Alternative Names</th>
    </tr>
    <tr>
        <td style="
              background-color: #000000;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§0</td>
        <td>black</td>
        <td></td>
    </tr>
    <tr>
        <td style="
              background-color: #0000aa;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§1</td>
        <td>blue</td>
        <td>dark blue</td>
    </tr>
    <tr>
        <td style="
              background-color: #00aa00;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§2</td>
        <td>green</td>
        <td>dark green</td>
    </tr>
    <tr>
        <td style="
              background-color: #00aaaa;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§3</td>
        <td>cyan</td>
        <td>aqua, dark cyan, dark aqua, dark turquoise, dark turquois</td>
    </tr>
    <tr>
        <td style="
              background-color: #aa0000;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§4</td>
        <td>red</td>
        <td>dark red</td>
    </tr>
    <tr>
        <td style="
              background-color: #aa00aa;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§5</td>
        <td>purple</td>
        <td>dark purple</td>
    </tr>
    <tr>
        <td style="
              background-color: #ffaa00;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§6</td>
        <td>orange</td>
        <td>orange, gold, dark yellow</td>
    </tr>
    <tr>
        <td style="
              background-color: #aaaaaa;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§7</td>
        <td>grey</td>
        <td>light grey, gray, light gray, silver</td>
    </tr>
    <tr>
        <td style="
              background-color: #555555;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§8</td>
        <td>dark gray</td>
        <td>dark grey</td>
    </tr>
    <tr>
        <td style="
              background-color: #5555ff;
              color: #ffffff;
              width: 10px;
              height: 10px;
            "></td>
        <td>§9</td>
        <td>light blue</td>
        <td>light blue, indigo</td>
    </tr>
    <tr>
        <td style="
              background-color: #55ff55;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§a</td>
        <td>light green</td>
        <td>lime, lime green</td>
    </tr>
    <tr>
        <td style="
              background-color: #55ffff;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§b</td>
        <td>light cyan</td>
        <td>light aqua, turquoise, turquois, light blue</td>
    </tr>
    <tr>
        <td style="
              background-color: #ff5555;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§c</td>
        <td>light red</td>
        <td>pink</td>
    </tr>
    <tr>
        <td style="
              background-color: #ff55ff;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§d</td>
        <td>magenta</td>
        <td>light purple</td>
    </tr>
    <tr>
        <td style="
              background-color: #ffff55;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§e</td>
        <td>yellow</td>
        <td>light yellow</td>
    </tr>
    <tr>
        <td style="
              background-color: #ffffff;
              color: #000000;
              width: 10px;
              height: 10px;
            "></td>
        <td>§f</td>
        <td>white</td>
        <td></td>
    </tr>
</table>

In **Minecraft 1.16**, support was added for **6-digit** hexadecimal colors to specify custom colors other than the 16 default color codes.
A new tag can be used to format with these colors. The tag looks like this:
```
<##hex code>
```
Here's what the tag would look like when used in a script:
```code
send "<##123456>Hey %player%!" to player
```
For information not related to Skript, see the [Minecraft Wiki page](https://minecraft.wiki/w/Formatting_codes#Color_codes) concerning colors.
Note that depending on Skript configuration, color codes may do more than just change color of text after them.
By default, for backwards compatibility, they clear following styles: **magic**, **bold**, **strikethrough**, **underlined**, **italic**.
Other styles are not affected, and this feature can be toggled off in config.sk.

## Other Styles

Minecraft also has various other styles available.
The following are available everywhere, including item and entity names:

| Code | Name                                                                        | Alternative Names |
|------|-----------------------------------------------------------------------------|-------------------|
| §k   | magic&nbsp;<span class="magic-text" style="position: absolute;">test</span> | obfuscated        |
| §l   | **bold**                                                                    | b                 |
| §m   | ~~strikethrough~~                                                           | strike, s         |
| §n   | <span style="text-decoration: underline">underlined</span>                  | underline, u      |
| §o   | *italic*                                                                    | italics, i        |
| §r   | reset                                                                       | r                 |

If it wasn't clear from the table, §r clears all other formatting and colors.
You'll probably use it quite often when sending chat messages from scripts.

Skript also supports certain newer features, which are only available in chat.
Those do not have formatting codes, so you must use tags for them.

| Name            | Alternative Names | Description                                                                      |
|-----------------|-------------------|----------------------------------------------------------------------------------|
| link            | open url, url     | Opens a link when player clicks on text                                          |
| run command     | command, cmd      | Makes player execute a chat command when they click on text                      |
| suggest command | sgt               | Adds a command to chat prompt of player when clicked                             |
| tooltip         | show text, ttp    | Shows a tooltip when players hover over text with their mouse                    |
| font            | f                 | Changes the font of the text (Requires Minecraft 1.16+)                          |
| insertion       | insert, ins       | Will append a text at the player's cursor in chat input only while holding SHIFT |

All of these styles require a parameter, in the format:
```code
<name:parameter>
```
For link, parameter must be either http or https url if you want clients to recognize it.
For others, it can be any text you would like (you can make player run invalid commands if you wish).

## Text and Variables

Variable names are text, but obviously formatting that text does no good.
However, everything else you can do for text, you can do for variable names.
A guide about this is coming... some day.

Guide written by [bensku](https://github.com/bensku).
