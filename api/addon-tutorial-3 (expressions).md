# Creating an Expression

### SimpleExpression

Let's start with a SimpleExpression this is a class of Skript which represents the base expression.

A simple expression typically returns one or many values, and it also typically doesn't include any expressions in the pattern like `example %something%`

It's typically like `[all] players` or `[all] worlds`

Practically all expressions can have changers which are set, add, remove, delete, etc. So lets go through all the methods of a SimpleExpression one by one and create one ourselves.

First we need to create a new class, and a new package to support our syntaxes. Let's create another package inside our elements package, so the name should be `me.example.addontutorial.elements.expressions` you right click the elements package to create a sub package of it.

When naming expressions it is a Skript recommendation to call it ExprNAME. This is just how Skript likes to name it's expressions. Again it's your addon, but using these naming methods are highly recommended as it helps other developers understand your coding if someone reports an error or something.

So for my example i'm going to create a new class called ExprExample and it's going to extend SimpleExpression.

Now SimpleExpression's need a generic type. Which is the Object inside the SimpleExpression<stuff>

The "stuff" that goes inside this, should be your returning type. So if I wanted to return the name of a player.

With the syntax `[the] name[s] of %players%` (Everything in [these braces] are optional)

This would be returning a String. Strings are text surrounded by "quotes". We need to extend this expression with a string. So our setup will look like this:

```java
package me.example.addontutorial.elements.expressions;

import org.bukkit.event.Event;
import org.eclipse.jdt.annotation.Nullable;

import ch.njol.skript.Skript;
import ch.njol.skript.doc.Description;
import ch.njol.skript.doc.Examples;
import ch.njol.skript.doc.Name;
import ch.njol.skript.doc.Since;
import ch.njol.skript.lang.Expression;
import ch.njol.skript.lang.ExpressionType;
import ch.njol.skript.lang.SkriptParser.ParseResult;
import ch.njol.skript.lang.util.SimpleExpression;
import ch.njol.util.Kleenean;
import ch.njol.util.coll.CollectionUtils;

@Name("Example Name With Case")
@Description("An example simple expression.")
@Examples({
	"# This is a multiple line example",
	"if balance of player is greater than 10:",
		"\tset {_variable} to our example" // Use \t in examples and tab it over for readability.
})
//@RequiredPlugins({"Paper 1.17+", "ProtocolLib"})
//@RequiredPlugins("Spigot 1.13+")
//@Events("player death")
@Since("1.0")
public class ExprExample extends SimpleExpression<String> {

	static {
		Skript.registerExpression(ExprExample.class, String.class, ExpressionType.SIMPLE, "[our] example");
	}

	@Override
	public boolean init(Expression<?>[] exprs, int matchedPattern, Kleenean isDelayed, ParseResult parseResult) {
		return true;
	}

	@Override
	@Nullable
	protected String[] get(Event event) {
		return CollectionUtils.array("Hello World!");
		//return new String[] {"Hello World!"};
	}

	@Override
	public boolean isSingle() {
		return true;
	}

	@Override
	public Class<? extends String> getReturnType() {
		return String.class;
	}

	@Override
	public String toString(@Nullable Event event, boolean debug) {
		return "our example";
	}

}

```

Alright so let's break this SimpleExpression.

The static block at the top is how Skript will register the syntax, in property expressions there is a `register` method to simplify this step.

We use registerExpression from the static method of the Skript class, the first argument is our class, the second is the return type of our expression, which may be removed in the future as it's now possible in Java to get generics, the third argument is the expression type, we'll go over all the type in this tutorial page, but we're making a simple expression, so we'll use the SIMPLE enum, and lastly is a VarArg String of patterns, you can use multiple strings like `"[our] example", "another pattern"`, you can also use expressions in these patterns, but we'll go over that for the next expression.

Next is the important init method. This is the main communication between your syntax and Skript. This method provides you all the details you'll need that come from how the Skript user used your pattern.
The expressions argument list returns any expressions that were used. Example being if we used `[our] example %players%` the first index of the array will be our Expression<Player> which can be null, but the array will not be empty.

