# Awesome Documentation Of DevOps Project-3 : MERN STACK IMPLEMENTATION

## STEP 1 – Backend Configuration 

Update and upgrade Ubuntu

`sudo apt update`

`sudo apt upgrade`

Lets get the location of Node.js software from Ubuntu repositories.

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

Install Node.js with the command below

`sudo apt-get install -y nodejs`

Note: The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.
Verify the node installation with the command below

`node -v `

Verify the npm installation with the command below

`npm -v`

Application Code Setup
Create a new directory for your To-Do project:

`mkdir Todo`

Run the command below to verify that the Todo directory is created with ls command

`ls`

TIP: In order to see some more useful information about files and directories, you can use following combination of keys ls -lih – it will show you different properties and size in human readable format. You can learn more about different useful keys for ls command with ls --help.

Now change your current directory to the newly created one:

`cd Todo`

Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

`npm init`

![todo init-image](https://user-images.githubusercontent.com/104405639/170556034-1f206121-b78a-423b-ab40-adc25e60d09e.png)

Run the command ls to confirm that you have package.json file created.
Next, we will Install ExpressJs and create the Routes directory.

## INSTALL EXPRESSJS

Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.
To use express, install it using npm:

`npm install express`

Now create a file index.js with the command below

`touch index.js`

Run ls to confirm that your index.js file is successfully created
Install the dotenv module

`npm install dotenv`

Open the index.js file with the command below

`vim index.js`

Type the code below into it and save. Do not get overwhelmed by the code you see. For now, simply paste the code into the file.

`const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});`

`code`

`code`

`code`
