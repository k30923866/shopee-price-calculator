<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>蝦皮售價計算器 V2</title>

<style>
*{
box-sizing:border-box;
margin:0;
padding:0;
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

.title{
font-size:28px;
font-weight:bold;
margin-bottom:15px;
}

.card{
background:white;
border-radius:20px;
padding:20px;
box-shadow:0 3px 10px rgba(0,0,0,.1);
margin-bottom:20px;
}

.input-group{
margin-bottom:15px;
}

label{
display:block;
font-weight:bold;
margin-bottom:8px;
}

input{
width:100%;
padding:14px;
border:1px solid #ddd;
border-radius:12px;
font-size:18px;
}

.switch-group{
display:flex;
justify-content:space-between;
align-items:center;
margin-bottom:15px;
}

select{
padding:10px;
border-radius:10px;
font-size:16px;
}

.blue-card{
background:linear-gradient(135deg,#2046d8,#3457ff);
color:white;
border-radius:20px;
overflow:hidden;
margin-bottom:20px;
}

.yellow-card{
background:linear-gradient(135deg,#ffbf00,#f6d84b);
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
padding:16px 20px;
border-bottom:1px solid #eee;
font-size:18px;
}

.profit{
background:#16c784;
color:white;
font-size:30px;
font-weight:bold;
}

.loss{
background:#ef4444;
color:white;
font-size:30px;
font-weight:bold;
}
</style>
</head>

<body>

<div class="container">

<div class="title">
蝦皮售價計算器 V2
</div>

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
<input id="profitTarget" type="number" value="50">
</div>

<div class="switch-group">
<label>預購訂單 (+3%)</label>
<select id="preorder">
<option value="0">否</option>
<option value="0.03">是</option>
</select>
</div>

<div class="switch-group">
<label>免運方案 (+6%)</label>
<select id="shipping">
<option value="0">否</option>
<option value="0.06" selected>是</option>
</select>
</div>

<div class="switch-group">
<label>蝦幣回饋 (+2%)</label>
<select id="coin">
<option value="0">否</option>
<option value="0.02">是</option>
</select>
</div>

</div>

<div class="blue-card">

<div class="card-title">
一般日＋免運方案
</div>

<div class="result">

<div class="row">
<div>建議售價</div>
<div id="price1">$0</div>
</div>

<div class="row">
<div>成交手續費</div>
<div id="fee1">$0</div>
</div>

<div class="row">
<div>金流處理費</div>
<div id="pay1">$0</div>
</div>

<div class="row">
<div>預購手續費</div>
<div id="pre1">$0</div>
</div>

<div class="row">
<div>免運手續費</div>
<div id="ship1">$0</div>
</div>

<div class="row profit">
<div>獲利</div>
<div id="profit1">$0</div>
</div>

</div>
</div>
<div class="yellow-card">

<div class="card-title">
活動日＋免運方案
</div>

<div class="result">

<div class="row">
<div>建議售價</div>
<div id="price2">$0</div>
</div>

<div class="row">
<div>成交手續費</div>
<div id="fee2">$0</div>
</div>

<div class="row">
<div>金流處理費</div>
<div id="pay2">$0</div>
</div>

<div class="row">
<div>預購手續費</div>
<div id="pre2">$0</div>
</div>

<div class="row">
<div>免運手續費</div>
<div id="ship2">$0</div>
</div>

<div class="row profit">
<div>獲利</div>
<div id="profit2">$0</div>
</div>

</div>
</div>

</div>

<script>

function calc(rate,priceId,feeId,payId,preId,shipId,profitId){

let cost=Number(document.getElementById("cost").value);
let pack=Number(document.getElementById("pack").value);
let labor=Number(document.getElementById("labor").value);
let target=Number(document.getElementById("profitTarget").value);

let preorder=Number(document.getElementById("preorder").value);
let shipping=Number(document.getElementById("shipping").value);
let coin=Number(document.getElementById("coin").value);

let totalCost=cost+pack+labor+target;

let totalRate=rate+0.025+preorder+shipping+coin;

let price=Math.ceil(totalCost/(1-totalRate));

let fee=Math.round(price*rate);
let pay=Math.round(price*0.025);
let pre=Math.round(price*preorder);
let ship=Math.round(price*shipping);

let realProfit=
price-fee-pay-pre-ship-cost-pack-labor;

document.getElementById(priceId).innerHTML="$"+price;
document.getElementById(feeId).innerHTML="$"+fee;
document.getElementById(payId).innerHTML="$"+pay;
document.getElementById(preId).innerHTML="$"+pre;
document.getElementById(shipId).innerHTML="$"+ship;
document.getElementById(profitId).innerHTML="$"+Math.round(realProfit);

let profitBox=document.getElementById(profitId).parentElement;

if(realProfit>=0){
profitBox.className="row profit";
}else{
profitBox.className="row loss";
}

}

function updateAll(){

// 一般日 6%
calc(
0.06,
"price1",
"fee1",
"pay1",
"pre1",
"ship1",
"profit1"
);

// 活動日 8%
calc(
0.08,
"price2",
"fee2",
"pay2",
"pre2",
"ship2",
"profit2"
);

}

document.querySelectorAll("input,select").forEach(item=>{
item.addEventListener("input",updateAll);
});

updateAll();

</script>

</body>
</html>
