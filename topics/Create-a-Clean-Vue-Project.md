# Create a Clean Vue Project

Create a new project wither by using the command line -

```Bash
npm create vue@latest
```

Or in WebStorm select new project, Vue, then un-tick use default options if you wish to use router or pinia etc

Answer the questions as they are displayed in the command line regarding what options you would like installed

Run npm install (this is run via popup in WebStorm)

### Remove Unneeded Code and Files

Within the src folder, delete the contents of the assets, components and views folders, and also the contents of the
store folder if you are using pinia

open the App.vue file and delete everthing witin the script and style tags, delete everything from within the template
tags except router-view (if you are using vue router)

If you are using vue router - open the index.js file within the router folder and deelete any imported views and 
any routes within the routes array

