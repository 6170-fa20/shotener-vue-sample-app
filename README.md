# URL Shortener
This sample app showcases how to deply your Vue app to Heroku.

## The way Vue builds
When you run "npm run build" Vue compiles your app into the dist folder. 'dist' is the folder name by default. You can change line 43 in app.js to use dist instead of public so you only need to run one server. The issue with that is now every time you make a change you have to build the app again in order to see the changes in the dist folder. This is why Vue has a dev server. This dev server allows you to make changes and see them in real time. But this means you have to run 2 servers. 

'npm run server' starts the Vue dev server whiles 'npm run awesome' starts the express server.

However, when you deploy to Heroku this is not an issue since you are not actively changing your code there. So in the Heroku environment you want to use the dist folder but when you are developing you want to use the public folder in conjunction with the dev server.

Now let's talk about how to prepare your app for Heroku.

## Notes for your App
- Add the dotenv package. This allows you to set variables in a .env file and access the vairables in your application through the process.env variable. Heroku uses the .env file to set environment variables, most notably process.env.NODE_ENV which it sets to 'Production' so that your app knows it is in the Heroku environment. Carefully look at lines 7, 8 and 45 in app.js.
- To reroute unmatched urls to the Vue frontend, you have to use the connect-history-api-fallback package. See https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations for more details. Carefully look at lines 9 and 44 in app.js
- In your package.json make sure you include the 2 new packages in your dependencies: connect-history-api-fallback and dotenv. See this app's package.json for an example.
- In your package.json make sure that start, build and serve are included in your scripts field. Heroku by default will run these. See this sample app's package.json to see what it should look like.
- After this you can push your app to Heroku and you should be all set. Reach out to staff on Piazza or during Office Hours if you have any questions or run into any issues.

This sample app is deployed here: https://shotener-vue-sample-app.herokuapp.com/

## General Notes
- To prevent accidentally assigning the same event listener multiple times for a specific event, you should clean up the event listeners when the component is destroyed. See components/UserSettings.vue and components/UserSettings.vue to see an example of how to handle this.
- The sample app that showcases how to use PostgreSQL as the persistent storage for your app can be found here.
https://github.com/6170-fa20/postgresql-sample-app
