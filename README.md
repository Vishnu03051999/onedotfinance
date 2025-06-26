<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Income Tax Calculator</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 2rem auto; background: #f0f8f5; color: #064635; }
    h1, h2 { color: #056d4a; text-align: center; }
    .logo { display: block; max-width: 150px; margin: 0 auto 1rem; }
    .step { display: none; }
    .step.active { display: block; }
    label { display: block; margin: .5rem 0 .2rem; }
    input, select { width: 100%; padding: .6rem; margin-bottom: 1rem; border: 1px solid #9ad3bc; border-radius: 4px; }
    .error { color: red; font-size: 0.9em; margin-top: -0.5rem; margin-bottom: 0.5rem; }
    button { padding: .6rem 1.2rem; background: #04a77b; color: #fff; border: none; border-radius: 4px; cursor: pointer; }
    button:hover { background: #037f60; }
    .nav-buttons { display: flex; justify-content: space-between; gap: 1rem; margin-top: 1rem; }
    .summary { background: #e3f6f5; padding: 1rem; margin-top: 1rem; border: 1px solid #9ad3bc; border-radius: 4px; }
    .result, .compare { font-size: 1.1rem; font-weight: bold; margin-top: 1rem; color: #056d4a; text-align: center; }
  </style>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.9.2/html2pdf.bundle.min.js"></script>
</head>
<body>
  <img src="/mnt/data/ONE.FINANCEORIGINAL.png" alt="One Finance Logo" class="logo" />
  <h1>Income Tax Calculator</h1>
  <p style="text-align:center;">(For F.Y 2024-2025 / A.Y 2025-2026)</p>
  <form id="taxForm">
    <!-- Steps 1-10 -->
    <section class="step active" data-step="1">
      <h2>1. Basic Info</h2>
      <label>Full Name<input type="text" id="name" required /></label>
      <label>Age Group<select id="ageGroup"><option value="below60">Below 60</option><option value="sixtyplus">60 & Above</option></select></label>
      <label>Tax Regime<select id="regime"><option value="old">Old Regime</option><option value="new">New Regime</option></select></label>
      <div class="nav-buttons"><div></div><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="2">
      <h2>2. Salary Income</h2>
      <label>Have you received salary?<select id="hasSalary"><option value="yes">Yes</option><option value="no">No</option></select></label>
      <div id="salaryFields">
        <label>Gross Salary (Rs.)<input type="number" id="grossSalary" min="0" /></label>
        <label>Basic Pay (Rs.)<input type="number" id="basicPay" min="0" /></label>
        <label>HRA Received (Rs.)<input type="number" id="hraReceived" min="0" /></label>
        <label>Rent Paid (Rs.)<input type="number" id="rentPaid" min="0" /></label>
        <label>Standard Deduction (Rs.)<input type="number" id="stdDeduction" min="0" value="50000" /></label>
        <div class="error" id="errStdDeduction"></div>
        <label>Professional Tax (Rs.)<input type="number" id="profTax" min="0" value="0" /></label>
        <div class="summary" id="salarySummary">Net Salary Income: Rs. 0</div>
      </div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="3">
      <h2>3. Business Income</h2>
      <label>Have you received business income?<select id="hasBusiness"><option value="yes">Yes</option><option value="no">No</option></select></label>
      <div id="businessFields">
        <label>Business Income (Rs.)<input type="number" id="businessIncome" min="0" /></label>
        <div class="summary" id="businessSummary">Net Business Income: Rs. 0</div>
      </div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="4">
      <h2>4. House Property</h2>
      <label>Have you received rent or paid home-loan?<select id="hasHouseProperty"><option value="yes">Yes</option><option value="no">No</option></select></label>
      <div id="houseFields">
        <label>Rent Received (Rs.)<input type="number" id="rentReceived" min="0" /></label>
        <label>Home Loan Interest Paid (Rs.)<input type="number" id="homeLoanInterest" min="0" /></label>
        <div class="summary" id="houseSummary">Net House Property Income: Rs. 0</div>
      </div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="5">
      <h2>5. Other Sources</h2>
      <label>Have you received interest/dividend?<select id="hasOther"><option value="yes">Yes</option><option value="no">No</option></select></label>
      <div id="otherFields">
        <label>Savings Bank Interest (Rs.)<input type="number" id="savingsInterest" min="0" /></label>
        <label>Fixed Deposit Interest (Rs.)<input type="number" id="fdInterest" min="0" /></label>
        <label>Dividend Received (Rs.)<input type="number" id="dividendReceived" min="0" /></label>
        <div class="summary" id="otherSummary">Net Other Income: Rs. 0</div>
      </div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="6">
      <h2>6. Capital Gains</h2>
      <label>Have you sold shares?<select id="hasCG"><option value="yes">Yes</option><option value="no">No</option></select></label>
      <div id="cgFields">
        <label>STCG (<12m) (Rs.)<input type="number" id="stcgAmount" min="0" /></label>
        <label>LTCG (>12m) (Rs.)<input type="number" id="ltcgAmount" min="0" /></label>
        <div class="summary" id="cgSummary">STCG: Rs. 0, LTCG: Rs. 0</div>
      </div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="7">
      <h2>7. 80C & 80CCD(1B)</h2>
      <label>Have you invested in eligible instruments?<select id="has80C"><option value="yes">Yes</option><option value="no">No</option></select></label>
      <div id="field80C">
        <label>80C Deduction (Max ₹150k)<input type="number" id="ded80C" min="0" max="150000" /></label>
        <label>80CCD(1B) Deduction (Max ₹50k)<input type="number" id="ded80CCD1B" min="0" max="50000" /></label>
        <div class="summary" id="summary80C">Total: Rs. 0</div>
      </div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="8">
      <h2>8. 80D (Health Insurance)</h2>
      <label>Have you paid health insurance?<select id="has80D"><option value="yes">Yes</option><option value="no">No</option></select></label>
      <div id="field80D">
        <label>Health Check-up (Max ₹5k)<input type="number" id="selfHealthCheck" min="0" /></label>
        <label>Premium – Self & Family (Max ₹25k/50k SC)<input type="number" id="selfInsurance" min="0" /></label>
        <label>Senior Citizen Premium (Max ₹50k)<input type="number" id="seniorInsurance" min="0" /></label>
        <label>Medical Expenditure (Max ₹50k)<input type="number" id="medicalExp" min="0" /></label>
        <div class="summary" id="summary80D">Total 80D: Rs. 0</div>
      </div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" class="nextBtn">Next →</button></div>
    </section>
    <section class="step" data-step="9">
      <h2>9. Review &amp; Compute</h2>
      <div class="summary" id="finalSummary"></div>
      <div class="result" id="taxResult"></div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" id="computeBtn">Compute &amp; Compare →</button></div>
    </section>
    <section class="step" data-step="10">
      <h2>10. Compare Regimes</h2>
      <div class="compare" id="compareSummary"></div>
      <div class="nav-buttons"><button type="button" class="backBtn">← Back</button><button type="button" id="finishBtn">Finish</button></div>
    </section>
  </form>
  <script>
    document.addEventListener('DOMContentLoaded', () => {
      const steps = Array.from(document.querySelectorAll('.step'));
      let current = 1;
      const data = {};
      const round10 = v => Math.round(v/10)*10;
      const toNum = id => parseFloat(document.getElementById(id).value) || 0;
      const slabs = { old: [250000,500000,1000000], new: [300000,600000,900000,1200000,1500000] };
      const rates = { old: [0,0.05,0.2,0.3], new: [0,0.05,0.1,0.15,0.2,0.3] };
      function calcTax(i,r){ let t=0,p=0; rates[r].forEach((rt,j)=>{ const m=slabs[r][j]||Infinity; if(i>m){t+=(m-p)*rt; p=m;} else {t+=(i-p)*rt; p=i;} }); if(r==='new'&&i<=700000) t=Math.max(0,t-12500); return round10(t*1.04); }
      function showStep(i){ steps.forEach(s=>s.classList.remove('active')); steps[i-1].classList.add('active'); }
      function collect(i){ switch(i){ case 1: data.name=document.getElementById('name').value; data.age=document.getElementById('ageGroup').value; data.regime=document.getElementById('regime').value; document.getElementById('stdDeduction').value=data.regime==='new'?75000:50000; break; case 2: if(document.getElementById('hasSalary').value==='yes'){ let gross=toNum('grossSalary'), std=toNum('stdDeduction'), prof=toNum('profTax'), basic=toNum('basicPay'), hraR=toNum('hraReceived'), rent=toNum('rentPaid'); let hra=round10(Math.max(0,Math.min(hraR,rent-0.1*basic,0.5*basic))); data.salary=round10(gross-Math.min(std,gross)-prof-hra); document.getElementById('salarySummary').innerHTML=`Net Salary: Rs. ${data.salary}<br>HRA Exempt: Rs. ${hra}`; } else data.salary=0; break; case 3: data.business=document.getElementById('hasBusiness').value==='yes'?round10(toNum('businessIncome')):0; document.getElementById('businessSummary').textContent=`Net Business: Rs. ${data.business}`; break; case 4: data.house=document.getElementById('hasHouseProperty').value==='yes'?round10(toNum('rentReceived')-toNum('homeLoanInterest')):0; document.getElementById('houseSummary').textContent=`Net House: Rs. ${data.house}`; break; case 5: data.other=document.getElementById('hasOther').value==='yes'?round10(toNum('savingsInterest')+toNum('fdInterest')+toNum('dividendReceived')):0; document.getElementById('otherSummary').textContent=`Net Other: Rs. ${data.other}`; break; case 6: if(document.getElementById('hasCG').value==='yes'){ data.stcg=round10(toNum('stcgAmount')); data.ltcg=round10(toNum('ltcgAmount')); } else{data.stcg=0;data.ltcg=0;} document.getElementById('cgSummary').textContent=`STCG: Rs. ${data.stcg}, LTCG: Rs. ${data.ltcg}`; break; case 7: let v1=toNum('ded80C'), v2=toNum('ded80CCD1B'); data.d80C=round10(Math.min(v1,150000)); data.d80CCD=round10(Math.min(v2,50000)); document.getElementById('summary80C').textContent=`Total: Rs. ${data.d80C+data.d80CCD}`; break; case 8: let hc=toNum('selfHealthCheck'), ip=toNum('selfInsurance'), sip=toNum('seniorInsurance'), me=toNum('medicalExp'); data.d80D=round10(Math.min(hc,5000)+Math.min(ip,data.age==='sixtyplus'?50000:25000)+Math.min(sip,50000)+Math.min(me,50000)); document.getElementById('summary80D').textContent=`Total 80D: Rs. ${data.d80D}`; break; }
      }
      function finalize(){ let gross=round10(data.salary+data.business+data.house+data.other+data.stcg+data.ltcg); let ded=round10(data.d80C+data.d80CCD+data.d80D); let net=round10(gross-ded); document.getElementById('finalSummary').innerHTML=`<p>Gross: Rs. ${gross}</p><p>Deduction: Rs. ${ded}</p><p>Net Taxable: Rs. ${net}</p>`; data.net=net; }
      document.querySelectorAll('.nextBtn').forEach((btn,i)=>{ btn.addEventListener('click',()=>{ let step=i+1; collect(step); if(step===9) finalize(); if(step<10){ current=step+1; showStep(current);} }); });
      // Real-time summary updates for steps 2-8
      const realTimeMap = {
        2: ['grossSalary','basicPay','hraReceived','rentPaid','stdDeduction','profTax'],
        3: ['businessIncome'],
        4: ['rentReceived','homeLoanInterest'],
        5: ['savingsInterest','fdInterest','dividendReceived'],
        6: ['stcgAmount','ltcgAmount'],
        7: ['ded80C','ded80CCD1B'],
        8: ['selfHealthCheck','selfInsurance','seniorInsurance','medicalExp']
      };
      Object.entries(realTimeMap).forEach(([step, ids]) => {
        ids.forEach(id => {
          const el = document.getElementById(id);
          if(el) el.addEventListener('input', () => collect(Number(step)));
        });
      });
      document.querySelectorAll('.backBtn').forEach((btn,i)=>{ btn.addEventListener('click',()=>{ if(current>1){ current--; showStep(current);} }); });
      document.getElementById('computeBtn').addEventListener('click',()=>{ collect(9); finalize(); let oldT=calcTax(data.net,'old'), newT=calcTax(data.net,'new'); document.getElementById('taxResult').textContent=`Old: Rs. ${oldT}, New: Rs. ${newT}`; document.getElementById('compareSummary').innerHTML=`<p>Old: Rs. ${oldT}</p><p>New: Rs. ${newT}</p><p>Diff: Rs. ${Math.abs(oldT-newT)}</p>`; current=10; showStep(10); });
      document.getElementById('finishBtn').addEventListener('click',()=>{ current=1; showStep(1); });
      showStep(current);
    });
  </script>
</body>
</html>
