# UI Development Cheat Sheet

## Toolset
### Content Management Systems (CMS)
* [Kirby](https://getkirby.com/) – A flexible, file-based CMS that doesn't require a database.
* [Webflow](https://webflow.com/) – A visual development platform used to design, build, and launch responsive websites.

### Frontend Frameworks & Libraries
* [Preact](https://preactjs.com/) – A fast, 3kB alternative to React with the same modern API.
* [Next.js](https://nextjs.org/) – A React framework for building full-stack web applications with optimized routing and rendering.
* [Tailwind CSS](https://tailwindcss.com/) – A utility-first CSS framework for rapid UI styling.

### Data Visualization & Animation
* [D3.js](https://d3js.org/) – A JavaScript library for manipulating documents based on data to create dynamic data visualizations.
* [GSAP](https://gsap.com/) – A robust JavaScript animation library built for high-performance UI animations.

### Backend & Data Layer

* [Supabase](https://supabase.com/) – An open-source Firebase alternative providing a Postgres database, authentication, and instant APIs.
* [Prisma](https://www.prisma.io/) – A next-generation Node.js and TypeScript ORM used to interact cleanly with databases.

### Hosting & Deployment Platforms
* [Vercel](https://vercel.com/) – A cloud platform for frontend developers, optimized for hosting Next.js and static frameworks.

### Build Tools & Developer Operations
* [Vite](https://vite.dev/) – A frontend build tool that is extremely fast, leveraging native ES modules for bundling.

### Real-time Infrastructure & Third-Party APIs
* [Pusher](https://pusher.com/) – A hosted API service that makes it easy to add real-time bi-directional functionality (like WebSockets) to UI apps.
* [Allium API](https://www.allium.so/) – An enterprise blockchain data provider offering real-time APIs for crypto/web3 UI projects.

## JavaScript
* [url to main cheat sheet with examples](https://github.com/cherkavi/javascripting)
* [In browser vector graphics](d3-cheat-sheet.md)
* [from JS to any language with single codebase](https://github.com/aws/jsii)

## Typescript cheat sheet
### start new typescript application
```sh
### create folder
mkdir sandbox-typescript
cd sandbox-typescript

# init npm 
npm init

# sudo apt install node-typescript
tsc --init
# {
#   "compilerOptions": {
#     "target": "es6",
#     "module": "commonjs",
#     "esModuleInterop": true,
#     "moduleResolution": "node",
#     "sourceMap": true,
#     "noImplicitAny": false,
#     "outDir": "dist"
#   },
#   "lib": ["es2015"]
# }
vim tsconfig.json


### install dependencies
## typescript should have the same version `tsc --version` - like 3.8.3 
npm install -D typescript
npm install -D tslint
## local server 
npm install -S express
npm install -D @types/express
## custom libraries 
npm install -D json-bigint
npm install -D bignumber.js

# tslint init
./node_modules/.bin/tslint --init

# "rules": {"no-console": false},
vim tslint.json

# "start": { "tsc && node dist/app.js", ...
vim package.json

### application start point
# import express from 'express';
# const app = express();
# const port = 3000;
# app.get('/', (req, res) => {res.send('up and running');});
# app.listen(port, () => {console.error(`server started on port ${port}`);});

mkdir src
vim src/app.ts

# start app
npm start
```

## Local Tools
### Terminal sh/bash replacement
```sh
npm install -g bun
```

```javascript
import { $ } from "bun";

const output = await $`ls *.js`.arrayBuffer();
```

### JS Obfuscator
```sh
npm install --save-dev javascript-obfuscator

javascript-obfuscator input_file_name.js [options]
javascript-obfuscator input_file_name.js --output output_file_name.js [options]
```