Next is the matchedPattern, if we used two patterns in the register as explained above, it'll be either 0 or 1, etc. The Kleenean is a tri-state boolean, TRUE, FALSE or UNKNOWN. This is if the user used `wait a second` before calling your expression. This can be useful if you want your expression to be something instant, like an event related value, that'll be changed after time has passed. It'll only be UNKNOWN if another addon has changed it's value, or the API has since changed from this tutorial. Kleenean is mainly used in other parts of Skript.

The ParseResult is a collection of methods to grab many details from the result of Skript parsing your expression, there are fields like hasTag (parser tags), marks, regexes and the raw expr. More on this is posted in the api pattern explanation tutorial.

get() this is the main method that returns values. This is where all the juice is. Here we return what we want. In our case we just return a simple "Hello World!" string. We need to return as an Array because our expression could support multiple strings if needed. This is called after the init method, so you'll be able to use private fields.

So for this we returned a string, with two different ways, one way is using Skript's own CollectionUtils class which places whatever the argument is, into an array for you. This can be helpful in some cases, and it's easier to read. The other way is done with normal Java.

isSingle() this method tells Skript if this expression returns as a list of String's or just one String. In our case it's just one string in that array we sent out, so we return true. This is called after the init method, so you'll be able to use private fields.

getReturnType() method should return the class of the type you want to return. This copies the Generic type we specified in the extends portion. And we wanted a String. So it should return String.class. This method can mainly be useful if your generic is an Object and you want to return a different type based on which pattern the user is using. This is called after the init method, so you'll be able to use private fields.

Lastly is the toString, this method is mainly only used for debugging and is printed when your syntax has an error.

In our example here we're just gonna return our pattern, it's best to return the pattern in this toString so you can understand the syntax when it errors. More about this will be explained below with other types of expressions.
This is called after the init method, so you'll be able to use private fields.

### PropertyExpression

Now we have property expressions. These are patterns in the format of

```
active potion effects of %livingentities%
%livingentities%'[s] active potion effects
```

Keep in mind that active potion effects already exists in Skript, this is just an example.

This pattern happens quite often. So PropertyExpression is a utility class for exactly these patterns. Lets go over the differences between a property expression and simple expression, here is our final class:

