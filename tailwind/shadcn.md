# 安装
```
bun x shadcn-vue@latest init
```

tsconfig.json 加,不然不能使用@xxx来导入
```
    "compilerOptions": {

    "baseUrl": ".",

    "paths": {

      "@/*": ["./src/*"]

    }

  },
```

加入组件
```
shadcn-vue@latest add button
```