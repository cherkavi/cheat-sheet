# ReactJS

[my projects with examples](https://github.com/cherkavi/javascripting/tree/master/react)  
[doc](https://create-react-app.dev/)  
[chrome plugin](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)  

![NextJS ServerSideRendering](https://i.postimg.cc/L6nxk6BP/nextjs-ssr.png)

## create project
```sh
npx create-react-app my-app
```

## create component
```sh
npm install --save-dev create-react-component-folder
npx crcf myComponent
```

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
