# ReactJS

[my projects with examples](https://github.com/cherkavi/javascripting/tree/master/react)  
[doc](https://create-react-app.dev/)  
[chrome plugin](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)  

## create project
```sh
npx create-react-app my-app
```

## create component
```sh
npm install --save-dev create-react-component-folder
npx crcf myComponent
```

![type of components](https://i.postimg.cc/RhWJ08B2/ksnip-20210211-230707.png)

![lifecycle - creation](https://i.postimg.cc/5y6kP6F9/lifecycle-creation-learning-card.png)
![lifecycle - update](https://i.postimg.cc/wxGr1cS1/lifecycle-update-external-learning-card.png)

## style pseudo selector
```js
# npm install --save radium

import Radium from 'radium';
style={
   ':hover': {
   	backgroundColor:"red"
   },
   '@media (min-witdh: 480px)':{
   	width: "350px"
   }
}

export default Radium(MyComponent);

// for media - wrap root component with:
// import { StyleRoot } from 'radium';
// <StyleRoot> </StyleRoot>
```

![NextJS ServerSideRendering](https://i.postimg.cc/L6nxk6BP/nextjs-ssr.png)

