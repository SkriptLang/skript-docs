# Event Values

Event values are a very important part of Skript. It sets up default expressions for most expressions and allows the user to grab values from the event they're wanting to listen to. Lets start with registering event values.

```java
package me.example.addontutorial.elements;

import org.bukkit.World;
import org.bukkit.block.Block;
import org.bukkit.event.player.PlayerBedEnterEvent;
import org.bukkit.event.player.PlayerBedEnterEvent.BedEnterResult;
import org.bukkit.event.player.PlayerChangedWorldEvent;
import org.eclipse.jdt.annotation.Nullable;

import ch.njol.skript.registrations.EventValues;
import ch.njol.skript.util.Getter;

public class Types {

	static {
		// PlayerBedEnterEvent
		EventValues.registerEventValue(PlayerBedEnterEvent.class, Block.class, new Getter<Block, PlayerBedEnterEvent>() {
			@Override
			@Nullable
			public Block get(PlayerBedEnterEvent event) {
				return event.getBed();
			}
		}, EventValues.TIME_NOW);
		EventValues.registerEventValue(PlayerBedEnterEvent.class, BedEnterResult.class, new Getter<BedEnterResult, PlayerBedEnterEvent>() {
			@Override
			@Nullable
			public BedEnterResult get(PlayerBedEnterEvent event) {
				return event.getBedEnterResult();
			}
		}, EventValues.TIME_NOW);

		// PlayerChangedWorldEvent
		EventValues.registerEventValue(PlayerChangedWorldEvent.class, World.class, new Getter<World, PlayerChangedWorldEvent>() {
			@Override
			@Nullable
			public World get(PlayerChangedWorldEvent event) {
				return event.getFrom();
			}
		}, EventValues.TIME_PAST);
	}

}

```

We're going to create a new class in `package me.example.addontutorial.elements;` no sub package needed here. Then we place a static block inside this class. Skript will call our code from our static block as Skript loads all classes within our elements package.

This calls `EventValues.registerEventValue` which is a class designated to event values. Here we're gonna add the block and BedEnterResult (we'll get into this more in this tutorial). This is pretty self explainatory in the terms of registering. We get the value from the event. Skript can support Arrays as of 2.7+

The last argument is the time, there is future, present and past, 1, 0 and -1 respectfully. This allows Skript and the user to grab different event values based on if it was before or after the event. This example has getFrom in the PlayerChangedWorldEvent event which then allows us to do

```
on world change:
    if the previous world was world "example":
        cancel event
        message "You're not allowed to change worlds!"
```

Do note that Skript already has these syntaxes, they're just examples.

So now that BedEnterResult.class I was talking about. This is a special enum class. This is where EventValueExpression comes into question. Rather than having `event-bedenterresult` each time, let's make a syntax that allows for `the bed enter result` to make things easier.

```java
package me.example.addontutorial.elements.expressions;

import org.bukkit.event.Event;
import org.bukkit.event.player.PlayerBedEnterEvent.BedEnterResult;
import org.eclipse.jdt.annotation.Nullable;

import ch.njol.skript.Skript;
import ch.njol.skript.doc.Description;
import ch.njol.skript.doc.Events;
import ch.njol.skript.doc.Examples;
import ch.njol.skript.doc.Name;
import ch.njol.skript.doc.Since;
import ch.njol.skript.expressions.base.EventValueExpression;
import ch.njol.skript.lang.ExpressionType;

@Name("Bed Enter Result")
@Description("The result of entering a bed if it may be cancelled or successful.")
@Examples({
	"on bed enter:",
		"\tthe bed enter result was not safe",
		"\tloop all monsters in radius 10 around player:",
			"\t\tadd 1 to {_monsters}",
		"\tmessage \"There are %{_monsters}% monsters nearby!\""
})
@Events("bed enter")
@Since("1.0")
public class ExprBedEnterResult extends EventValueExpression<BedEnterResult> {

	static {
		Skript.registerExpression(ExprBedEnterResult.class, BedEnterResult.class, ExpressionType.SIMPLE, "[the] bed enter result");
	}

	public ExprBedEnterResult() {
		super(BedEnterResult.class);
	}

	@Override
	public String toString(@Nullable Event event, final boolean debug) {
		return "bed enter result";
	}

}

```

This allows us to use that syntax in the event now. See the example present.
