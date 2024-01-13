# deploy_reactApp-expressJs_in_netlify--vercel

In this repository, I will guide you through deploying a React app on Netlify and an Express.js API connected to the frontend on Vercel.

<hr/>

## react on netilfy :
After you finish building the react project , you can deploy with simple commands

**note : you must have account in netlify**


- first install npm netlify command line globally   ```install netlify-cli -g```
- creates a build directory with a production build of your app ``` npm run build ```
- deploy the application ``` netlify deploy ```
  - and give the publish directory ```(e.g. "public" or "dist" or "."):```  if you are from the React development bootcamp, choose ``` dist ```
- If everything looks good , deploy it to your main site URL ```netlify deploy --prod```

you can check the docs for more info : https://docs.netlify.com/integrations/frameworks/react/

##  expressjs api and connect with front-end  on vercel

Steps:
- Create Express API with NODE
- Push the server folder to a GitHub repo
- Connect your Vercel account with that Github repo.
- Update your repo locally then watch changes deploy immediately.


the project structure it should be similar like this :

```
 ┣ 📂api
 ┃ ┗ 📜index.ts
 ┣ 📂public
 ┃ ┗ 📂images
 ┣ 📂src
 ┃ ┣ 📂config
 ┃ ┣ 📂controllers
 ┃ ┣ 📂models
 ┃ ┣ 📂routers
 ┣ 📜index.d.ts
 ┣ 📜package-lock.json
 ┣ 📜package.json
 ┣ 📜README.md
 ┣ 📜tsconfig.json
 ┣ 📜vercel.json
```
1- create vercel.json :
```
{
    "version": 2,
    "builds": [
      {
        "src": "api/index.ts",
        "use": "@vercel/node"
      }
    ],
    "routes": [
      {
        "src": "/(.*)",
        "dest": "api/index.ts",
        "methods": ["GET", "POST", "PUT", "DELETE", "PATCH", "OPTIONS"],
        "headers": {
          "Access-Control-Allow-Origin": "Your frontend URL (e.g., https://**********.netlify.app)",
          "Access-Control-Allow-Credentials":"true",
          "Access-Control-Allow-Headers":"Origin, X-Requested-With, Content-Type, Accept"
        }
      }
    ]
  }
```

2- install ``` npm install cors ``` and in api/index.ts add : 

```
import { connectDB } from '../src/config/db'

export const app: Application = express()
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', "Your frontend URL (e.g., https://**********.netlify.app)");
  res.header('Access-Control-Allow-Credentials', 'true');
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
  
  connectDB() // connect mongo database
  next();
});
app.use(cors({
  origin:'Your frontend URL (e.g., https://**********.netlify.app)',
  credentials:true
}));

```

