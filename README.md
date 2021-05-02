
# frontend-tech-challenge-time

## Description
This repository contains the frontend of a web app for creating, querying, updating and deleting timing sessions.
It has been built using svelte and urql for connecting to a GraphQL backend.

## Notes
 * As the frontend was not the focus of this challenge. The code is implemented and work but I have not been rigorous or specifically clean on its implementation, things could be cleaner.
 * No tests have beeen developed for the frontend as, again, the frontend is not the focus of this challenge.

## Execute
Refer to [tech-challenge-time](https://github.com/OscarClemente/tech-challenge-time/tree/main) to execute the full app.

## Execute only the frontend

If you want to only build the frontend then:
Install the dependencies...

```bash
cd svelte-app
npm install
```

...then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see your app running. Edit a component file in `src`, save it, and reload the page to see your changes.

By default, the server will only respond to requests from localhost. To allow connections from other computers, edit the `sirv` commands in package.json to include the option `--host 0.0.0.0`.

## Building and running in production mode

To create an optimised version of the app:

```bash
npm run build
```

You can run the newly built app with `npm run start`. This uses [sirv](https://github.com/lukeed/sirv), which is included in your package.json's `dependencies` so that the app will work when you deploy to platforms like [Heroku](https://heroku.com).

