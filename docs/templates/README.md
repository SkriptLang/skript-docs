# Skript Documentation Templates

When generating final result, each HTML file is surrounded by template.html,
which provides head element, navigation bar and so on.

## Template Patterns

Patterns have syntax of ${pattern_here}. For example, ${skript.version} is replaced with
current Skript version. Please see below for more...

You can also include other files by using ${include <filename>}. Just please make
sure that those included files don't have tags which are not allowed in position
where include is called.

## Pattern Reference
```
skript.* - Information of Skript
version - Skript's version
include <filename> - Load given file and place them here
generate <expressions/effects/events/types/functions> <loop template file> - Generated reference
content - In template.html, marks the point where other file is placed
```