```java
package me.example.addontutorial.elements.expressions;

import java.util.Arrays;

import org.bukkit.entity.LivingEntity;
import org.bukkit.event.Event;
import org.bukkit.potion.PotionEffect;
import org.bukkit.potion.PotionEffectType;
import org.eclipse.jdt.annotation.Nullable;

import ch.njol.skript.classes.Changer.ChangeMode;
import ch.njol.skript.doc.Description;
import ch.njol.skript.doc.Examples;
import ch.njol.skript.doc.Name;
import ch.njol.skript.doc.Since;
import ch.njol.skript.expressions.base.PropertyExpression;
import ch.njol.skript.lang.Expression;
import ch.njol.skript.lang.SkriptParser.ParseResult;
import ch.njol.util.Kleenean;
import ch.njol.util.coll.CollectionUtils;

@Name("Active Potion Effects")
@Description("All of the active potion effects of living entities.")
@Examples("remove all active potion effects from all the players")
@Since("1.0")
public class ExprActionPotionEffects extends PropertyExpression<LivingEntity, PotionEffect> {

	static {
		register(ExprActionPotionEffects.class, PotionEffect.class, "[all [of]] [the] active potion effects", "livingentities");
		//registerDefault(ExprActionPotionEffects.class, PotionEffect.class, "[all [of]] [the] active potion effects", "livingentities");
	}

	@Override
	@SuppressWarnings("unchecked")
	public boolean init(Expression<?>[] exprs, int matchedPattern, Kleenean isDelayed, ParseResult parseResult) {
		setExpr((Expression<? extends LivingEntity>) exprs[0]);
		return true;
	}

	@Override
	protected PotionEffect[] get(Event event, LivingEntity[] source) {
		//return get(source, entity -> entity.getSingleValue());
		return Arrays.stream(source)
				.flatMap(entity -> entity.getActivePotionEffects().stream())
				.toArray(PotionEffect[]::new);
	}

	@Override
	@Nullable
	public Class<?>[] acceptChange(ChangeMode mode) {
		switch (mode) {
			case REMOVE:
			case REMOVE_ALL:
				return CollectionUtils.array(PotionEffect[].class, PotionEffectType[].class);
			case ADD:
			case SET:
				return CollectionUtils.array(PotionEffect[].class);
			case DELETE:
				return CollectionUtils.array();
			case RESET:
			default:
				return null;
		}
	}

	@Override
	public void change(Event event, @Nullable Object[] delta, ChangeMode mode) {
		for (LivingEntity entity : getExpr().getArray(event)) {
			switch (mode) {
				case REMOVE_ALL:
				case REMOVE:
					assert delta != null;
					for (Object object : delta) {
						if (object instanceof PotionEffect) {
							entity.removePotionEffect(((PotionEffect) object).getType());
							continue;
						}
						assert object instanceof PotionEffectType;
						entity.removePotionEffect((PotionEffectType) object);
					}
					break;
				case SET:
				case DELETE:
					for (PotionEffect effect : entity.getActivePotionEffects())
						entity.removePotionEffect(effect.getType());
					if (mode != ChangeMode.SET)
						break;
					//$FALL-THROUGH$
				case ADD:
					assert delta != null;
					for (PotionEffect effect : (PotionEffect[]) delta)
						entity.addPotionEffect(effect);
					break;
				case RESET:
				default:
					break;
			}
		}
	}

	@Override
	public boolean isSingle() {
		return false;
	}

	@Override
	public Class<? extends PotionEffect> getReturnType() {
		return PotionEffect.class;
	}

	@Override
	public String toString(@Nullable Event event, boolean debug) {
		return "active potion effects of " + getExpr().toString(event, debug);
	}

}

```
You can see key differences with this class now. The generics at the top are the left intake and the right output. This expression takes in a LivingEntity and returns a list of PotionEffects `PropertyExpression<LivingEntity, PotionEffect>`

The following;
```java
	static {
		register(ExprActionPotionEffects.class, PotionEffect.class, "[all [of]] [the] active potion effects", "livingentities");
		//registerDefault(ExprActionPotionEffects.class, PotionEffect.class, "[all [of]] [the] active potion effects", "livingentities");
	}
```

is a register method provided by the PropertyExpression class. It allows to simplify the process of making the syntax, the first string is the property and the last string is the type, which results in our original syntax we wanted.
The registerDefault method is for allowing the type to be optional. Now you may be wonder how and what that does to our expressions.

Well when we make an expression optional like

```
active potion effects [of %livingentities%]
```

This will be called a default expression. Skript will attempt to grab the entity of the event this syntax was called from. So if this expression was called in an entity combust event; Skript will grab the entity from the event. It does this by grabbing the event value expression you may have registered or Skript already has registered, we'll go over event-values on another tutorial page, but just know this is called a default expression and uses the values present from the event. You may avoid default expressions by notating the expression with a `-`:

```
active potion effects [of %-livingentities%]
```

This lets the expression be null if the value doesn't exist in the event. To do this, you must use Skript.registerExpression though, and you also need to account for the fact that the expression will be null in your init method now.

The following;

```java
	@Override
	@SuppressWarnings("unchecked")
	public boolean init(Expression<?>[] exprs, int matchedPattern, Kleenean isDelayed, ParseResult parseResult) {
		setExpr((Expression<? extends LivingEntity>) exprs[0]);
		return true;
	}
```

Is how we're setting our expression. This is only required for PropertyExpression, SimplePropertyExpression does this for us, which we'll get into at the bottom of this tutorial.

