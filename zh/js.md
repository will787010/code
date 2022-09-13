> lastupdate:  {docsify-updated}
## 格式化邮箱

```js
function formatEmail(string) {
  if (!string) {
    return '';
  }
  var arr = string.split('@');
  if (arr.length == 2) {
    if (arr[0].length <= 3) {
      return arr[0] + '@' + arr[1]
    }else {
      return arr[0].slice(0,3) + '****@' + arr[1];
    }
  }else {
    return '';
  }
}
```

## 格式化手机号码

```js
function formatMobile(string) {
  var result = '';
  if (string) {
    result = string.slice(0,3) + '****' + string.slice(-4);
  }
  return result;
}
```

## 生成uuid

```js
function uuid () {
  let date = new Date().getTime()
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, (c) => {
    let r = (date + Math.random() * 16) % 16 | 0
    date = Math.floor(date / 16)
    return (c === 'x' ? r : (r & 0x3) | 0x8).toString(16)
  })
}
```

## dataUrltofile

```js
function dataURLtoFile (dataurl, filename) {
  var arr = dataurl.split(','),
    mime = arr[0].match(/:(.*?);/)[1],
    bstr = atob(arr[1]),
    n = bstr.length,
    u8arr = new Uint8Array(n)
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n)
  }
  return new File([u8arr], filename, { type: mime })
}
```

## 打乱数组

```js
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1))
    ;[array[i], array[j]] = [array[j], array[i]]
  }
}
```

## 自定义storage

```js
function useStorage (storageType = 'sessionStorage') {
  const isBrowser = typeof window !== 'undefined'
  const types = ['sessionStorage', 'localStorage']
  if (types.indexOf(storageType) === -1) {
    console.warn('wrong storagetype')
    return null
  }
  const getItem = (key) => {
    return isBrowser ? window[storageType][key] : '';
  };

  const setItem = (key, value) => {
    if (isBrowser) {
      window[storageType].setItem(key, value);
      return true;
    }

    return false;
  };

  const removeItem = (key) => {
    if (isBrowser) {
      window[storageType].removeItem(key);
    }
  };

  const getAll = () => {
    const keys = Object.keys(window[storageType])
    const result = {}
    keys.map(key => {
      result[key] = getItem(key)
    })
    return result
  }

  return {
    getItem,
    getAll,
    setItem,
    removeItem
  };
};
```
## 获取url参数

```js
function getParameterByName(name) {
  name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
  var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
  str = location.search
  if (!str) {
    var index = location.href.indexOf('?')
    if (index >= 0) {
      str = location.href.slice(index)
    }
  }
  var results = regex.exec(str);
  return results === null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
}
```

## 获取url参数2

```js
function getParameter (str) {
  if (!location.search) return null
  const result = {}
  str = str || location.search.slice(1)
  const arr = str.split('&')
  arr.forEach(item => {
    let arr1 = item.split('=')
    if (arr1.length === 2) {
      result[arr1[0]] = decodeURIComponent(arr1[1])
    }
  })
  return result
}
```

## 是否在数组中

```js
function oneOf (item, arr) {
  return arr.indexOf(item) > -1
}
```
## 简单的yyyy--mm-dd

```js
function formateDate (timestamp) {
  let date = new Date()
  if (timestamp) {
    date = new Date(timestamp)
  }
  const year = date.getFullYear();
  const month = ("0" + (date.getMonth() + 1)).slice(-2);
  const day = ("0" + date.getDate()).slice(-2);
  const hour = ("0" + date.getHours()).slice(-2);
  const min = ("0" + date.getMinutes()).slice(-2);
  const sec = ("0" + date.getSeconds()).slice(-2);
  return `${year}-${month}-${day} ${hour}:${min}:${sec}`;
}
```

## 一个简答的websocket封装

```js
var socket = {
  ws: null,
  init: function(socketurl) {
    if (!socketurl) return
    var socket = window.WebSocket || window.MozWebSocket;
    if(!socket) {
      console.log('浏览器不支持websocket');
      return;
    }
    this.ws = new socket(socketurl) 
  },
  subscribe: function(params, callback) {
    if(this.ws === null) {
      this.init();
    }
    if(this.ws.readyState) {
      this.ws.send(JSON.stringify(params));
    } else {
      this.ws.onopen = evt => {
        this.ws.send(JSON.stringify(params));
      }
    }
    this.ws.onopen = e => {
    }
    this.ws.onmessage = e => {
      var str = e.data;
      var data;
      try {
        data = JSON.parse(e.data);
      } catch(err) {
        
      }
      callback && callback(data)
    }
  },
  close: function() {
    if(this.ws) {
      this.ws.close();
      this.ws = null;
    }
  }
}
```

## 自定义主题 vue.config.js

```js
const autoprefixer = require('autoprefixer')
const pxtorem = require('postcss-pxtorem')
var path = require('path')
const myTheme = path.resolve(__dirname, "src/css/var.less");

//
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');

const env = process.env.NODE_ENV;

const configureWebpack = config => {
  if (process.env.NODE_ENV === 'production') {
    config.optimization.minimizer[0].options.terserOptions.compress.drop_console = true
  }
}
module.exports = {
  pluginOptions: {
    'style-resources-loader': {
      preProcessor: 'less',
      patterns: [path.resolve(__dirname, 'src/css/mixin.less')]
    }
  },
  css: {
    loaderOptions: {
      postcss: {
        plugins: [
          autoprefixer(),
          pxtorem({
            rootValue: 10,
            propList: ['*','!border','!letter-spacing']
          })
        ]
      },
      less: {
        modifyVars: {
          hack: `true; @import "${myTheme}";`
        }
      }
    }
  },
  devServer: {
    proxy: {
      "/api": {
        target: 'http://xxx.yy/api',
        ws: true,
        pathRewrite: {
          // 路径重写
          "^/api": "" // 这个意思就是以api开头的，定向到哪里, 如果你的后边还有路径的话， 会自动拼接上
        }
      }
    }
  },
  configureWebpack
}
```

## 一个简单的深拷贝实现

```js
function cloneDeep (obj) {
  var result = Array.isArray(obj) ? [] : {}
  if (obj && typeof obj === 'object') {
    for (var key in obj) {
      if (obj.hasOwnProperty(key)) {
        if (typeof obj[key] === 'object') {
          result[key] = cloneDeep(obj[key])
        } else {
          result[key] = obj[key]
        }
      }
    }
  }
  return result
}
```

## 严格的获取类型的函数

```js
function getType (obj) {
  return Object.prototype.toString.call(obj).slice(8,-1).toLowerCase()
}
```

## 字符串快速反转

```js
function reverseStr (str) {
  return str.split('').reverse().join('')
}
```
## vue history 模式nginx配置

```config
try_files $uri $uri/ /index.html;
```

## 获取设置cookie

```js
function getCookie(name) {
    var arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
    if (arr = document.cookie.match(reg))
        return unescape(arr[2]);
    else
        return "";
}
```

```js
function setCookie(name, value) {
    var Days = 30; var exp = new Date();
    exp.setTime(exp.getTime() + Days * 24 * 60 * 60 * 1000);
    document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();
}
```

```js
function delCookie(key) {
    var date = new Date();
    date.setTime(date.getTime() - 1);
    var delValue = getCookie(key);
    if (!!delValue) {
        document.cookie = key + '=' + delValue + ';expires=' + date.toGMTString();
    }
}
```
