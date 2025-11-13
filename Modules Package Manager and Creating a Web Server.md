# Modules, Package Manager, and Creating a Web Server

Welcome to this beginner-friendly, step-by-step tutorial on Node.js! Node.js is a runtime environment that allows you to run JavaScript on the server side. It's great for building web servers, APIs, and more. We'll cover three main topics: Node.js modules, the package manager (npm), and creating a simple web server.

This tutorial assumes you have basic knowledge of JavaScript. If you're completely new, consider learning JS basics first. Let's get started!

## Step 1: Installing Node.js
Before anything, you need Node.js installed on your computer.

1. Go to the official Node.js website: [nodejs.org](https://nodejs.org).
2. Download the LTS (Long Term Support) version for stability—it's recommended for beginners.
3. Run the installer and follow the prompts. This will install Node.js and npm (Node Package Manager) automatically.
4. Verify the installation by opening your terminal (Command Prompt on Windows, Terminal on macOS/Linux) and running:
   ```
   node -v
   ```
   This should print the Node.js version (e.g., v20.x.x).
   
   Then check npm:
   ```
   npm -v
   ```
   It should print something like 10.x.x.

If you encounter issues, search for "install Node.js on [your OS]" for detailed guides.

## Step 2: Understanding Node.js Modules
Modules are reusable pieces of code in Node.js. They help organize your code and avoid repetition. Node.js has two types: built-in (like `fs` for file system) and custom (ones you create).

### 2.1: Using Built-in Modules
Node.js comes with many pre-built modules. No installation needed—just require them.

1. Create a new folder for your project, e.g., `node-tutorial`.
2. Inside it, create a file called `app.js`.
3. Open `app.js` in a text editor (like VS Code) and add this code to use the `fs` module for reading/writing files:
   ```javascript
   const fs = require('fs');  // Require the built-in 'fs' module

   // Write to a file
   fs.writeFileSync('hello.txt', 'Hello from Node.js!');

   // Read from the file
   const data = fs.readFileSync('hello.txt', 'utf8');
   console.log(data);  // Outputs: Hello from Node.js!
   ```
4. Run it in your terminal:
   ```
   cd node-tutorial
   node app.js
   ```
   You'll see the output, and a `hello.txt` file will appear in your folder.

### 2.2: Creating Custom Modules
You can make your own modules to split code.

1. In the same folder, create a file called `math.js` (your custom module):
   ```javascript
   function add(a, b) {
       return a + b;
   }

   function subtract(a, b) {
       return a - b;
   }

   module.exports = { add, subtract };  // Export the functions
   ```
2. Update `app.js` to use your custom module:
   ```javascript
   const math = require('./math');  // Require your custom module (note the './' for local files)

   console.log(math.add(5, 3));      // Outputs: 8
   console.log(math.subtract(5, 3)); // Outputs: 2
   ```
3. Run `node app.js` again. It should work!

Tip: `require()` loads modules synchronously. For modern JS, you can use ES modules with `import/export` by adding `"type": "module"` to your `package.json` (we'll cover that soon).

## Step 3: Using the Package Manager (npm)
npm is Node.js's package manager. It lets you install third-party libraries (packages) and manage your project's dependencies.

### 3.1: Initializing a Project
1. In your `node-tutorial` folder, run:
   ```
   npm init -y
   ```
   This creates a `package.json` file with default settings. It's like a manifest for your project, tracking dependencies and scripts.

   Your `package.json` will look something like:
   ```json
   {
     "name": "node-tutorial",
     "version": "1.0.0",
     "description": "",
     "main": "app.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "keywords": [],
     "author": "",
     "license": "ISC"
   }
   ```

### 3.2: Installing Packages
Let's install a popular package like `lodash` (for utility functions).

1. Run:
   ```
   npm install lodash
   ```
   - This downloads `lodash` and adds it to `node_modules/` folder.
   - It also updates `package.json` with a new "dependencies" section:
     ```json
     "dependencies": {
       "lodash": "^4.17.21"
     }
     ```
   - A `package-lock.json` file is created to lock versions for consistency.

2. Use it in `app.js`:
   ```javascript
   const _ = require('lodash');

   const numbers = [1, 2, 3, 4];
   console.log(_.chunk(numbers, 2));  // Outputs: [ [1, 2], [3, 4] ]
   ```
3. Run `node app.js`.

### 3.3: Managing Packages
- Update a package: `npm update lodash`
- Uninstall: `npm uninstall lodash`
- Install globally (for tools like CLI apps): `npm install -g nodemon` (nodemon restarts your server on file changes—great for development).
- To share your project: Share the folder without `node_modules/`. Others can run `npm install` to get dependencies from `package.json`.

Tip: Use `npm start` by adding a script to `package.json`:
```json
"scripts": {
  "start": "node app.js"
}
```
Then run `npm start`.

## Step 4: Creating a Web Server
Node.js excels at building servers. We'll use the built-in `http` module first, then touch on Express.js for something more robust.

### 4.1: Basic HTTP Server
1. Update `app.js`:
   ```javascript
   const http = require('http');

   // Create a server
   const server = http.createServer((req, res) => {
       res.statusCode = 200;  // OK status
       res.setHeader('Content-Type', 'text/plain');
       res.end('Hello, World! This is my first Node.js server.');
   });

   // Listen on port 3000
   server.listen(3000, '127.0.0.1', () => {
       console.log('Server running at http://127.0.0.1:3000/');
   });
   ```
2. Run `node app.js`.
3. Open your browser and go to `http://localhost:3000`. You should see "Hello, World! This is my first Node.js server."

   - `req` is the request object (incoming data).
   - `res` is the response object (what you send back).
   - Press Ctrl+C in terminal to stop the server.

### 4.2: Handling Routes
Add simple routing to respond differently based on URL.

Update the server callback in `app.js`:
```javascript
const server = http.createServer((req, res) => {
    if (req.url === '/') {
        res.end('Home Page');
    } else if (req.url === '/about') {
        res.end('About Page');
    } else {
        res.statusCode = 404;
        res.end('Page Not Found');
    }
});
```
Run it and visit `http://localhost:3000/about` or `http://localhost:3000/random` to see.

### 4.3: Introducing Express.js (Optional Next Step)
For real apps, use Express.js—it's a minimal framework that simplifies routing, middleware, etc.

1. Install: `npm install express`
2. Create `server.js`:
   ```javascript
   const express = require('express');
   const app = express();

   app.get('/', (req, res) => {
       res.send('Home Page');
   });

   app.get('/about', (req, res) => {
       res.send('About Page');
   });

   app.all('*', (req, res) => {  // Catch-all for 404
       res.status(404).send('Page Not Found');
   });

   app.listen(3000, () => {
       console.log('Server running at http://localhost:3000/');
   });
   ```
3. Run `node server.js` and test in browser.

Express makes things easier—add middleware for JSON parsing, static files, etc., as you advance.

## Final Tips
- Practice by building small projects, like a todo API.
- Debug with `console.log()` or tools like Node Inspector.
- Learn more from official docs: [nodejs.org/docs](https://nodejs.org/docs) and [expressjs.com](https://expressjs.com).
- Common errors: Check ports (3000 might be in use), file paths, and dependencies.