The `setExpr` method sets the expression field of the property expression. This is done because a PropertyExpression should only have 1 expression, you may define more expression fields if you want to make your expression a combined expression by using the PropertyExpression utility class.

We then return true to notate that everything checks out. The casting is also required as it needs to match our output generic.

The following;

```java
	@Override
	protected PotionEffect[] get(Event event, LivingEntity[] source) {
		//return get(source, entity -> entity.getSingleValue());
		return Arrays.stream(source)
				.flatMap(entity -> entity.getActivePotionEffects().stream())
				.toArray(PotionEffect[]::new);
	}
```

Is how we grab our values from the expression we just set. PropertyExpression provides us an array of our details and the event the expression was used in.
Now if this expression wasn't returning multiple values, we can actually use the `get` method as commented out. This is a shortcut method the PropertyExpression class has, that allows us to use lambdas to grab our single value and Skript will then collect it to an array for us.

But because I know this is niche area, I made this example return multiple to better explain complex syntaxes. Here we use a stream and a flatMap to collect all the values down to a single array list.

The following;

```java
	@Override
	public boolean isSingle() {
		return false;
	}
```

Is where we define if this expression return a single value or multiple, and due to how this should operate, we want it to always be false.
You don't need to override this method for PropertyExpressions unless the value is always going to be multiple, as Skript handles this for us by returning `getExpr().isSingle()` which determines if the user is using one or multiple entities.

The following;

```java

	@Override
	@Nullable
	public Class<?>[] acceptChange(ChangeMode mode) {
		switch (mode) {
			case REMOVE:
			case REMOVE_ALL:
				return CollectionUtils.array(PotionEffect[].class, PotionEffectType[].class);
			case ADD:
			case SET:
				return CollectionUtils.array(PotionEffect[].class);
			case DELETE:
				return CollectionUtils.array();
			case RESET:
			default:
				return null;
		}
	}

	@Override
	public void change(Event event, @Nullable Object[] delta, ChangeMode mode) {
		for (LivingEntity entity : getExpr().getArray(event)) {
			switch (mode) {
				case REMOVE_ALL:
				case REMOVE:
					assert delta != null;
					for (Object object : delta) {
						if (object instanceof PotionEffect) {
							entity.removePotionEffect(((PotionEffect) object).getType());
							continue;
						}
						assert object instanceof PotionEffectType;
						entity.removePotionEffect((PotionEffectType) object);
					}
					break;
				case SET:
				case DELETE:
					for (PotionEffect effect : entity.getActivePotionEffects())
						entity.removePotionEffect(effect.getType());
					if (mode != ChangeMode.SET)
						break;
					//$FALL-THROUGH$
				case ADD:
					assert delta != null;
					for (PotionEffect effect : (PotionEffect[]) delta)
						entity.addPotionEffect(effect);
					break;
				case RESET:
				default:
					break;
			}
		}
	}
```

Is called a changer. Every expression that extends a SimpleExpression (Which is majority) can use changers. Changers allows us to add, remove and clear the properties of this expression.

So like we're gonna do here, we want to be able to add, remove and clear potion effects from this expression so the following is possible `add potion effect of regeneration 2 for 5 seconds to the active potion effects of player`

The `acceptChange` method defines what the changer can accept and the types of changers. We return an empty array for the delete, because we don't care the type as it can't be present, and the set has to be a potioneffect due to the Spigot API and the remove can be a potioneffect or a potioneffectype.

Do note that we're returning an array.class here because we want to be able to support adding multiple potion effects at the same time, this also does allow for single addition too.

In our `change` method we use a switch and iterate over all the entities, do note that delta will only be null if the DELETE or RESET changers are used. Also note that in this example, we're using the switch to our advantage by allowing falldowns for the SET mode.

All of the assertions present are for your understanding and are not required.

### SimplePropertyExpression

