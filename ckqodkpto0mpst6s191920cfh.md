## Create and serve your first Nx app

>💡 **Heads up!** You can find the code for every article in this series on GitHub, with a tag per article: https://github.com/trumbitta/giant-robots/tags

## What you'll need

* Your favorite code editor: Visual Studio Code, WebStorm, ...
* Node >= 12 (it might work with earlier versions)
* A terminal

## Let's roll

Open a terminal and run:
```sh
npx create-nx-workspace
```
This will ask you some questions. Here's how you're going to answer them:

```sh
❯ npx create-nx-workspace
npx: installed 48 in 7.875s
✔ Workspace name (e.g., org name)     · giant-robots
✔ What to create in the new workspace · react
✔ Application name                    · frontend
✔ Default stylesheet format           · styled-components
✔ Use Nx Cloud? (It's free and doesn't require registration.) · No
```
Let's not use Nx Cloud right now: that's for a future post. Though it really is awesome and I'm dying here, I just want you to know what it does. 🤩

Anyway, Nx will now go ahead and it will install npm packages, create the actual workspace, and initialize it.

```sh
>  NX  Nx is creating your workspace.

  To make sure the command works reliably in all environments, and that the preset is applied correctly,
  Nx will run "npm install" several times. Please wait.

✔ Installing dependencies with npm
⠦ Creating your workspace
```

## Oh but then the fun begins

Let's jump right in!

![nathan-dumlao-eUbNeGQEh2U-unsplash.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1625349661187/Ixhutu6OI.jpeg)

> Photo by <a href="https://unsplash.com/@nate_dumlao?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Nathan Dumlao</a> on <a href="https://unsplash.com/s/photos/fun?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

* Import the `giant-robots` folder in your editor of choice
* Open a terminal and launch your first React app with Nx:
```sh
cd giant-robots
npm start
```
* Point your browser at http://localhost:4200
![Screen Shot 2021-07-04 at 00.07.45.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625350095886/AJcDtpDA3.png)

## What just happened?

* You ran the same old `npm start`
* If you look into `package.json` you'll see that's defined as `nx serve`
* I didn't make you specify any project so Nx inferred that you wanted to work with the default project aka the only existing project at this moment: `frontend`.  
You'll find this configured at the bottom of `workspace.json`:  
```json
[...]
  "defaultProject": "frontend"
}
```
* `nx serve frontend` is just syntax sugar for the real Nx command:  
```sh
# nx run <project>:<target>
nx run frontend:serve
```

Go ahead and try to serve the app using `nx serve frontend` and then `nx run frontend:serve`! You can hit `ctrl c` to stop the app each time around.

If you need to install the Nx CLI first (you don't need it if the commands above work for you):

```sh
npm i -g @nrwl/cli

# or don't install and use it with npx
npx nx <commands>
```

Then, with the app still running and the browser displaying it:

* Open `apps/frontend/src/app/app.tsx` in your editor
* Change the `<h1>` into `<h1>Welcome to the annual Giant Robot Fair!</h1>` and save
* Go see your browser automatically refreshing and displaying the updated page!

And if you keep the console open in the browser's devtools, you'll notice these messages:
```
[WDS] App hot update...
[HMR] Checking for updates on the server...
[HMR] Updated modules:
[HMR]  - ./app/app.tsx
[HMR] App is up to date.
```
Hot Module Replacement! Nx is awesome and it made Webpack only reload the minimum group of files it needed to reload for taking your change into account.

## Wrapping up

That's a lot to process for your first time with Nx, so I'm calling it a day.  
You just:

* Created your first Nx workspace
* Created your first React app within it
* Learned about the three levels of a Nx command:
  * `npm start`
  * `nx serve frontend`
  * `nx run frontend:serve`
* Ran your React app, made a change, and discovered Nx has HMR enabled to make your development workflow speedier and flawless

🍻 Next time we are going to learn about testing, linting, and building.

> Cover photo by <a href="https://unsplash.com/@bto16180?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Humberto Arellano</a> on <a href="https://unsplash.com/s/photos/creation?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>