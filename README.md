# Weather Visualizer
This app was created using Vue.js! It gets your current location and displays the weather for it. The background and icons change according to the weather at your location!
Run the app at: https://klmui.com/Weather-Visualizer/

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


### My Vue Notes
- Create Vue app: "vue create 'app-name'
- Start: 'yarn serve'
- v-model is for two-way binding
- v-on
- v-if
- v-else-if
- v-else
- v-model is for two-way binding
- {{ "Can call any functions or use data from below here" }}
- mounted is on load
- :class="Javascript logic here"

### Deploying
- checkout to gh-pages
- git merge master
- yarn build
- git add dist && git commit -m "Initial dist subtree commit"
- git subtree push --prefix dist origin gh-pages