## Stressful and convoluted

Modern web development is a complex affair: bundling, linting, tests, design systems, TypeScript, Babel, production builds… woah, give me a break! I’m trying to ship features here!

I’m sorry but there’s more: now you have to fetch data from a web service, but the backend devs are using data models that are slightly different from what you are using in the frontend app and nobody is really sure why but you have to refactor your code to account for the differences… oof.
Just another Tuesday?

## Dream a little dream

Ok but it doesn’t have to be like this.

You deserve robust, effective defaults. Pre-configured tools that just work. Maybe even _*gasps*_ a suite of commands with a shared user interface to run apps and tools.

Complexity is not inevitable.

Misunderstandings are not inevitable.

That’s a Thanos thing.

## Enter Nx

![image.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1625259477851/ddoQcwVyj.gif)

> **First-class support for your favorite stack**
>
> Nx is a smart and extensible build framework to help you architect, test, and build at any scale — integrating seamlessly with modern technologies and libraries while providing a robust CLI, caching, dependency management, and more.

Think Lerna on steroids, Create React App or Angular CLI for frontend, backend, and more.

Developing with Nx is so comfortable I use it for everything, both at work and for my side projects no matter the complexity level: from trivial experiments to serious enterprise multi-app, multi-lib, endeavors.

## Highlights

If you read this far, you deserve some bullet points:

* Build apps and libs in the same monorepo sharing dependencies, tools, and workflows
* Manage React, Angular, Next.js, NestJS, apps with the same commands and the same options
* Built-in environment files for your global config constants
* Organize apps into libs and also share them between projects
* Dependency graph: only test, build, lint, what you need
* Local and cloud cache: don’t waste time rebuilding unchanged code

And there’s more.

Keep following this series, and I’ll show you how to improve your life as a developer with Nx.

Find Nx at https://nx.dev/.