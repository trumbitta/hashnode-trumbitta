## Why is Nx so powerful and different?

>💡 **Heads up!** You can find the code for every article in this series on GitHub, with a tag per article: https://github.com/trumbitta/giant-robots/tags

If you are following  [this series about Nx](https://trumbitta.hashnode.dev/series/nx), you learned among other things how to share code between apps and libs.

Now you are going to learn how Nx keeps track of the relationships between those apps and libs, and which amazing features and tools that enables.

## The dependency graph

As always, if you want to follow along (you should!) with the examples, you'll need a workspace at least similar to the one we are progressively building throughout this series.  
At the very minimum, you'll need one library and two apps that both import something from that library.

Let's go! Open a terminal at the root of your workspace and run:

```sh
nx dep-graph --watch
```

And if you use the "Select All" button you should see something like this:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626383793246/KmTrwmX7o.png)

As you can see, every relationship is displayed: if a project (app or lib) imports something from a lib, you'll see a simple arrow, while implicit dependencies are marked with the word "implicit".  
When project A doesn't import anything from project B, but you know they need to be related, you'll set an implicit dependency in `nx.json`:

```json
[...]
    "frontend-e2e": {
      "tags": [],
      "implicitDependencies": ["frontend"]
    },
[...]
```

Nx automatically set the implicit dependency between `frontend-e2e` and `frontend`  for you when you created the `frontend` app.

Now keep the dependency graph running, open a new terminal, and run:

```sh
nx generate @nrwl/react:library --name=robots --directory=features --no-component
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626385323853/OUPI-Coi_.png)

The dep graph updated itself to show the new library. But there's a problem: no arrow!  
Let's change that.

* Move the `apps/frontend/src/app/giant-robots` folder into `libs/features/robots/src/lib`
* Update `libs/features/robots/src/index.ts` to export the component:

```ts
export * from './lib/giant-robots/giant-robots-list.component';
```

* Update `apps/frontend/src/app/app.tsx` to import the `GiantRobotsList` component from the new library:

```ts
import { useEffect, useState } from 'react';

// Components
import { GiantRobotsList } from '@giant-robots/features/robots';

// Environments
import { environment } from '../environments/environment';

function App() {
[...]
```

And the dep graph should now look like this:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626385799218/EPU1j9wil.png)

## Affected magic

Now the real fun begins.

* Close the dep graph process (`ctrl C` on the terminal where it's running)
* Commit all the changes you have
* Make a change in `libs/features/robots/src/giant-robots/giant-robots-list.component.tsx`:

```jsx
[...]
export const GiantRobotsList: FC<GiantRobotsListProps> = ({ giantRobots }) => (
  <>
    <h1>Giant robots we love</h1>
    <ul>
      {giantRobots.map((giantRobot) => (
        <li key={giantRobot.name}>
          {giantRobot.name} ({giantRobot.height}m &times; {giantRobot.weight}
          tons)
        </li>
      ))}
    </ul>
  </>
[...]
```

* Relaunch the dep graph like this:

```sh
nx affected:dep-graph
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626386312842/9Pl0Y6Fn8.png)

Nx knows what's changed, and it will show it to you!

And if you use the "Select All" button again:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626386364544/JzfMLx3yc.png)

🚀 Projects affected by your changes in red, the rest of the graph in black. Just how cool is that?

## Ok but what's in it for me?

Let me stress this again: Nx knows what's changed! This means that:

* `nx affected:test` will **only** test **all** projects affected by your changes
* `nx affected:lint` will **only** lint **all** projects affected by your changes
* `nx affected:e2e` will **only** e2e test **all** projects affected by your changes
* `nx affected:build` will **only** build **all** projects affected by your changes

👋 Goodbye wasted time: you can now architect your apps following a pattern of one light "app shell" and several feature libs, and only test and lint what's actually changed without having to do it all every time.

Once in a while you should still run a comprehensive test suite, and you should still lint a project as a whole.

But you don't have to do it every single time you change some comment in a single file anymore!

## One more thing... or two

There's one more powerful feature the dependency graph enables: Computation Caching.

* Close the dep-graph process and run:

```sh
nx affected:lint
```

* Now run it again. This time it should take just about a bunch of milliseconds, and this should be the output:

```sh
❯ nx affected:lint

>  NX   NOTE  Affected criteria defaulted to --base=master --head=HEAD


>  NX  Running target lint for 3 project(s):

  - features-robots
  - frontend
  - frontend-e2e

———————————————————————————————————————————————

> nx run frontend-e2e:lint [existing outputs match the cache, left as is]

Linting "frontend-e2e"...

All files pass linting.


> nx run frontend:lint [existing outputs match the cache, left as is]

Linting "frontend"...

All files pass linting.


> nx run features-robots:lint [existing outputs match the cache, left as is]

Linting "features-robots"...

All files pass linting.


———————————————————————————————————————————————

>  NX   SUCCESS  Running target "lint" succeeded

  Nx read the output from cache instead of running the command for 3 out of 3 tasks.
```

> Nx read the output from cache instead of running the command for 3 out of 3 tasks.

Awesome!

But there's more! There's **Distributed** Computation Caching, so that you can share results with your teammates, or with your CI server.

No need to wait for a task to finish only to get the exact same result a teammate got fifteen minutes ago.

I'm going to write about Distributed Computation Caching in a future post. In the meantime you can find all there is to know about  [Computation Caching on the official docs](https://nx.dev/latest/react/core-extended/computation-caching).

## Wrapping up

The dependency graph is what makes Nx tick, and it also enables awesome features and tools.

Thanks to it, organizing apps by using a light app shell and several libs pays back in terms of time spared, that you can reinvest into actually working on your projects.

Or having a beer 🍻, cheers!

Next time, we are going to build for production and deploy to Netlify 🚀

> Cover photo by <a href="https://unsplash.com/@scaitlin82?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Sarah Cervantes</a> on <a href="https://unsplash.com/s/photos/strong?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>