<div align="center">
  <p>
 <a href="https://www.npmjs.com/package/clinei"><img src="https://img.shields.io/npm/v/clinei.svg?style=for-the-badge" alt="NPM version" /></a>
 <a href="https://www.npmjs.com/package/clinei"><img src="https://img.shields.io/npm/dt/clinei.svg?maxAge=3600&style=for-the-badge" alt="NPM downloads" /></a>
  <a href="https://www.npmjs.com/package/clinei"><img src="https://img.shields.io/npm/l/clinei.svg?maxAge=3600&style=for-the-badge" alt="license" /></a>
  </p>
</div>

# **command-line interface handler**

**clinei is a command line interface handler to facilitate the building of cli cli programs with stability and speed and also you can specify the type of entry of each command and customization that helps you write a clean application**
[Installation](#installation)

 [Map](#map)

[Usage](#Usage)

[Example](#Example)

# **Installation**

```sh-session
npm install clinei
yarn add clinei
```

# **Usage**

### CommonJS

```js
const { Build, Register } = require("clinei");
```

### ES6

```js
import { Build, Register } from "clinei";
```

# **Map**

> ## **RegisterConfig**

```js
const { Register } = require("clinei");
Register(
  {
    cmd: "",
    description: "",
    aliases: [],
    usage: "",
    options: [],
  },
  (get) => {
    //...
  }
);
```

> ### **RegisterConfigDeep**

### **Types**

> #### **Aliases**

```js
//alias name like -a (required)
name: string;
// command description (defualt:false - not required)
description: string;
//This is the type of alias input (defualt:false - not required)
type: boolean;
//This is the message of alias when error is thrown (defualt:false - not required)
msg: string;
//(defualt:false - not required)
required: boolean;
/*
if bind is true, then you can use it
 like <Prefix> testcmd -tcmd <command>
  or <Prefix> testcmd -tcmd
    if bind is false, then you can use it like <Prefix> -tcmd <command> or <Prefix> testcmd -tcmd (defualt:false - not required)
*/
bind: boolean;
```

> #### **options**

```js
//option name like --age (required)
name: string;
//command description (defualt:false - not required)
description: string;
//This is the type of option input (defualt:false - not required)
type: string | number | boolean;
//This is the message of option  when error is thrown (defualt:false - not required)
msg: string;
//(defualt:false - not required)
required: boolean;
```

### **ExplanationRegister**

```js
const { Register } = require("clinei");
Register(
  {
    cmd: "",
    description: "",
    aliases: [
      //if you want to use aliases you must use this
      {
        name: "",
        description: "",
        type: <YourType>, //this type: Boolean (Boolean,Number,String)
        msg: "",
        required: false,
        bind: false,
      },
    ],
    usage: "",
    options: [
      //if you want to use options you must use this
      {
        name: "",
        description: "",
        type: <YourType>, //this type: Boolean (Boolean,Number,String)
        msg: "",
        required: false,
      },
    ],
  },
  (get) => {
    //...
  }
);
```

## **getmethod**

> #### **get() to get all values for options and aliases**

### **options cli**

```sh-session
fake testcmd hi --username Arth --age 18
```

```js
console.log(get().options);
/*
[
  {
    option: "username",
    value: "Arth"
  },
  {
    option: "age",
    value: 18
  }
]
*/
```

> #### **get() specific value use key like this**

```js
console.log(get("username").options); //Arth
```

### **aliases cli**

```sh-session
fake testcmd hi -u Arth -a 18
```

```js
console.log(get().aliases);
/*
[
  {
    alias: "u",
    value: "Arth"
  },
  {
    alias: "a",
    value: 18
  }
  ]
 */
```

> #### **get() specific value use key like this**

```js
console.log(get("u").aliases); //Arth
```

> #### **Arguments**

### **arguments(normal) - cli**

```sh-session
fake testcmd hi
```

```js
console.log(get().args); // ["hi"]
```

### **arguments(argsminus) - cli**

## _NOTE **---** It should be at the end of command line_

```sh-session
fake testcmd hi --- args1 args2 args3
```

```js
console.log(get().argsminus); // ["args1","args2","args3"]
```

> #### **get() Not used much**

```js
//get all commands
console.log(get().commands); // [Object] see ExplanationRegister up for more info
//get prefix
console.log(get().prefix); // prefix
//get this command
console.log(get().this); // this command
//we use this when we need to access command information
/*
  {
    cmd: "",
    description: "",
    aliases: [],
    usage: "",
    options: [],
  }
*/
```

# **IFused**

> #### **if used bind, SupportsOnly `Aliases`**

```js
bind: true,//(default:false)
```

```sh-session
fake testcmd -tcmd
```

```js
bind: false,//(default:false)
```

```sh-session
fake -tcmd
```

> #### **if used type**

```js
type: String, //Boolean,Number,String (default:false)
```

> #### **Result** > **type**

```js
root\clinei\index.js:344
            throw new Error(
                  ^

Error: Invalid value for option password expected String got 123456
                                                            ^^^^^
<message when error is thrown>

```

> #### **if used msg**

```js
msg: "<message when error is thrown>",
```

> #### **Result** > **msg**

```js
root\clinei\index.js:344
            throw new Error(
                  ^

Error: Invalid value for <option or alias> <Name> expected String got 123456
                                                            ^^^^^
<message when error is thrown>

```

> #### **if used required**

```js
required: true,//(defualt:false)
```

> #### **Result** > **required**

```js
root\clinei\index.js:248
            throw new Error(
                  ^

Error: Missing required <option or alias> <Name>
                                ^^^^^
```

# **Example**

### **Run as an Test** `node index.js` **in terminal**

### **index.js**

```js
const { Build, Register } = require("clinei");
new Build({
  path: `${__dirname}/commands`, //path to commands folder
  prefix: "fake", //prefix of commands
});
```

### **commands/Admin/login.js**

```js
const { Register } = require("clinei");
module.exports = Register(
  {
    cmd: "login",
    description: "Log in and print data in console",
    aliases: [
      {
        name: "l",
        description: "Short For Login",
      },
      {
        name: "username",
        description: "Your name that you want to print",
        type: String,
        msg: "Please type it correctly ex:\n-username Arth",
        required: true,
        bind: true,
      },
      {
        name: "password",
        description: "Your password that you want to print",
        type: String,
        msg: "Please type it correctly ex:\n-password Arth",
        required: true,
        bind: true,
      },
    ],
    usage: "-username Arth -password 123456 --age 99 --save",
    options: [
      {
        name: "age",
        description: "Your age",
        type: Number,
        msg: "Please type it correctly ex:\n--age 99",
        required: true,
      },
      {
        name: "save",
        description: "Save your data locally",
        msg: "Please type it correctly ex:\n--save",
        required: false,
      },
    ],
  },
  (get) => {
    console.log(`Hello ${get("username").aliases}, your password is [${
      get("password").aliases
    }] and your age is ${
      get("age").options
    } and you want to save your data locally ${
      get("save").options ? "Yes" : "No"
    }
    `);
  }
);
```

## Links

- [Twiter](https://twitter.com/onlyarth)
- [Github](https://github.com/4i8)

## License

- [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0)

```

```
# clinei