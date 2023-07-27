# eslint校验

1. 

```
npm i -D eslint
npx eslint -init
```

2. 创建修改.eslintrc文件

```
{
    "env": {
        "browser": true,
        "es2021": true,
        "node": true
    },
    "extends": [
        // "standard-with-typescript",
        // "plugin:vue/vue3-essential"
        "standard",
        "plugin:vue/vue3-recommended"
    ],
    "overrides": [
        {
            "env": {
                "node": true
            },
            "files": [
                ".eslintrc.{js,cjs}"
            ],
            "parserOptions": {
                "sourceType": "script"
            }
        }
    ],
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": [
        "vue"
    ],
    "rules": {
        "semi": [2, "never"], // 禁止尾部使用分号
        "no-var": "error", // 禁止使用var
        "indent": ["error", 2], // 缩进2格
        "no-mixed-spaces-and-tabs": "error", // 不能空格与tab混用
        "quotes": [2, "double"], // 使用单引号
        "vue/html-closing-bracket-newline": "off", // 不强制换行
        "vue/singleline-html-element-content-newline": "off", //不强制换行
        "vue/max-attributes-per-line": ["error", {
            "singleline": { "max": 5 },
            "multiline": { "max": 5 }
        }]
    }
}

```

3. 安装vite-plugin-eslint，在vite.config.js添加

```
import eslint from "vite-plugin-eslint"
plugins: [vue(),eslint(),]
```

