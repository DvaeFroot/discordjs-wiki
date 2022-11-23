---
parent: [[Directories]]
---
# Initial Files
#### 1. .env
Create a file in the root folder named `.env`.
```ad-info
This is where our tokens and other special values will be stored. This can only be utilized with the dotenv package installed earlier.
```
```ad-warning
This should never be shared or committed to version control other than your local machine. This file contains information that will allow other people to gain access to your bot and may make unwanted changes.
```
Open the 📄`.env` file and insert your bot's token like below. 
````ad-example
title: .env
TOKEN=MY_SPECIAL_TOKEN
```ad-info
Environment variable syntax is just like writing in regular programming: VARIABLE_NAME=VALUE. The TOKEN variable can be renamed to something else if you'd like. You can also add more variables.
```
````

#### 2. index.js or index.ts
Create a file in the root folder named `index.js` (`index.ts` if typescript).
```ad-info
This is the file which will contain our driver code where our main code will be run.
```
Inside the 📃`index.ts` file, import the following.
````ad-example
title: index.ts
~~~js
import DiscordJS from 'discord.js'
import dotenv from 'dotenv'
~~~
```ad-note
Since the [tutorial](https://www.youtube.com/watch?v=JMmUW4d3Noc&list=PLaxxQQak6D_f4Z5DtQo0b1McgjLVHmE8Q&t=681s) I was following was using module to import, I will also be using module for this guide.
```
````
To use the environment variables from the 📄`.env` file, the config method must be called from the imported dotenv.
```ad-example
title: index.ts
~~~js
dotenv.config()
~~~
```
Now that we've imported our essential packages, it's time to create the client which is what we refer to as our discord bot or simply just bot.
````ad-example
title: index.ts
~~~js
const client = new DiscordJS.Client({
	intents: [
		DiscordJS.GatewayIntentBits.Guilds,
		DiscordJS.GatewayIntentBits.GuildMessages]
})
~~~
```ad-note
The [tutorial](https://youtu.be/JMmUW4d3Noc?list=PLaxxQQak6D_f4Z5DtQo0b1McgjLVHmE8Q&t=758) I was following was using DiscordJS v13 so the one in this guide might look different because it's been updated for DiscordJS v14.
```
````ad-info
The intents should include the list of information that we want the bot to receive or listen to. In this case, we have allowed our bot to receive information from the guild and messages sent in the guild.

It's often recommended to just give the bot access to all the intents but according to [Worn Off Keys](https://youtu.be/JMmUW4d3Noc?list=PLaxxQQak6D_f4Z5DtQo0b1McgjLVHmE8Q&t=681), this is bad practice as it will eat up most of your bots bandwidth.
```ad-note
DiscordJS refers to Discord servers as Guilds.
```
```ad-bug
For a list of intents, refer to [this stackoverflow post](https://stackoverflow.com/questions/73027011/guild-intent-throwing-error-discord-js/73027274#73027274) or the [official DiscordJS guide](https://discordjs.guide/popular-topics/intents.html#privileged-intents) or the [Discord API](https://discord.com/developers/docs/topics/gateway).
```
````
Now we need to listen for when the client starts up or when the bot goes online. Events are further explained [[Event Handling | here]].
```ad-example
title: index.ts
~~~js
client.once('ready', () => {
	console.log('Ready!')
})
~~~
```
And finally we can log the client or bot
````ad-example
title: index.ts
~~~js
client.login(process.env.TOKEN)
~~~
```ad-warning
The environment variable must match the name declared in the 📃`.env` file which in this case is TOKEN
```
````
```ad-warning
For upgrading from discord.js v13 to v14, you may refer to [this post](https://stackoverflow.com/questions/73028854/discord-js-v13-code-breaks-when-upgrading-to-v14) from stackoverflow for some possible errors and fixes.

```
##### Resulting Code
If you followed the steps exacly, your 📄`index.ts` should be like the one below
```ad-example
title: index.ts
~~~js
import DiscordJS from 'discord.js'
import dotenv from 'dotenv'
dotenv.config()

const client = new DiscordJS.Client({
	intents: [
		DiscordJS.GatewayIntentBits.Guilds,
		DiscordJS.GatewayIntentBits.GuildMessages
	]
})

client.once('ready', ()=>{
	console.log('Ready!')
})

client.login(process.env.TOKEN)
~~~
```
Alternatively for CommonJS
````ad-example
title: index.ts
~~~js
const { Client, GatewayIntentBits } = require('discord.js')
require('dotenv').config()

const client = new Client({
	intents: [
		GatewayIntentBits.Guilds,
		GatewayIntentBits.GuildMessages
	]
})

client.once('ready', ()=>{
	console.log('Ready!')
})

client.login(process.env.TOKEN)
~~~
```ad-info
The line of code below is implementing a method known as Object Destructuring. It is a method to quickly access the variables that we need from an object. In this case, our object is what we receive from `require('discord.js')`. 
~~~js
const { Client, GatewayIntentBits } = require('discord.js')
~~~
We can also write the same line of code like below and it will also give the same output.
~~~js
const DiscordJS = require('discord.js')
const Client = DiscordJS.Client
const GatewayIntentBits = DiscordJS.GatewayIntentBits
~~~
But notice how much more concise Object Destructuring is rather than the traditional method.

It is also important that the variable names in our Object Destructuring is the same as what is in the actual object. Notice in the latter example that `DiscordJS` has a value of `Client` as well as `GatewayIntentBits`. These need to be the same in our Object Destructuring.
```
````
Now we can manually run the bot by running the appropriate command in the terminal
```
ts-node index.ts
```
```
node index.js
```
or call our script from 📃`package.json` file
```
npm run dev
```