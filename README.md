
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#104b38">
<title>Personal Expense Tracker Pro</title>
<style>
:root{--green:#104b38;--deep:#092e24;--gold:#d8b45a;--bg:#f3f6f2;--white:#fff;--red:#b83b42}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:#20352c;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif}
header{background:linear-gradient(135deg,var(--deep),#176349);color:white;padding:26px 18px 30px;border-bottom:4px solid var(--gold)}
.wrap{max-width:850px;margin:auto}
.brand{font-size:12px;letter-spacing:2px;color:var(--gold);font-weight:800}
h1{font-size:27px;margin:8px 0}
header p{color:#d9e8df;font-size:14px;margin:0}
main{max-width:850px;margin:auto;padding:16px}
.cards{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}
.card,.panel{background:white;border:1px solid #e0e8e1;border-radius:16px;padding:16px;box-shadow:0 3px 12px #102f1909}
.card span{font-size:12px;color:#66776c}
.money{font-size:22px;font-weight:800;margin-top:8px;overflow-wrap:anywhere}
.income{color:#188253}.expense{color:var(--red)}
.panel{margin-top:14px}
h2{font-size:17px;color:var(--green);margin:0 0 14px}
label{display:block;font-size:13px;font-weight:650;margin-bottom:6px}
input,select,button{font:inherit}
input,select{width:100%;padding:12px;border:1px solid #d5dfd6;border-radius:10px;background:#fff;color:#20352c;min-height:44px}
input:focus,select:focus{outline:2px solid #a8cbb7;border-color:var(--green)}
.formgrid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px}
.field{min-width:0}
.full{grid-column:1/-1}
button{border:0;border-radius:10px;padding:12px 14px;font-weight:750;cursor:pointer;min-height:44px}
.primary{background:var(--green);color:white;width:100%;margin-top:13px}
.secondary{background:#e7eee8;color:var(--green)}
.danger{background:#fff0ef;color:var(--red)}
.tools{display:flex;gap:8px;flex-wrap:wrap}
.tools button{flex:1}
.transaction{padding:13px 0;border-bottom:1px solid #e8eee9;display:flex;gap:10px;align-items:center;justify-content:space-between}
.transaction:last-child{border-bottom:0}
.txinfo{min-width:0;flex:1}
.txname{font-weight:750;overflow-wrap:anywhere}
.meta{font-size:12px;color:#758078;margin-top:5px}
.txamount{font-weight:800;white-space:nowrap}
.txbuttons{display:flex;gap:5px;margin-top:7px}
.txbuttons button{min-height:34px;padding:6px 10px;font-size:12px}
.empty{text-align:center;padding:24px;color:#7b897f;font-size:14px}
.progress{height:10px;background:#e5ece6;border-radius:99px;overflow:hidden;margin:12px 0}
.bar{height:100%;width:0;background:linear-gradient(90deg,#176349,var(--gold));transition:width .2s}
.small{font-size:12px;color:#728076;line-height:1.6}
footer{text-align:center;padding:24px;color:#7b897f;font-size:12px}
@media(max-width:480px){main{padding:12px}.formgrid{grid-template-columns:1fr}.full{grid-column:auto}.money{font-size:19px}.card,.panel{padding:13px}h1{font-size:24px}}
</style>
</head>
<body>
<header><div class="wrap">
<div class="brand">EXECUTIVE • GREEN & GOLD</div>
<h1>Personal Expense Tracker</h1>
<p>Know your money. Plan your future.</p>
</div></header>
<main>
<div class="cards">
<div class="card"><span>MONTHLY INCOME</span><div id="totalIncome" class="money income">RM 0.00</div></div>
<div class="card"><span>MONTHLY EXPENSES</span><div id="totalExpense" class="money expense">RM 0.00</div></div>
<div class="card"><span>MONTHLY BALANCE</span><div id="balance" class="money">RM 0.00</div></div>
<div class="card"><span>RECORDS THIS MONTH</span><div id="count" class="money">0</div></div>
</div>

<section class="panel">
<h2>Monthly Overview</h2>
<div class="field"><label for="month">Choose month</label><input type="month" id="month"></div>
<div style="margin-top:15px;display:flex;justify-content:space-between;gap:8px;flex-wrap:wrap">
<strong>Monthly spending budget</strong><strong id="budgetLabel">RM 0.00</strong>
</div>
<div class="progress"><div id="budgetBar" class="bar"></div></div>
<div id="budgetStatus" class="small">Set a budget to track your spending.</div>
<div class="field" style="margin-top:12px"><label for="budget">Set budget (RM)</label><input id="budget" type="number" min="0" step="0.01" placeholder="e.g. 1500"></div>
<button class="primary" id="saveBudget">Save Monthly Budget</button>
</section>

<section class="panel" id="entryPanel">
<h2 id="formTitle">Add a Transaction</h2>
<form id="transactionForm">
<div class="formgrid">
<div class="field"><label for="type">Transaction type</label><select id="type"><option value="expense">Expense</option><option value="income">Income</option></select></div>
<div class="field"><label for="amount">Amount (RM)</label><input id="amount" type="number" min="0.01" step="0.01" placeholder="0.00" required></div>
<div class="field"><label for="category">Category</label><select id="category"></select></div>
<div class="field"><label for="date">Date</label><input id="date" type="date" required></div>
<div class="field full"><label for="note">Description</label><input id="note" maxlength="100" placeholder="e.g. Lunch, salary, transport"></div>
</div>
<button class="primary" type="submit" id="submitBtn">+ Add Transaction</button>
<button class="secondary" type="button" id="cancelEdit" style="display:none;width:100%;margin-top:8px">Cancel Editing</button>
</form>
</section>

<section class="panel">
<h2>Transaction History</h2>
<div class="formgrid">
<div class="field"><label for="filterType">Filter type</label><select id="filterType"><option value="all">All transactions</option><option value="income">Income only</option><option value="expense">Expenses only</option></select></div>
<div class="field"><label for="search">Search description</label><input id="search" placeholder="Search records..."></div>
</div>
<div id="transactions"></div>
</section>

<section class="panel">
<h2>Backup & Data</h2>
<div class="tools">
<button class="secondary" id="exportBtn">Export CSV</button>
<button class="secondary" id="shareBtn">Share Summary</button>
<button class="danger" id="resetBtn">Reset All Data</button>
</div>
<p class="small">Records are saved in this browser on this device. Export CSV regularly to keep a backup. Clearing browser data or changing devices may remove access to your records.</p>
</section>
<footer>PERSONAL EXPENSE TRACKER PRO<br>Private by design • Executive Green & Gold</footer>
</main>
<script>
(function(){
"use strict";
const $=id=>document.getElementById(id);
const KEY="expenseTrackerPro_v1";
const today=new Date();
const localDate=d=>d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0");
const monthNow=localDate(today).slice(0,7);
$("month").value=monthNow;
$("date").value=localDate(today);
let data={transactions:[],budgets:{}},editing=null;
const categories={
expense:["Food & Drinks","Rent","Transport","Groceries","Shopping","Bills & Utilities","Health","Family","Education","Entertainment","Travel","Other"],
income:["Salary","Overtime","Business","Freelance","Investment","Gift","Other Income"]
};
function money(n){return "RM "+Number(n||0).toLocaleString("en-MY",{minimumFractionDigits:2,maximumFractionDigits:2})}
function safeLoad(){
try{
const raw=localStorage.getItem(KEY);
if(raw){const parsed=JSON.parse(raw);if(Array.isArray(parsed.transactions)&&parsed.budgets&&typeof parsed.budgets==="object")data=parsed}
}catch(e){alert("Saved data could not be read. Existing browser data has not been intentionally cleared.")}
}
function save(){try{localStorage.setItem(KEY,JSON.stringify(data));return true}catch(e){alert("Unable to save. Your browser storage may be full or unavailable. Export your data if possible.");return false}}
function selectedMonth(){return $("month").value||monthNow}
function updateCategories(chosen){
const list=categories[$("type").value];
$("category").innerHTML="";
list.forEach(c=>{const op=document.createElement("option");op.value=c;op.textContent=c;$("category").appendChild(op)});
if(chosen&&list.includes(chosen))$("category").value=chosen;
}
function monthly(){return data.transactions.filter(t=>t.date.startsWith(selectedMonth()))}
function renderSummary(){
const list=monthly();
const inc=list.filter(t=>t.type==="income").reduce((s,t)=>s+t.amount,0);
const exp=list.filter(t=>t.type==="expense").reduce((s,t)=>s+t.amount,0);
$("totalIncome").textContent=money(inc);
$("totalExpense").textContent=money(exp);
$("balance").textContent=money(inc-exp);
$("balance").className="money "+(inc-exp<0?"expense":"income");
$("count").textContent=list.length;
const budget=Number(data.budgets[selectedMonth()]||0);
$("budget").value=budget||"";
$("budgetLabel").textContent=money(budget);
const pct=budget>0?Math.min(100,exp/budget*100):0;
$("budgetBar").style.width=pct+"%";
$("budgetStatus").textContent=budget>0?(exp>budget?"Budget exceeded by "+money(exp-budget):"You have "+money(budget-exp)+" left ("+Math.round(pct)+"% used)."):"Set a budget to track your spending.";
}
function renderTransactions(){
const root=$("transactions");
root.innerHTML="";
const query=$("search").value.trim().toLowerCase();
const ft=$("filterType").value;
const list=data.transactions.filter(t=>t.date.startsWith(selectedMonth())&&(ft==="all"||t.type===ft)&&((t.note+" "+t.category).toLowerCase().includes(query))).sort((a,b)=>b.date.localeCompare(a.date)||b.created-a.created);
if(!list.length){root.innerHTML='<div class="empty">No transactions found for this month.</div>';return}
list.forEach(t=>{
const row=document.createElement("div");row.className="transaction";
const info=document.createElement("div");info.className="txinfo";
const name=document.createElement("div");name.className="txname";name.textContent=t.note||t.category;
const meta=document.createElement("div");meta.className="meta";meta.textContent=t.date+" • "+t.category;
info.append(name,meta);
const right=document.createElement("div");right.style.textAlign="right";
const amount=document.createElement("div");amount.className="txamount "+(t.type==="income"?"income":"expense");amount.textContent=(t.type==="income"?"+":"−")+money(t.amount);
const actions=document.createElement("div");actions.className="txbuttons";
const edit=document.createElement("button");edit.className="secondary";edit.textContent="Edit";edit.addEventListener("click",()=>editTransaction(t.id));
const del=document.createElement("button");del.className="danger";del.textContent="Delete";del.addEventListener("click",()=>deleteTransaction(t.id));
actions.append(edit,del);right.append(amount,actions);row.append(info,right);root.appendChild(row);
});
}
function render(){renderSummary();renderTransactions()}
function clearForm(){
editing=null;$("transactionForm").reset();$("date").value=localDate(new Date());$("type").value="expense";updateCategories();$("formTitle").textContent="Add a Transaction";$("submitBtn").textContent="+ Add Transaction";$("cancelEdit").style.display="none";
}
$("type").addEventListener("change",()=>updateCategories());
$("transactionForm").addEventListener("submit",e=>{
e.preventDefault();
const amount=Number($("amount").value),date=$("date").value;
if(!Number.isFinite(amount)||amount<=0){alert("Please enter an amount greater than zero.");return}
if(!date){alert("Please choose a date.");return}
const old=editing?data.transactions.find(t=>t.id===editing):null;
const item={id:editing||("tx_"+Date.now()+"_"+Math.random().toString(36).slice(2,8)),type:$("type").value,amount:Math.round((amount+Number.EPSILON)*100)/100,category:$("category").value,date,note:$("note").value.trim(),created:old?old.created:Date.now()};
if(old){const idx=data.transactions.findIndex(t=>t.id===editing);data.transactions[idx]=item}else data.transactions.push(item);
if(!save())return;
const newMonth=date.slice(0,7);$("month").value=newMonth;clearForm();render();
});
function editTransaction(id){
const t=data.transactions.find(x=>x.id===id);if(!t)return;
editing=id;$("type").value=t.type;updateCategories(t.category);$("amount").value=t.amount;$("category").value=t.category;$("date").value=t.date;$("note").value=t.note;$("formTitle").textContent="Edit Transaction";$("submitBtn").textContent="Save Changes";$("cancelEdit").style.display="block";
$("entryPanel").scrollIntoView({behavior:"smooth",block:"start"});
}
function deleteTransaction(id){
if(!confirm("Delete this transaction?"))return;
data.transactions=data.transactions.filter(t=>t.id!==id);
if(save())render();
}
$("cancelEdit").addEventListener("click",clearForm);
$("month").addEventListener("change",render);
$("filterType").addEventListener("change",renderTransactions);
$("search").addEventListener("input",renderTransactions);
$("saveBudget").addEventListener("click",()=>{
const value=Number($("budget").value);
if(!Number.isFinite(value)||value<0){alert("Please enter a valid budget of zero or more.");return}
data.budgets[selectedMonth()]=Math.round((value+Number.EPSILON)*100)/100;
if(save())render();
});
function csvCell(v){return '"'+String(v??"").replace(/"/g,'""')+'"'}
$("exportBtn").addEventListener("click",()=>{
const rows=[["Date","Type","Category","Description","Amount (RM)"],...data.transactions.slice().sort((a,b)=>a.date.localeCompare(b.date)).map(t=>[t.date,t.type,t.category,t.note,t.amount])];
const csv="\uFEFF"+rows.map(r=>r.map(csvCell).join(",")).join("\r\n");
const blob=new Blob([csv],{type:"text/csv;charset=utf-8;"});
const url=URL.createObjectURL(blob);const a=document.createElement("a");a.href=url;a.download="expense-tracker-"+selectedMonth()+".csv";document.body.appendChild(a);a.click();a.remove();setTimeout(()=>URL.revokeObjectURL(url),1000);
});
$("shareBtn").addEventListener("click",async()=>{
const list=monthly();const inc=list.filter(t=>t.type==="income").reduce((s,t)=>s+t.amount,0);const exp=list.filter(t=>t.type==="expense").reduce((s,t)=>s+t.amount,0);
const summary="Personal Expense Tracker Pro — "+selectedMonth()+"\nIncome: "+money(inc)+"\nExpenses: "+money(exp)+"\nBalance: "+money(inc-exp)+"\nTransactions: "+list.length;
try{if(navigator.share)await navigator.share({title:"Monthly Money Summary",text:summary});else if(navigator.clipboard&&navigator.clipboard.writeText){await navigator.clipboard.writeText(summary);alert("Summary copied.")}else prompt("Copy your summary:",summary)}catch(e){if(e.name!=="AbortError")alert("Sharing was unavailable.")}
});
$("resetBtn").addEventListener("click",()=>{
if(!confirm("This permanently removes all transactions and budgets saved by this app in this browser. Export a CSV backup first if needed. Continue?"))return;
data={transactions:[],budgets:{}};
try{localStorage.removeItem(KEY);clearForm();render()}catch(e){alert("Unable to clear saved browser data.")}
});
safeLoad();updateCategories();render();
})();
</script>
</body>
</html>