```java
package me.example.addontutorial.elements.expressions;

import org.bukkit.entity.LivingEntity;
import org.bukkit.event.Event;
import org.eclipse.jdt.annotation.Nullable;

import ch.njol.skript.classes.Changer.ChangeMode;
import ch.njol.skript.doc.Description;
import ch.njol.skript.doc.Examples;
import ch.njol.skript.doc.Name;
import ch.njol.skript.doc.Since;
import ch.njol.skript.expressions.base.SimplePropertyExpression;
import ch.njol.skript.util.Timespan;
import ch.njol.util.coll.CollectionUtils;

@Name("Arrow Cooldown")
@Description("Gets the time until the next arrow leaves the entity's body.")
@Examples("add 5 seconds and 2 ticks to arrow cooldown of player")
@Since("1.0")
public class ExprArrowCooldown extends SimplePropertyExpression<LivingEntity, Timespan> {

	static {
		register(ExprArrowCooldown.class, Timespan.class, "arrow cooldown", "livingentities");
	}

	@Override
	@Nullable
	public Timespan convert(LivingEntity entity) {
		return Timespan.fromTicks_i(entity.getArrowCooldown());
	}

	@Override
	@Nullable
	public Class<?>[] acceptChange(ChangeMode mode) {
		if (mode == ChangeMode.DELETE)
			return null;
		return CollectionUtils.array(Timespan.class);
	}

	@Override
	public void change(Event event, @Nullable Object[] delta, ChangeMode mode) {
		Timespan timespan = null;
		if (mode != ChangeMode.RESET && mode != ChangeMode.DELETE) {
			assert delta != null;
			timespan = (Timespan) delta[0];
			if (timespan == null)
				return;
		}
		long ticks = timespan.getTicks_i();
		for (LivingEntity entity : getExpr().getArray(event)) {
			switch (mode) {
				case REMOVE_ALL:
				case REMOVE:
					entity.setArrowCooldown((int) Math.max(0, entity.getArrowCooldown() - ticks));
					break;
				case SET:
					entity.setArrowCooldown((int) ticks);
					break;
				case ADD:
					entity.setArrowCooldown((int) Math.max(0, entity.getArrowCooldown() + ticks));
					break;
				case RESET:
					entity.setArrowCooldown(40);
					break;
				default:
					break;
			}
		}
	}

	@Override
	public Class<? extends Timespan> getReturnType() {
		return Timespan.class;
	}

	@Override
	protected String getPropertyName() {
		return "arrow cooldown";
	}

}

```

This following is a simple property expression to where we don't need an init method and the get method is simplified even more.

The `getPropertyName` is simply our property that we're registering in our pattern, Skript just needs this for the toString it does itself.

Do note that in this example we're using Skript's timespan instead of directly using the Spigot integer ticks, as users are more familiar with Timespan than doing the time converting themselfs. This utilizes Skript's own types to better improve this expression for users.

Other notes include `if (mode == ChangeMode.DELETE)` where clearing the arrow cooldown does not make sense, because it's a static value on the entity and doesn't change unless you do. So we use RESET here instead which resets to the default 2 seconds that allows an entity to pickup arrows.

The following;

```java
		Timespan timespan = null;
		if (mode != ChangeMode.RESET && mode != ChangeMode.DELETE) {
			assert delta != null;
			timespan = (Timespan) delta[0];
			if (timespan == null)
				return;
		}
```
Is done like this because the value itself can be null, when we use getSingle in any context or on the array it may be null, so if using getSingle be aware that the value may be null. This is why getOptionalSingle is present if it turns out to be null. Do note that `getArray` will return one of the following `diamond, stone or dirt` where as `getAll` will return all the values.

`Timespan.fromTicks_i` exists because `Timespan.fromTicks` is an integer and we need to be using long instead of integer, this is deprecated changes, and will soon only be the long method, so adapt accordingly to the present API as of reading this.

There is also EventValueExpression which we'll go over in the next tutorial and WrapperExpression which is a wrapper of another expression, so for example say we need to modify what an expression is going to do, we can use a wrapper to modify it, example being Skript's own ExprLocation
