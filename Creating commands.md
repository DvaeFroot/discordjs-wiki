---
parent: [[DiscordJS Directories]]
---
# Creating Commands

#### Getting the Guild ID and Client ID
##### Guild ID
To get the ID of a guild, right click on the icon of the guild and select copy id on the drop down menu
![[Pasted image 20220802235908.png]]
```ad-note
To enable this setting, go to your discord settings and enable developer mode
![[Pasted image 20220803000014.png]]
```
We can now paste it into our code
````ad-example
title: index.ts
~~~js
const GUILD_ID = '9108245753219012312'; //This is an altered guildId so don't even try using it
~~~
```ad-note
Although we could also use an environment variable for this, since this is only going to be used for testing locally, this setup should be fine. Just make sure that whatever you are doing is secure.
```
````
%%
This can now be used to get the guild from the client
````ad-example
title: index.ts
~~~js
const guildId = '9108245753219012312';
const guild = client.guilds.cache.get(guildId);
~~~

```ad-note
Since I'm a bit of a dummy I will have to explain to myself what's going on here. The `guildId` is only the id of the `guild` with which we can't do anything with yet. To actually do something in the `guild` we need to get the actual reference to the `guild` with which we can interact with. The `client` or bot will have a `collection` of `guilds` that it is in currently and they will be stored in the `guilds.cache`. The `get()` method of the `guilds.cache` takes an Id for an input and outputs the reference to the `guild` which allows us to interact with it.
```
````
%%
##### Client ID
To get the client id, go to your discord developer portal and go to your discord bot's general information tab. Your client id is located in the Application ID section. 
![[Pasted image 20220803151210.png]]
Copy and paste it onto your code.
```ad-example
title: index.ts
~~~js
const CLIENT_ID = '1000007910308459292'; //This is also altered
~~~
```

#### Creating the commands
```ad-info
Previously, commands were invoked via a messsage sent to a channel. The message is then parsed and if it matches the conditions of executing a command, then the command is executed. Previously you would always need some form of parser for every command and is often non-standardized. With Slash commands, this is no longer the case. Slash Commands allow the user to be able to invoke commands by typing a forward slash and the command name as well as choosing the appropriate bot. Not only that, the user can also see the list of all commands for a specific bot when typing a forward slash, eliminating the need to call the list of commands manually. There are also autocomplete features that work much better than memorizing a list of preconfigured commands. There are many more benefits to slash commands than the traditional commands and as of DiscordJS v13, slash command is the primary type of command that is commonly used for most bots.
```
######  Package imports
Before we can make and register any commands we need to import two packages: `REST` and `Routes`
````ad-example
title: index.ts
~~~js
const { REST } = require('@discordjs/rest');
const { Routes } = require('discord.js');
~~~
```ad-info
The `REST` package allows us to interact with the discord API directly without using any other HTTP packages like axios. 

The `Routes` allow us to get the route to an API by giving it some parameters instead of manually entering the URI.
```
````
Next we need to create an instance of a REST object which we can then use to interact with the discord API.
````ad-example
title: index.ts
~~~js
const rest = new REST({ version: '10'}).setToken(process.env.TOKEN);
~~~
```ad-note
I'm not exactly sure how much each version differs but version 10 is recommended by guides so that's what we are using as well.
```
````

Before we can start creating commands, we must first identify what type of command we are creating. There are 2 types of slash commands that we need to be aware of:
1. Guild-based 
	- Registered instantly
	- Highly encouraged to use when developing or testing slash commands as it can be specified to only register to one test guild which will also register instantly
2. Global-based
	- Registers to every guild that the bot is in which may take a long time even up to hours before being visible
