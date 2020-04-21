# Weather Visualizer
This app was created using Vue.js! It gets your current location and displays the weather for it. The background and icons change according to the weather at your location! Please note that if you want the animated Skycon to appear, you will have to search for your location (I am trying to fix this bug).

Run the app at: https://klmui.com/Weather-Visualizer/

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


### My App Notes
- Create Vue app: "vue create 'app-name'
- Start: 'yarn serve'
- v-model is for two-way binding
- v-on
- v-if
- v-else-if
- v-else
- mounted is on load
- :class="Javascript logic here"

### Deploying
- checkout to gh-pages
- yarn build
- git add dist && git commit -m "Initial dist subtree commit"
- git subtree push --prefix dist origin gh-pages