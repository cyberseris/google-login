# Google 第三方登入練習

### 建立專案

```
npm init -y
```

### 安裝套件

```
npm install express passport passport-google-oauth20 express-session dotenv
```

### google 設定

google cloud console
![image](https://hackmd.io/_uploads/rkZbonJJgg.png)

選取專案 >> 新增專案
![image](https://hackmd.io/_uploads/rkkSi2kkxg.png)

專案名稱 >> google auth >> create
![image](https://hackmd.io/_uploads/Sy1tjnJkge.png)

選取專案
![image](https://hackmd.io/_uploads/By47hn1ygg.png)

左上角漢堡選單 >> 選擇 API 和服務
![image](https://hackmd.io/_uploads/HkCG9hJygx.png)

選擇 OAuth 同意畫面
![image](https://hackmd.io/_uploads/Byrih2Jyxl.png)

![image](https://hackmd.io/_uploads/HJGpnhyyex.png)

資料存取權
![image](https://hackmd.io/_uploads/rkbzTn1kgx.png)

點擊開始
![image](https://hackmd.io/_uploads/rJydT3k1le.png)

![image](https://hackmd.io/_uploads/H1IRMkxJxe.png)

![image](https://hackmd.io/_uploads/rk90p2Jkll.png)

![image](https://hackmd.io/_uploads/BynzQJeyxl.png)

![image](https://hackmd.io/_uploads/Sk0PAnJkll.png)

![image](https://hackmd.io/_uploads/H14tRn1yxg.png)

資料存取權
![image](https://hackmd.io/_uploads/HkonRnyyee.png)

點擊新增或移除範圍
![image](https://hackmd.io/_uploads/SJz4JpJylx.png)
更新
![image](https://hackmd.io/_uploads/rJJwJ6Jklx.png)
儲存
![image](https://hackmd.io/_uploads/Byadk61ylx.png)
![image](https://hackmd.io/_uploads/Hynikakkxe.png)

回到憑證
![image](https://hackmd.io/_uploads/S1n1g6k1eg.png)

建立憑證 >> OAuth 用戶端 ID
![image](https://hackmd.io/_uploads/rJT7eaykgl.png)

選擇網頁應用程式
![image](https://hackmd.io/_uploads/HJ9dx6kkgx.png)

名稱 >> Web Client 2.0 >> 已授權的重新導向 URI >> 新增 URI
![image](https://hackmd.io/_uploads/ByyzWaJ1xl.png)

URI1 >> `http://localhost:3000/auth/google/callback` >> CREATE
![image](https://hackmd.io/_uploads/SymqZ6JJle.png)

成功建立憑證
![image](https://hackmd.io/_uploads/BkXOQkxJlx.png)

將 Client ID 和 密碼貼到 .env
![image](https://hackmd.io/_uploads/HyvnXkgkxx.png)

index.js

```
require('dotenv').config()

const express = require('express')
const passport = require('passport')
const session = require('express-session')
const GoogleStrategy = require('passport-google-oauth20').Strategy

const app = express()

app.use(session({
    secret: "secret",
    resave: false,
    saveUninitialized: true
}))

app.use(passport.initialize())
app.use(passport.session())

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: 'http://localhost:3000/auth/google/callback'
}, (accessToken, refreshToken, profile, done) => {
    return done(null, profile)
}))

passport.serializeUser((user, done) => done(null, user))
passport.deserializeUser((user, done) => done(null, user))

app.get("/", (req, res) => {
    res.send("<a href='auth/google'>Login with Google</a>")
})

app.get("/auth/google", passport.authenticate('google', {scope: ["profile", "email"]}))

app.get("/auth/google/callback", passport.authenticate('google', {failureRedirect: "/"}), (req, res)=>{
    res.redirect('/profile')
})

console.log(JSON);
app.get("/profile", (req, res) => {
    res.send(`Welcome ${req.user.displayName}, ${JSON.stringify(req.user)}`);
})

app.get("/logout", (req, res) => {
    req.logout(()=>{
        res.redirect("/")
    });
})

app.listen(3000, () => {
    console.log(`Server is running at port 3000`)
})
```

google 登入
![image](https://hackmd.io/_uploads/H1RlEJg1le.png)

登入成功
![image](https://hackmd.io/_uploads/Sykn41x1ex.png)

登出
![image](https://hackmd.io/_uploads/rJa5j61kxe.png)
