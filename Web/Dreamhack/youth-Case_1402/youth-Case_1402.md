# 문제 설명
<img width="365" height="146" alt="image" src="https://github.com/user-attachments/assets/f3dcae2e-eecc-464f-9129-cabd28177dde" /><br>
### 목표
문제를 통해 **플래그(FLAG)** 획득<br>
# 코드 분석
```js
const words = require("./ag")
const express = require("express")

const PORT   = 3000
const app = express()
app.set('case sensitive routing', true)
app.use(express.urlencoded({ extended: true }))

function search(words, leg) {
    return words.find(word => word.name === leg.toUpperCase())
}

app.get("/",(req, res)=>{
    return res.send("hi guest")
})

app.post("/shop",(req, res)=>{
    const leg = req.body.leg.toLowerCase()

    if (leg == 'flag'){
        return res.status(403).send("Access Denied")
    }

    const obj = search(words,leg)

    if (obj){
        return res.send(JSON.stringify(obj))
    }

    return res.status(404).send("Nothing")
})

app.listen(PORT,()=>{
    console.log(`[+] Started on ${PORT}`)
})
```
