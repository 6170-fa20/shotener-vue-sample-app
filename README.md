# URL Shortener
This sample app showcases how to deply your Vue app to Heroku.

## The way Vue builds
When you run "npm run build" Vue compiles your app into the dist folder. 'dist' is the folder name by default. You can change line 43 in app.js to use dist instead of public. The issue with that is now everytime you make a change you have to build the app again in order to see the changes in the dist folder. This is why Vue has a dev server. This dev server allows you to make changes and see them in real time. But this means you have to run 2 servers effectively. 

'npm run server' starts the Vue dev server whiles 'npm run awesome' starts the express server.

However, when you deploy to Heroku this is not an issue since you are not actively changing your code there. So in the Heroku environment you want to use the dist folder but when you are developing you want to use the public folder in conjunction with the dev server.

Now let's talk about how to prepare your app for Heroku.

## Notes for your App
- Add the dotenv package. This allows you to set variables in a .env file and access the vairables in your application through the process.env variable. Heroku uses the .env file to set environment variables, most notably of these is process.env.NODE_ENV which it sets to 'Production' so that your app knows it is in the Heroku environment. Carefully look at lines 7, 8 and 43 in app.js.
- In your package.json make sure to start, build and serve are included in your scripts field. Heroku by default will run these. See this sample app's package.json to see what it should look like.
- After this you can push your app to Heroku and you should be all set. Reach out to staff on Piazza or during Office Hours if you have any questions or run into any issues.

This sample app is deployed here: https://shotener-vue-sample-app.herokuapp.com/
