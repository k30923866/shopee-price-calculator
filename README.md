<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>蝦皮售價計算器 V2.1</title>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
}

body{
background:#f3f4f6;
padding:15px;
}

.container{
max-width:600px;
margin:auto;
}

h1{
font-size:30px;
margin-bottom:20px;
font-weight:bold;
}

.card{
background:#fff;
border-radius:20px;
padding:20px;
box-shadow:0 2px 10px rgba(0,0,0,.08);
margin-bottom:20px;
}

.input-group{
margin-bottom:16px;
}

label{
display:block;
font-size:16px;
font-weight:bold;
margin-bottom:8px;
}

input{
width:100%;
height:50px;
padding:10px;
font-size:18px;
border-radius:12px;
border:1px solid #ddd;
}

select{
width:100%;
height:50px;
font-size:18px;
border-radius:12px;
border:1px solid #ddd;
padding:10px;
}

.blue-card{
background:linear-gradient(135deg,#2958ff,#1d43d8);
color:white;
border-radius:20px;
overflow:hidden;
margin-bottom:20px;
}

.yellow-card{
background:linear-gradient(135deg,#ffca28,#ffb300);
color:white;
border-radius:20px;
overflow:hidden;
margin-bottom:20px;
}

.card-title{
padding:20px;
font-size:24px;
font-weight:bold;
}

.result{
background:white;
color:#333;
}

.row{
display:flex;
justify-content:space-between;
align-items:center;
padding:16px 20px;
border-bottom:1px solid #eee;
font-size:18px;
}

.profit{
background:#16c784;
color:white;
font-size:28px;
font-weight:bold;
}

.loss{
background:#ef4444;
color:white;
font-size:28px;
font-weight:bold;
}

.switch{
margin-bottom:15px;
}

</style>
</head>

<body>

<div class="container">

<h1>
蝦皮售價計算器 V2.1
</h1>

<div class="card">

<div class="input-group">
<label>商品成本</label>
<input id="cost" type="number" value="100">
</div>

<div class="input-group">
<label>包裝成本</label>
<input id="pack" type="number" value="0">
</div>

<div class="input-group">
<label>人力成本</label>
<input id="labor" type="number" value="0">
</div>

<div class="input-group">
<label>目標利潤</label>
<input id="target" type="number" value="50">
</div>

<div class="input-group">
<label>是否預購（+3%）</label>
<select id="preorder">
<option value="0">否</option>
<option value="0.03">是</option>
</select>
</div>

<div class="input-group">
<label>是否參加免運（+6%）</label>
<select id="shipping">
<option value="0">否</option>
<option value="0.06" selected>是</option>
</select>
</div>

<div class="input-group">
<label>是否參加蝦幣回饋（+2%）</label>
<select id="coin">
<option value="0">否</option>
<option value="0.02">是</option>
</select>
</div>

<div class="input-group">
<label>2026促銷檔期（+2%）</label>
<select id="promo">
<option value="0">否</option>
<option value="0.02">是</option>
</select>
</div>

</div>
<!-- 一般日 18% -->

<div class="blue-card">

<div class="card-title">
一般日＋免運方案（18%）
</div>

<div class="result">

<div class="row">
<div>建議售價</div>
<div id="price18">$0</div>
</div>

<div class="row">
<div>成交手續費（6%）</div>
<div id="fee18">$0</div>
</div>

<div class="row">
<div>金流處理費（2.5%）</div>
<div id="pay18">$0</div>
</div>

<div class="row">
<div>預購手續費</div>
<div id="pre18">$0</div>
</div>

<div class="row">
<div>免運手續費</div>
<div id="ship18">$0</div>
</div>

<div class="row">
<div>蝦幣回饋</div>
<div id="coin18">$0</div>
</div>

<div class="row">
<div>促銷檔期</div>
<div id="promo18">$0</div>
</div>

<div class="row profit" id="profitBox18">
<div>獲利</div>
<div id="profit18">$0</div>
</div>

</div>

</div>



<!-- 活動日 20% -->

<div class="yellow-card">

<div class="card-title">
活動日＋免運方案（20%）
</div>

<div class="result">

<div class="row">
<div>建議售價</div>
<div id="price20">$0</div>
</div>

<div class="row">
<div>成交手續費（8%）</div>
<div id="fee20">$0</div>
</div>

<div class="row">
<div>金流處理費（2.5%）</div>
<div id="pay20">$0</div>
</div>

<div class="row">
<div>預購手續費</div>
<div id="pre20">$0</div>
</div>

<div class="row">
<div>免運手續費</div>
<div id="ship20">$0</div>
</div>

<div class="row">
<div>蝦幣回饋</div>
<div id="coin20">$0</div>
</div>

<div class="row">
<div>促銷檔期</div>
<div id="promo20">$0</div>
</div>

<div class="row profit" id="profitBox20">
<div>獲利</div>
<div id="profit20">$0</div>
</div>

</div>

</div>
<script>

function saveData(){
localStorage.setItem("cost",cost.value);
localStorage.setItem("pack",pack.value);
localStorage.setItem("labor",labor.value);
localStorage.setItem("target",target.value);
localStorage.setItem("preorder",preorder.value);
localStorage.setItem("shipping",shipping.value);
localStorage.setItem("coin",coin.value);
localStorage.setItem("promo",promo.value);
}

function loadData(){

if(localStorage.getItem("cost")){

cost.value=localStorage.getItem("cost");
pack.value=localStorage.getItem("pack");
labor.value=localStorage.getItem("labor");
target.value=localStorage.getItem("target");
preorder.value=localStorage.getItem("preorder");
shipping.value=localStorage.getItem("shipping");
coin.value=localStorage.getItem("coin");
promo.value=localStorage.getItem("promo");

}

}

function calc(baseRate,priceId,feeId,payId,preId,shipId,coinId,promoId,profitId,profitBoxId){

let c=Number(cost.value);
let p=Number(pack.value);
let l=Number(labor.value);
let t=Number(target.value);

let preRate=Number(preorder.value);
let shipRate=Number(shipping.value);
let coinRate=Number(coin.value);
let promoRate=Number(promo.value);

let totalRate=baseRate;

let totalCost=c+p+l+t;

let price=Math.ceil(totalCost/(1-totalRate));

document.getElementById(priceId).innerHTML="$"+price;

document.getElementById(feeId).innerHTML="$"+Math.round(price*baseRate);
document.getElementById(payId).innerHTML="$"+Math.round(price*0.025);
document.getElementById(preId).innerHTML="$"+Math.round(price*preRate);
document.getElementById(shipId).innerHTML="$"+Math.round(price*shipRate);
document.getElementById(coinId).innerHTML="$"+Math.round(price*coinRate);
document.getElementById(promoId).innerHTML="$"+Math.round(price*promoRate);

let profit=
price
-Math.round(price*baseRate)
-Math.round(price*0.025)
-Math.round(price*preRate)
-Math.round(price*shipRate)
-Math.round(price*coinRate)
-Math.round(price*promoRate)
-c-p-l;

document.getElementById(profitId).innerHTML="$"+profit;

if(profit>=0){
document.getElementById(profitBoxId).className="row profit";
}else{
document.getElementById(profitBoxId).className="row loss";
}

}

function updateAll(){

calc(
0.18,
"price18",
"fee18",
"pay18",
"pre18",
"ship18",
"coin18",
"promo18",
"profit18",
"profitBox18"
);

calc(
0.20,
"price20",
"fee20",
"pay20",
"pre20",
"ship20",
"coin20",
"promo20",
"profit20",
"profitBox20"
);

saveData();

}

loadData();

document.querySelectorAll("input,select").forEach(item=>{
item.addEventListener("input",updateAll);
});

updateAll();

</script>

</body>
</html>