###### Creating commands
Now we can start creating slash commands. To create a command, we must give it a name and a description.
````ad-example
title: index.ts
~~~js
const commands = [
	{
		name: 'commandname'
		description: 'command description here'
	}
];
~~~
```ad-info
The `commands` variable is an array containing all of our commands. Each command is represented by an object in the array. This makes it easier to pass the commands to the body of our HTTP request to the discord API.
```
````
###### Registering commands
Now that we have created slash commands, we can now register them so the client knows our commands.
````ad-example
title: index.ts
~~~js
( async () => {
	const commands = [
		{
			name: 'commandname'
			description: 'command description here'
		}
	];
	try {
		console.log('Started refreshing application (/) commands.');
		await rest.put(Routes.applicationGuildCommands(CLIENT_ID, GUILD_ID), {
			body: commands,
		});
	} catch (err) {
		console.log(err);
	};
})();
~~~
```ad-info
Now there seems to be a lot of things going on here in this block of code so let's break it down.
~~~js
( async () => {
	//Write block of code here
})()
~~~
This block of code allows us to write an anonymous function and execute immediately. Think of it as invoking a function like `functionName()` but `functionName` is replaced by the block of code that represents the function. You are declaring a function and then executing right after.

~~~js
async () => {
	//Write block of code here
}
~~~
The `async` keyword allows the block of code inside the function to be able to run asynchronous functions. HTTP requests by design are asynchronous since there will always be delays sending and receiving requests. This is important as asynchronous functions will not run without this keyword.

~~~js
async () => {
	try {
		await rest.put(Routes.applicationGuildCommands(CLIENT_ID, GUILD_ID), { body: commands})
	} catch(err) {
		console.log(err)
	}
}
~~~
The `try-catch` blocks allow us to catch errors without necessarily crashing the application. The `try` block will always run unless it runs into an error in which case the `catch` block runs with the error as its parameter. We use `try-catch` blocks with asynchronous functions because they are prone to errors.

The `await` keyword lets the program wait for the function to finish before proceeding to the next line of code.

`rest` is the `REST` instance declared earlier which allows us to interact with the discord API. We use the 'PUT' HTTP request via the rest.put() method which allows us to update the API with our bot. Since it's also a 'PUT' HTTP request, we can send it a body which the backend can then use to process.

The `Routes.applicationGuildCommands()` is used if we want to register commands to a specific guild. It takes in 2 parameters which are the client ID and the guild ID and returns the URI with the specified parameters. 

There are also other routes that you can use but I can't seem to find the documentation for it though so for now you're just going to have to rely on the autocomplete feature of your IDE of choice.
```
````
```ad-note
More info about registering commands can be found [here](https://discord.com/developers/docs/interactions/application-commands).
```
#### Results
If you followed everything then your code should somewhat look like this
````ad-example
title: index.ts
~~~js
const { REST } = require('@discordjs/rest');
const { Client, GatewayIntentBits, Routes } = require('discord.js');

const TOKEN = process.env.TOKEN;
const CLIENT_ID = process.env.CLIENT_ID;
const GUILD_ID = process.env.GUILD_ID;

const client = new Client({
	intent: [
		GatewayIntentBits.Guilds,
		GatewayIntentBits.GuildMessages,
	]
});

const rest = new REST({ version: '10'}).setToken(TOKEN);

( async () => {
	const commands = [
		{
			name: "commandName",
			description: "This is the command description"
		},
	];

	rest.put(Routes.applicationGuildCommands(CLIENT_ID, GUILD_ID), {
		body: commands,
	});
})();

client.login(TOKEN);
~~~
```ad-note
I'm sticking back to CommonJS as it is more flexible and is the method commonly used in the official documentation.
```
````
If we start the bot, we can now see our new Slash command popping up when we type of a forward slash in the discord channel. 
#### Recap
From this tutorial, we have learned how to import the required packages for registering commands. We also learned how to create commands and then register them using the packages we imported.

And that's it, we have now successfully created and registered a Slash command. But we haven't given it any behaviour yet. The bot recognizes the Slash command but won't do anything with it yet. In the next guide we'll be looking at how we can give behavior to our Slash commands.
#### References
- [Discord.js v14 - Register Slash Commands](https://www.youtube.com/watch?v=ZtDtjjUHbIg&list=PL_cUvD4qzbkwA7WITceoc2_FFjQsBkwX7&index=4)