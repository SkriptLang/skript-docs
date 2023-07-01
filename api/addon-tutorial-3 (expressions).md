# Creating an Expression

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
	"set {_variable} to our example"
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
