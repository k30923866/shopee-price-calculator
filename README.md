<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>蝦皮售價計算器</title>

<style>
body{
background:#f5f5f5;
font-family:-apple-system;
max-width:500px;
margin:auto;
padding:15px;
}

.card{
background:white;
border-radius:20px;
overflow:hidden;
box-shadow:0 2px 10px rgba(0,0,0,.1);
}

.header{
background:linear-gradient(to right,#295eff,#2446db);
color:white;
padding:18px;
font-size:24px;
font-weight:bold;
}

.row{
display:flex;
justify-content:space-between;
padding:18px;
border-bottom:1px solid #eee;
font-size:20px;
}

.result{
background:#1ec97f;
color:white;
font-size:30px;
font-weight:bold;
}

input{
width:100%;
height:45px;
border-radius:10px;
border:1px solid #ddd;
font-size:18px;
margin-bottom:15px;
}

button{
width:100%;
height:50px;
border:none;
background:#16a34a;
color:white;
font-size:20px;
border-radius:15px;
}

.box{
background:white;
padding:20px;
border-radius:20px;
margin-bottom:20px;
}
</style>
</head>

<body>

<h1>蝦皮售價計算器</h1>

<div class="box">

商品成本

<input id="cost" value="100">

包裝成本

<input id="pack" value="10">

人力成本

<input id="labor" value="10">

目標利潤

<input id="profit" value="50">

<button onclick="cal()">
開始計算
</button>

</div>


<div class="card">

<div class="header">
一般日＋免運方案（18%）
</div>

<div class="row">
<div>建議售價</div>
<div id="price">$0</div>
</div>

<div class="row result" id="profitShow">
獲利 $0
</div>

</div>

<script>

function cal(){

let cost=Number(document.getElementById("cost").value);

let pack=Number(document.getElementById("pack").value);

let labor=Number(document.getElementById("labor").value);

let profit=Number(document.getElementById("profit").value);

let total=cost+pack+labor+profit;

let price=Math.ceil(total/0.82);

document.getElementById("price").innerHTML="$"+price;

document.getElementById("profitShow").innerHTML="獲利 $"+profit;

}

cal();

</script>

</body>
</html>
