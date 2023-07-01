# Starting our addon (Main class)

Class named AddonTutorial.java;

```java
package me.example.addontutorial;

import java.io.IOException;

import org.bstats.bukkit.Metrics;
import org.bstats.charts.SimplePie;
import org.bukkit.plugin.java.JavaPlugin;

import ch.njol.skript.Skript;
import ch.njol.skript.SkriptAddon;

public class AddonTutorial extends JavaPlugin {

	private static AddonTutorial instance;
	private SkriptAddon addon;

	public void onEnable() {
//		if (!Skript.isRunningMinecraft(1, 13)) {
//			getPluginLoader().disablePlugin(this);
//			getLogger().info("AddonTutorial only works on 1.13+");
//			return;
//		}
		instance = this;
    try {
      addon = Skript.registerAddon(this)
          .loadClasses("me.example.addontutorial", "elements")
          .setLanguageFileDirectory("lang");
    } catch (IOException e) {
      e.printStackTrace();
    }

		// Saves the raw contents of the default config.yml file to the locationretrievable by getConfig(). 
		saveDefaultConfig();
		if (!getDescription().getVersion().equalsIgnoreCase(getConfig().getString("version")))
			getLogger().info("There is a new configuration version! Please save your data and delete your config.yml to allow it to regenerate.");

		// Replace 1234 with your bStats plugin ID.
//		Metrics metrics = new Metrics(this, 1234);
//		metrics.addCustomChart(new SimplePie("example", () -> "some string"));
		getLogger().info("AddonTutorial has been enabled!");
	}

	public static AddonTutorial getInstance() {
		return instance;
	}

	public SkriptAddon getAddonInstance() {
		return addon;
	}

}
```

The following will be our main class. The following;

```java
//		if (!Skript.isRunningMinecraft(1, 13)) {
//			getPluginLoader().disablePlugin(this);
//			getLogger().info("AddonTutorial only works on 1.13+");
//			return;
//		}
```
Is commented out, as not everyone may want to support a specific version, but this is how you can it with Skript.

The following;

```java
try {
  addon = Skript.registerAddon(this)
      .loadClasses("me.example.addontutorial", "elements")
      .setLanguageFileDirectory("lang");
} catch (IOException e) {
  e.printStackTrace();
}
```

Is how we register our addon syntaxes to Skript. This will read all syntaxes within the `me.example.addontutorial.elements.*` packages. We'll go over each syntax and it's registers in the next sections.
The `.setLanguageFileDirectory("lang");` method is as formentioned, it'll search the lang/ folder for all files that end with `.lang` which will be how Skript picks the language to use, or `default.lang` for default.
We'll go over this file in the future with classinfos, but for now it's best to create a blank `default.lang` file inside this folder. You can insert `version: @version@` as mentioned before this will be replaced with your version.

The metrics is bStats API, you can read up on their documentation for more information on this, but we're essentially registering the plugin with it's bStats ID which is given on their site (bstats.org). Metrics is totally optional.

`getInstance` is the only static methods as we only need `AddonTutorial#getInstance()` to be static, so we can grab this instance else where, and our config with `AddonTutorial#getInstance()#getConfig()` or any other details from inside this class.
The less static the better off your addon will be. Static is a very common issue for security which is why for example Kotlin trys to eliminate it from the language, for critical security and crashing potential applications like Android.

Now to point our plugin.yml to this main class

```yaml
name: AddonTutorial
authors: [SkriptLang, Njol]
description: An example Skript addon.
api-version: 1.13
version: @version@
main: me.example.addontutorial.AddonTutorial
depend: [Skript]

```
Ensure you have the depend tag in there. Also line break at the end, as GitHub doesn't like no line break at the end. (It's an old programming standard to signify the end of the file, but modern programming languages know the end of the file now).

Also inside our config.yml is just; (You can add your own nodes if you need a config.yml otherwise you don't need all the config code in this main class example.)
```
version: @version@

```

Now that's it, our addon is now registered to Skript and we can start making syntaxes in the next tutorial section.



