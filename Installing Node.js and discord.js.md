---
parent: [[DiscordJS Directories]]
---
# Installing Node.js and discord.js
#### 1. package.json
Run the following command in the terminal within the root directory of your project.
```
npm init -y
```
This will create a package.json file. 
```ad-warning
It's important that you create the package.json file first because all dependencies will be appended to it.
```
#### 2. NodeJS 
NodeJS can be downloaded from this [link](https://nodejs.org/en/)
```ad-tip
Although not required, it might be a good idea to use a versioning manager for NodeJS incase there are versioning issues. NVM is one such tool. You can follow the installation process via this [link](https://github.com/nvm-sh/nvm/blob/master/README.md)
```

 ```ad-warning 
DiscordJS v14 requires **AT LEAST** Node version **16.9.X**
 ```
#### 3. discord.js
To install discord.js, run the following appropriate command in the terminal within the root directory of your project.
```
npm install discord.js
```
```
yarn add discord.js
```
#### 4. dotenv
To install dotenv, run the following appropriate command in the terminal within the root directory of your project.
```
npm install dotenv
```
```
yarn add dotenv
```
```ad-note
This will allow us to use a .env file to store tokens. This is a secure method to prevent tokens from leaking since tokens are the identifyer for our bots. If other people get a hold of your bot's token, they can make unwanted changes to it, or for worse, completely destroy it.
```
#### 5. @discordjs/rest
To install @discordjs/rest, run the appropriate command in the terminal within the root directory of your project
```
npm install @discordjs/rest
```
```
yarn add @discordjs/rest
```
#### 6. discord-api-types
To install `discord-api-types`, run the appropriate command in the terminal within the root directory.
```
npm install discord-api-types
```
```
yarn add discord-api-types
```
#### 7. typescript (optional) 
To install typescript, run the following command in the terminal. This will be set as global so it can be used in any other directory.
```
npm install -g typescript ts-node
```
```ad-warning
If there are issues while installing typescript, try running the terminal as administrator. If using VSCode, run VSCode as administrator.

The issue occurs because we are trying install something globally which would generally require adminstrator permission
```
To initialize a typescript project, run the following command in the terminal within the root directory of your project
```
tsc -init
```
This will create a 📃`tsconfig.json` (typescript config file)
```ad-bug
In case your terminal doesn't recognize 📃`tsc` as a command try this instead
~~~
npx --package typescript tsc --init
~~~
You can follow this [link](https://bobbyhadz.com/blog/typescript-tsc-command-not-found) for further instructions if it doesn't work.
```
#### 8. nodemon (optional but highly recommended)
To install nodemon, run the appropriate command in the terminal
```
npm install nodemon
```
```
yarn add nodemon
```
Insert the following to the 📃`package.json` file. There should be a "scripts" key, you can simply replace it.
```json
"scripts":{
	"dev": "nodemon index.ts" //or index.js
}
```

```ad-info
nodemon will automatically refresh our 📃`index.js` file whenever new changes are applied. You can see how much time this will save us for implementing minute changes to our discord bot.
```
## Result
If you followed everything correctly, your directory should look like this
> - 📂 node_modules
> - 📄 package-lock.json
> - 📄 package.json
> - 📄 tsconfig.json

---
Next -> [[Setting up a bot application]]