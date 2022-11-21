---
parent: [[DiscordJS Directories]]
---
# Event Handling
Within a guild, there are various events happening at any time whether that be a command, a chat message, a member joining, a ping, or etc. We can make our client listen to any of those events and make it do something whenever the event takes place. The full list of events can be found [here](https://discord.js.org/#/docs/main/stable/class/Client).
## Listening to events
### client.once() and client.on()
You may have noticed that we've been making our bot listen to events via the `client.on()` method as well as the `client.once()` method. 
- `client.on()` listens to an event and responds whenever an event is triggered.
- `client.once()` only responds once even when an event is triggered again.

These are callback methods that take in two parameters: `("eventName", ()=>{})` the event name and a function. 
### Common Events
#### 'ready'
The `ready` event runs whenever the client is online and logged in for the first time. This is useful for initializing other events, commands, and APIs.
```js
client.once('ready', () => {
	console.log(`${client.user.tag} has logged in!`)
})
```
#### 'interactionCreate'
The `interactionCreate` event runs whenever the user uses an application command such as Slash commands or a message component. 
```js
client.on('interactionCreate', (interaction) => {
	if (!interaction.isChatInputCommand()) return;

	const { commandName } = interaction;

	if (commandName === 'ping') {
		await interaction.reply('Pong!');
	} else if (commandName === 'server') {
		await interaction.reply('Server info.');
	} else if (commandName === 'user') {
		await interaction.reply('User info.');
	}
})
```
```ad-info
The `interaction` is the message that the bot receives. With the interaction, we can also interact with it, in this case we replied to the interaction with the client.

There are also other types of interactions. In this case we are checking for a chat input command. 

More information about interactions can be read [here](https://discord.com/developers/docs/interactions/receiving-and-responding)
```
```ad-note
The line of code below is what's known as a guard clause, where instead of checking if the interaction is a chat input command we are checking if the interaction is otherwise. If it isn't then we simply return and end the function there. This allows us to write cleaner and more readable code by not enclosing our running code within a parenthesis. This is a healthy programming pattern and is encouraged to be used whenever possible.
~~~js
if (!interaction.isChatInputCommand()) return;
~~~
```