# Admin plugin

This plugin has started as an example of a few extensions.
The extension points are defined in `blueocean-web`.

However it had become the main app for all client side screens.

## Running this

### With mvn


1. Go into `blueocean-plugin` and run `mvn hpi:run` in a terminal. (mvn clean install from the root of the project is always a good idea regularly!)
2. From this directory, run `gulp bundle:watch` to watch for JS changes and reload them.
3. Open browser to http://localhost:8080/jenkins/blue/ to see this
4. hack away. Refreshing the browser will pick up changes. If you add a new extension point or export a new extension you may need to restart the `mvn hpi:run` process. 

### With npm/storybook

We are supporting React Storybook https://voice.kadira.io/introducing-react-storybook-ec27f28de1e2#.8zsjledjp 

```javascript
npm run storybook
```

Then it’ll start a webserver on port 9001. Further on any change it will
refresh the page for you on the browser. The design is not the same as 
in blueocean yet but you can fully develop the components without a 
running jenkins,

#### Writing Stories

Basically, a story is a single view of a component. It's like a test case, but you can preview it (live) from the Storybook UI.

You can write your stories anywhere you want. But keeping them close to your components is a pretty good idea.

Let's write some stories:

```js
// src/main/js/components/stories/button.js

import React from 'react';
import { storiesOf, action } from '@kadira/storybook';

storiesOf('Button', module)
  .add('with a text', () => (
    <button onClick={action('clicked')}>My First Button</button>
  ))
  .add('with no text', () => (
    <button></button>
  ));
```

Here, we simply have two stories for the built-in `button` component. But, you can import any of your components and write stories for them instead.

#### Configurations for storybook

Now you need to tell Storybook where it should load the stories from. For that, you need to add the new story to the configuration file `.storybook/config.js`:

```js
import { configure } from '@kadira/storybook';

function loadStories() {
  require('../components/stories/button'); // add this line after the loadStories()
  // require as many as stories you need.
}

configure(loadStories, module);
```

That's it. Now simply run “npm run storybook” and start developing your components.


### Testing

We have created different test environments that you can us during development.
To run the test once:

```
npm run test
```


#### TDD support via watch

```
npm run test:watch
```

Can be used for TDD with a rapid feedback loop (as soon you save all tests will run)

### Linting with npm

ESLint with React linting options have been enabled.

```
npm run lint
```

#### lint:fix

You can use the command lint:fix and it will try to fix all
offenses, however there maybe some more that you need to fix manually.

```
npm run lint:fix
```

#### lint:watch

You can use the command lint:watch and it will give rapid feedback 
(as soon you save, lint will run) while you try to fix all offenses.

```
gulp lint:watch --continueOnLint
```