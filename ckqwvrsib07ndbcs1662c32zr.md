## Nx: sharing code between applications

>💡 **Heads up!** You can find the code for every article in this series on GitHub, with a tag per article: https://github.com/trumbitta/giant-robots/tags

In [the previous post of this series](https://trumbitta.hashnode.dev/baking-a-backend-service-with-nx) you learned how quick and easy it is to add a backend application (well, any kind of application, actually) to a Nx workspace.

Let's do something with it.

## This is us doing something with it

NestJS is a powerful, versatile, Node framework. With it you can have a solid, well architected, maintainable, testable application.

But this is not a series on NestJS, so we are just going to invade the pre-configured route and be perfectly fine with it.

Open `apps/backend/src/app/app.service.ts` and update it like this:

```ts
import { Injectable } from '@nestjs/common';

interface GiantRobot {
  name: string;
  height: number;
  weight: number;
}

@Injectable()
export class AppService {
  getData(): GiantRobot[] {
    return [
      {
        name: 'Mazinger Z',
        height: 18,
        weight: 20,
      },
      {
        name: 'God Sigma',
        height: 66,
        weight: 1200,
      },
      {
        name: 'GoLion',
        height: 60,
        weight: 700,
      },
      {
        name: 'Tengen Toppa Gurren Lagann',
        height: 9.4607305e22,
        weight: Infinity,
      },
      {
        name: 'Albegas',
        height: 40,
        weight: 1200,
      },
    ] as GiantRobot[];
  }
}

```

Oh by the way, did I tell you NestJS has automagic hot reload? Yup, you don't need to close and relaunch it if you already have it running.

If not, by now you should be quite familiar with running:

```sh
nx run backend:serve
```

Then pointing your browser to http://localhost:3333/api

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625846624135/aIemx388Z.png)

## Using the data in the frontend app

In the previous post of this series, you asked Nx to create a backend application for you. You also told Nx you'd be going to use it together with the `frontend` frontend app.

This means Nx pre-configured the webpack DevServer proxy for you so that any problem with CORS while developing is already solved.

If you are curious, go take a look at `apps/frontend/proxy.conf.json`

```json
{
  "/api": {
    "target": "http://localhost:3333",
    "secure": false
  }
}
```

and at the `serve` target configuration for `frontend` in `workspace.json`:

```json
[...]
        "serve": {
          "executor": "@nrwl/web:dev-server",
          "options": {
            "buildTarget": "frontend:build",
            "hmr": true,
            "proxyConfig": "apps/frontend/proxy.conf.json"
          },
[...]
```

Now open `apps/frontend/src/app/app.tsx`, delete everything, and paste this in:

```ts
import { useEffect, useState } from 'react';

// Components
import { GiantRobotsList } from './giant-robots/giant-robots-list.component';

// Environments
import { environment } from '../environments/environment';

function App() {
  const { fairAdjective } = environment;
  const [giantRobots, setGiantRobots] = useState([]);

  useEffect(() => {
    // Thank you, Nx, for the proxy conf!
    fetch('/api')
      .then((response) => response.json())
      .then(
        (data) => setGiantRobots(data)
        // TODO: Add error handling
      );
  }, []);

  return (
    <>
      <h1>Welcome to the {fairAdjective} Giant Robot Fair!</h1>
      <main>
        <GiantRobotsList giantRobots={giantRobots} />
      </main>
    </>
  );
}

export default App;
```

It won't work, because we still need to create the `GiantRobotList` component.

Create `apps/frontend/src/app/giant-robots/giant-robots-list.component.tsx`:

```ts
import { FC } from 'react';

export interface GiantRobotsListProps {
  giantRobots: GiantRobot[];
}

export const GiantRobotsList: FC<GiantRobotsListProps> = ({ giantRobots }) => (
  <ul>
    {giantRobots.map((giantRobot) => (
      <li key={giantRobot.name}>
        {giantRobot.name} ({giantRobot.height}m &times; {giantRobot.weight}tons)
      </li>
    ))}
  </ul>
);
```

It still won't work, though, because we need to import the `GiantRobot` model! Importing directly from another app? *Ew. Gross.* Apps aren't supposed to be strictly coupled like that.

Nx will do us a solid here.

## Creating a lib and importing it into applications

A lib of shared models, in our case. Just run:

```sh
nx generate @nrwl/workspace:library --name=models --directory=shared --strict --unitTestRunner=none

# or just nx generate @nrwl/workspace:library then answer some questions
# or use Nx console
```

* Open `apps/backend/src/app/app.service.ts`
* Cut  the `GiantRobot` interface
* Now open `libs/shared/models/src/lib/shared-models.ts` and export the interface from there:

```ts
export interface GiantRobot {
  name: string;
  height: number;
  weight: number;
}
```

* Finally, import the same `GiantRobot` model from the new library both in `apps/backend/src/app/app.service.ts` and in `apps/frontend/src/app/giant-robots/giant-robots-list.component.tsx`

```ts
// apps/backend/src/app/app.service.ts
import { Injectable } from '@nestjs/common';

// Libs
import { GiantRobot } from '@giant-robots/shared/models';

@Injectable()
[...]
```

```ts
// apps/frontend/src/app/giant-robots/giant-robots-list.component.tsx
import { FC } from 'react';

// Libs
import { GiantRobot } from '@giant-robots/shared/models';

export interface GiantRobotsListProps {
  giantRobots: GiantRobot[];
}

[...]
```

What? Nx created the new lib and it also assigned a package name to it? How cool is that?

That's right: every lib you will create, no matter the type, will be accessible from the same custom package name plus its relative path. Fantastic.

Now make sure to have two terminals with the two applications running:

```sh
nx run backend:serve
```

```sh
nx run frontend:serve
```

And point your browser to http://localhost:4200

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1625866299746/VJO_q5B0Q.png)

🍻 Cheers!

## Wrapping up

In this post you put together everything you learned about Nx from this series, and took it to the next level by learning how to share code using libs.

But this is just the beginning, and the real fun begins with the next post: you're going to learn about Nx's biggest differentiator, and the enabler of very very cool features: the dependency graph.