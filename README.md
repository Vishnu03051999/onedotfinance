<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vishnu Income Tax Calculator</title>
<style>
  :root {
    --primary-color: #0FBF61;
    --accent-color: #14C8C0;
    --bg-color: #F0FFF4;
    --font-family: 'Poppins', sans-serif;
  }
  body {
    font-family: var(--font-family);
    background: var(--bg-color);
    color: var(--primary-color);
    margin: 0; padding: 0;
    display: flex; align-items: center; justify-content: center;
    min-height: 100vh;
  }
  .wizard {
    background: #fff;
    border-radius: 16px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.05);
    width: 100%; max-width: 640px;
    padding: 2rem;
    animation: fadeIn 0.5s ease-in-out;
  }
  .progress-bar-container {
    background-color: #e0e0e0;
    border-radius: 100px;
    height: 10px;
    margin-bottom: 0.5rem;
    overflow: hidden;
  }
  .progress-bar {
    height: 100%;
    background: linear-gradient(to right, var(--primary-color), var(--accent-color));
    width: 0%;
    transition: width 0.3s ease;
  }
  .progress-labels {
    display: flex;
    justify-content: space-between;
    font-size: 0.75rem;
    color: #444;
    margin-bottom: 1.5rem;
  }
  .step { display: none; }
  .step.active { display: block; }
  h2, h3 {
    margin-top: 0;
    color: var(--primary-color);
    font-weight: 600;
  }
  h2 { font-size: 1.6rem; }
  h3 { font-size: 1.2rem; }
  label {
    display: block;
    margin: 1rem 0 .5rem;
    font-weight: 500;
  }
  input, select {
    width: 100%;
    padding: .6rem .75rem;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 8px;
    box-sizing: border-box;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  input:focus, select:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 3px rgba(15, 191, 97, 0.2);
  }
  button {
    margin-top: 1.5rem;
    padding: 0.85rem 1.75rem;
    font-size: 1rem;
    font-weight: 600;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background 0.3s ease;
  }
  button:hover {
    opacity: 0.95;
  }
  .next-btn {
    background: linear-gradient(to right, var(--primary-color), var(--accent-color));
    color: #fff;
    margin-right: .5rem;
  }
  .prev-btn {
    background: #f0f0f0;
    color: #333;
  }
  .section-separator {
    margin: 1.5rem 0;
    border-top: 2px dashed #ddd;
  }
  .error {
    color: #D8000C;
    background-color: #FFBABA;
    padding: 0.5rem;
    font-size: .9rem;
    margin-top: .5rem;
    border-radius: 6px;
  }
  .result-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1rem;
    border-radius: 8px;
    overflow: hidden;
  }
  .result-table th, .result-table td {
    border: 1px solid #ddd;
    padding: .75rem;
    text-align: center;
  }
  .result-table th {
    background: #f8f8f8;
    font-weight: 600;
  }
  .download-btn {
    background: linear-gradient(to right, var(--primary-color), var(--accent-color));
    color: #fff;
    padding: 0.85rem 1.5rem;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    margin-top: 1.5rem;
    font-weight: 600;
  }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
  @media (max-width: 480px) {
    .wizard {
      padding: 1rem;
    }
    h2 {
      font-size: 1.25rem;
    }
    input, select {
      font-size: 0.9rem;
      padding: 0.5rem;
    }
  }
</style>

<div class="wizard">
  <div class="progress-bar-container">
    <div class="progress-bar" id="progressBar"></div>
  </div>
  <div class="progress-labels">
    <span>Basic Info</span>
    <span>Salary</span>
    <span>House</span>
    <span>Other</span>
    <span>80C</span>
    <span>80D</span>
    <span>Review</span>
  </div>
  <div class="wizard">

    <!-- Step 1 -->
    <div id="step1" class="step active">
      <h2>Basic Information</h2>
      <label for="name">Name</label>
      <input type="text" id="name" placeholder="Your Name">
      <label for="ageGroup">Age Group</label>
      <select id="ageGroup">
        <option value="below60">Below 60</option>
        <option value="above60">Above 60</option>
      </select>
      <button class="next-btn" data-next="step2">Next</button>
    </div>

    <!-- Step 2 -->
    <div id="step2" class="step">
      <h2>Income from Salary</h2>
      <label for="grossSalary">Gross Salary (₹) </label>
      <input type="number" id="grossSalary" min="0">
      <label for="professionalTax">Professional Tax (₹)</label>
      <input type="number" id="professionalTax" min="0">
      <label for="hraExempt">HRA Exempt (₹)</label>
      <input type="text" id="hraExempt" readonly>
      <label for="otherExempt">Other Exempted Allowance (₹)</label>
      <input type="number" id="otherExempt" min="0">
      <div class="section-separator"></div>
      <h3>For HRA Calculation</h3>
      <label for="basicPay">Basic Pay (₹)</label>
      <input type="number" id="basicPay" min="0">
      <label for="hraReceived">HRA Received (₹)</label>
      <input type="number" id="hraReceived" min="0">
      <label for="rentPaid">Rent Paid (₹)</label>
      <input type="number" id="rentPaid" min="0">
      <div class="error" id="step2Error"></div>
      <div class="section-separator"></div>
      <h3>Summary</h3>
      <p>Gross Salary: ₹<span id="sumGrossSalary">0.00</span></p>
      <p>Total Exemptions: ₹<span id="sumTotalExemptions">0.00</span></p>
      <button class="prev-btn" data-prev="step1">Back</button>
      <button class="next-btn" id="step2Next" data-next="step3">Next</button>
    </div>

    <!-- Step 3 -->
    <div id="step3" class="step">
      <h2>Income from House Property</h2>
      <label for="rentReceived">Rent Received (₹)</label>
      <input type="number" id="rentReceived" min="0">
      <label for="municipalTax">Municipal Tax Paid (₹)</label>
      <input type="number" id="municipalTax" min="0">
      <label for="stdDeduction">Standard Deduction (30%) (₹)</label>
      <input type="text" id="stdDeduction" readonly>
      <label for="interestLoan">Interest from Housing Loan (₹)</label>
      <input type="number" id="interestLoan" min="0">
      <label for="netHHP">Net House Property Income (₹)</label>
      <input type="text" id="netHHP" readonly>
      <button class="prev-btn" data-prev="step2">Back</button>
      <button class="next-btn" data-next="step4">Next</button>
    </div>

    <!-- Step 4 -->
    <div id="step4" class="step">
      <h2>Income from Other Sources</h2>
      <label for="savingsIncome">Savings Account Interest (₹)</label>
      <input type="number" id="savingsIncome" min="0">
      <label for="fdIncome">Fixed Deposit Interest (₹)</label>
      <input type="number" id="fdIncome" min="0">
      <label for="dividendIncome">Dividend Received (₹)</label>
      <input type="number" id="dividendIncome" min="0">
      <div class="section-separator"></div>
      <h3>Summary</h3>
      <p>Total Other Income: ₹<span id="sumOtherIncome">0.00</span></p>
      <button class="prev-btn" data-prev="step3">Back</button>
      <button class="next-btn" data-next="step5">Next</button>
    </div>

    <!-- Step 5 -->
    <div id="step5" class="step">
      <h2>Chapter VI A Deductions</h2>
      <label for="ded80C">80C Deduction (Max ₹150,000)</label>
      <input type="number" id="ded80C" min="0">
      <label for="ded80CCD">80CCD Deduction (Combined with 80C ≤ ₹200,000)</label>
      <input type="number" id="ded80CCD" min="0">
      <label for="ded80CCD2">80CCD2 Deduction</label>
      <input type="number" id="ded80CCD2" min="0">
      <label for="donationPaid">Donation Paid (₹) (80G 50% deductible)</label>
      <input type="number" id="donationPaid" min="0">
      <div class="section-separator"></div>
      <h3>Tax Saving Interest (80TTA / 80TTB)</h3>
      <label for="ded80TTA">80TTA (Max ₹10,000)</label>
      <input type="text" id="ded80TTA" readonly>
      <label for="ded80TTB">80TTB (Max ₹50,000)</label>
      <input type="text" id="ded80TTB" readonly>
      <div class="section-separator"></div>
      <h3>Summary</h3>
      <p>Total Chapter VI A Deductions: ₹<span id="sumChap6A">0.00</span></p>
      <button class="prev-btn" data-prev="step4">Back</button>
      <button class="next-btn" data-next="step6">Next</button>
    </div>

    <!-- Step 6 -->
    <div id="step6" class="step">
      <h2>80D Deductions</h2>
      <label for="premSelf">Insurance Premium Self (₹)</label>
      <input type="number" id="premSelf" min="0">
      <label for="preventiveCheck">Preventive Health Checkup (₹)</label>
      <input type="number" id="preventiveCheck" min="0">
      <label for="premParent">Parent Insurance (₹)</label>
      <input type="number" id="premParent" min="0">
      <label for="medExpend">Medical Expenditure (₹)</label>
      <input type="number" id="medExpend" min="0">
      <div class="section-separator"></div>
      <h3>Summary</h3>
      <p>Total 80D Deductions: ₹<span id="sum80D">0.00</span></p>
      <button class="prev-btn" data-prev="step5">Back</button>
      <button class="next-btn" data-next="step7">Next</button>
    </div>

    <!-- Step 7 -->
    <div id="step7" class="step">
      <h2>Review & Compare</h2>
      <table class="result-table">
        <thead>
          <tr><th>Particulars</th><th>Old Regime (₹)</th><th>New Regime (₹)</th></tr>
        </thead>
        <tbody id="reviewBody"></tbody>
      </table>
      <div style="margin-top:1rem">
        <h3>Tax Calculation Details</h3>
        <table class="result-table">
          <thead>
            <tr><th>Component</th><th>Old Regime (₹)</th><th>New Regime (₹)</th></tr>
          </thead>
          <tbody id="taxBreakdownBody"></tbody>
        </table>
      </div>
      <button class="prev-btn" data-prev="step6">Back</button>
    </div>
  </div>

  <script>
  document.addEventListener('DOMContentLoaded', () => {
    function showStep(id) {
      document.querySelectorAll('.step').forEach(s => s.classList.remove('active'));
      document.getElementById(id)?.classList.add('active');
    }

    // Navigation
    document.querySelectorAll('.next-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        if (btn.id==='step2Next' && !validateStep2()) return;
        showStep(btn.dataset.next);
        if (btn.dataset.next==='step3') { calculateHRA(); calculateHHP(); }
        if (btn.dataset.next==='step7') renderReview();
      });
    });
    document.querySelectorAll('.prev-btn').forEach(btn => {
      btn.addEventListener('click', () => showStep(btn.dataset.prev));
    });

    // Prevent negative
    document.querySelectorAll('input[type="number"]').forEach(i => {
      i.addEventListener('input', ()=>{ if (+i.value<0) i.value='' });
    });

    // Step 2
    function validateStep2() {
      const g = +document.getElementById('grossSalary').value||0;
      const p = +document.getElementById('professionalTax').value||0;
      const o = +document.getElementById('otherExempt').value||0;
      const b = +document.getElementById('basicPay').value||0;
      const h = +document.getElementById('hraReceived').value||0;
      const e = document.getElementById('step2Error');
      let msg='';
      if (p>30000) msg='Professional Tax cannot exceed ₹30,000.';
      else if (o+b+h>g) msg='Other Exempt + Basic Pay + HRA Received cannot exceed Gross Salary.';
      e.innerText=msg;
      return !msg;
    }
    ['grossSalary','professionalTax','otherExempt'].forEach(id=>
      document.getElementById(id).addEventListener('input', updateSummary)
    );
    ['basicPay','hraReceived','rentPaid'].forEach(id=>
      document.getElementById(id).addEventListener('input', ()=>{ calculateHRA(); updateSummary() })
    );
    function calculateHRA() {
      const b=+document.getElementById('basicPay').value||0;
      const h=+document.getElementById('hraReceived').value||0;
      const r=+document.getElementById('rentPaid').value||0;
      const ex=Math.max(0, Math.min(h, r-0.1*b, 0.5*b));
      document.getElementById('hraExempt').value = ex.toFixed(2);
    }
    function updateSummary() {
      const g=+document.getElementById('grossSalary').value||0;
      const p=+document.getElementById('professionalTax').value||0;
      const h=+document.getElementById('hraExempt').value||0;
      const o=+document.getElementById('otherExempt').value||0;
      document.getElementById('sumGrossSalary').innerText = g.toFixed(2);
      document.getElementById('sumTotalExemptions').innerText = (p+h+o).toFixed(2);
    }

    // Step 3
    ['rentReceived','municipalTax','interestLoan'].forEach(id=>
      document.getElementById(id).addEventListener('input', calculateHHP)
    );
    function calculateHHP() {
      const r=+document.getElementById('rentReceived').value||0;
      let m=+document.getElementById('municipalTax').value||0;
      const i=+document.getElementById('interestLoan').value||0;
      m=Math.min(m,r);
      let net;
      if (r>0) {
        const std=Math.max(0,(r-m)*0.3);
        document.getElementById('stdDeduction').value = std.toFixed(2);
        net = r-m-std-i;
      } else {
        document.getElementById('stdDeduction').value = '0.00';
        net = r-i;
      }
      net = Math.max(net,-200000);
      document.getElementById('netHHP').value = net.toFixed(2);
    }

    // Step 4
    ['savingsIncome','fdIncome','dividendIncome'].forEach(id=>
      document.getElementById(id).addEventListener('input', updateOtherSummary)
    );
    function updateOtherSummary() {
      const s=+document.getElementById('savingsIncome').value||0;
      const f=+document.getElementById('fdIncome').value||0;
      const d=+document.getElementById('dividendIncome').value||0;
      document.getElementById('sumOtherIncome').innerText=(s+f+d).toFixed(2);
    }

    // Step 5
    ['ded80C','ded80CCD','ded80CCD2','donationPaid'].forEach(id=>
      document.getElementById(id).addEventListener('input', updateChap6ASummary)
    );
    ['savingsIncome','fdIncome','ageGroup'].forEach(id=>
      document.getElementById(id).addEventListener('change', updateInterestDeductions)
    );
    function updateChap6ASummary() {
      const raw80C=+document.getElementById('ded80C').value||0;
      let raw80CCD=+document.getElementById('ded80CCD').value||0;
      const rawCCD2=+document.getElementById('ded80CCD2').value||0;
      const don=(+document.getElementById('donationPaid').value||0)*0.5;
      const tta=+document.getElementById('ded80TTA').value||0;
      const ttb=+document.getElementById('ded80TTB').value||0;
      const cap80C=Math.min(raw80C,150000);
      let cap80CCD=Math.min(raw80CCD,200000);
      if(cap80C+cap80CCD>200000) cap80CCD=200000-cap80C;
      const gross=+document.getElementById('grossSalary').value||0;
      const otherE=+document.getElementById('otherExempt').value||0;
      const hraR=+document.getElementById('hraReceived').value||0;
      let base=+document.getElementById('basicPay').value||0;
      if(base===0) base=Math.max(0,gross-otherE-hraR);
      const capOld2=base*0.10;
      const cap80CCD2Old=Math.min(rawCCD2,capOld2);
      const totalOld=cap80C+cap80CCD+cap80CCD2Old+don+tta+ttb;
      document.getElementById('sumChap6A').innerText=totalOld.toFixed(2);
    }
    function updateInterestDeductions() {
      const age=document.getElementById('ageGroup').value;
      const s=+document.getElementById('savingsIncome').value||0;
      const f=+document.getElementById('fdIncome').value||0;
      let tta=0,ttb=0;
      if(age==='below60') tta=Math.min(s,10000);
      else ttb=Math.min(s+f,50000);
      document.getElementById('ded80TTA').value=tta.toFixed(2);
      document.getElementById('ded80TTB').value=ttb.toFixed(2);
      updateChap6ASummary();
    }

    // Step 6
    ['premSelf','preventiveCheck','premParent','medExpend'].forEach(id=>
      document.getElementById(id).addEventListener('input', update80DSummary)
    );
    function update80DSummary() {
      let s=+document.getElementById('premSelf').value||0;
      let p=+document.getElementById('preventiveCheck').value||0;
      s=Math.min(s,25000); p=Math.min(p,5000);
      if(s+p>25000) p=25000-s;
      let par=+document.getElementById('premParent').value||0;
      let m=+document.getElementById('medExpend').value||0;
      par=Math.min(par,50000); m=Math.min(m,50000);
      if(par+m>50000) m=50000-par;
      document.getElementById('sum80D').innerText=(s+p+par+m).toFixed(2);
    }

    // Step 7 & Tax
    function renderReview() {
      calculateHRA(); calculateHHP(); updateOtherSummary();
      updateInterestDeductions(); updateChap6ASummary(); update80DSummary();

      const gross = +document.getElementById('grossSalary').value||0;
      const prof  = +document.getElementById('professionalTax').value||0;
      const hra   = +document.getElementById('hraExempt').value||0;
      const otherE= +document.getElementById('otherExempt').value||0;
      const basicP= +document.getElementById('basicPay').value||0;
      const hhpOld= +document.getElementById('netHHP').value||0;
      const hhpNew= Math.max(0,hhpOld);
      const others=(+document.getElementById('savingsIncome').value||0)
                   +(+document.getElementById('fdIncome').value||0)
                   +(+document.getElementById('dividendIncome').value||0);

      // Gross totals
      let grossOld = Math.max(0, gross+hhpOld+others);
      let grossNew = Math.max(0, gross+hhpNew+others);

      // Std deductions
      const stdOld = gross>0?Math.min(50000,gross):0;
      const stdNew = gross>0?Math.min(75000,gross):0;

      // Exemptions
      let rawEx = prof+hra+otherE;
      const exOld = Math.min(rawEx, Math.max(0, grossOld-stdOld));
      const exNew = 0;

      // Chapter VIA
      let raw6A = +document.getElementById('sumChap6A').innerText||0;
      const cap6AOld = Math.max(0, grossOld-stdOld-exOld);
      const d6Aold = Math.min(raw6A, cap6AOld);
      let capNew2;
      {
        let base = basicP||Math.max(0, gross-otherE-hra);
        capNew2 = Math.min(+document.getElementById('ded80CCD2').value||0, base*0.14);
      }
      const d6Anew = capNew2;
      const d8old = Math.min(+document.getElementById('sum80D').innerText||0, Math.max(0, grossOld-stdOld-exOld-d6Aold));
      const d8new = 0;

      // Taxable incomes
      const netOld = Math.max(0, grossOld-stdOld-exOld-d6Aold-d8old);
      const netNew = Math.max(0, grossNew-stdNew-d6Anew);

      // Review table
      const reviewBody = document.getElementById('reviewBody');
      reviewBody.innerHTML = '';
      [
        ['Salary', gross, gross],
        ['House Property', hhpOld, hhpNew],
        ['Other Sources', others, others],
        ['Gross Total Income', grossOld, grossNew],
        ['Standard Deduction', stdOld, stdNew],
        ['Exemptions', exOld, exNew],
        ['Chapter VI A', d6Aold, d6Anew],
        ['80D', d8old, d8new],
        ['Taxable Income', netOld, netNew]
      ].forEach(row=>{
        const tr=document.createElement('tr');
        row.forEach(v=>{
          const td=document.createElement('td');
          td.innerText=typeof v==='number'?v.toFixed(2):v;
          tr.appendChild(td);
        });
        reviewBody.appendChild(tr);
      });

      // Tax breakdown
      const tb=document.getElementById('taxBreakdownBody');
      tb.innerHTML='';
      if(netOld>0||netNew>0){
        const oldD=computeTaxOld(netOld, document.getElementById('ageGroup').value);
        const newD=computeTaxNew(netNew);
        [
          ['Basic Tax', oldD.basicTax, newD.basicTax],
          ['Surcharge', oldD.surcharge, newD.surcharge],
          ['Rebate',    oldD.rebate,    newD.rebate   ],
          ['Cess (4%)', oldD.cess,      newD.cess     ],
          ['Total Tax Payable', oldD.totalTax, newD.totalTax]
        ].forEach(row=>{
          const tr=document.createElement('tr');
          row.forEach(v=>{
            const td=document.createElement('td');
            td.innerText=typeof v==='number'?v.toFixed(2):v;
            tr.appendChild(td);
          });
          tb.appendChild(tr);
        });
      } else {
        const tr=document.createElement('tr');
        const td=document.createElement('td');
        td.colSpan=3; td.innerText='No tax due';
        tr.appendChild(td); tb.appendChild(tr);
      }
    }

    // Tax functions
    function computeTaxOld(income, age) {
      let tax=0;
      if(age==='below60'){
        if(income>1000000) tax=112500+(income-1000000)*0.30;
        else if(income>500000) tax=12500+(income-500000)*0.20;
        else if(income>250000) tax=(income-250000)*0.05;
      } else {
        if(income>1000000) tax=110000+(income-1000000)*0.30;
        else if(income>500000) tax=10000+(income-500000)*0.20;
        else if(income>300000) tax=(income-300000)*0.05;
      }
      let sRate=0;
      if(income>50000000) sRate=0.37;
      else if(income>20000000) sRate=0.25;
      else if(income>10000000) sRate=0.15;
      else if(income>5000000 ) sRate=0.10;
      const surcharge=tax*sRate;
      const rebate   = income<=500000?tax:0;
      const base     = tax+surcharge-rebate;
      const cess     = base*0.04;
      return { basicTax:tax, surcharge, rebate, cess, totalTax:base+cess };
    }
    function computeTaxNew(income) {
      let tax=0;
      if(income>1500000) tax=140000+(income-1500000)*0.30;
      else if(income>1200000) tax=80000+(income-1200000)*0.20;
      else if(income>1000000) tax=50000+(income-1000000)*0.15;
      else if(income>700000) tax=20000+(income-700000)*0.10;
      else if(income>300000) tax=(income-300000)*0.05;
      let sRate=0;
      if(income>20000000) sRate=0.25;
      else if(income>10000000) sRate=0.15;
      else if(income>5000000 ) sRate=0.10;
      const surcharge=tax*sRate;
      const rebate   = income<=700000?tax:0;
      const base     = tax+surcharge-rebate;
      const cess     = base*0.04;
      return { basicTax:tax, surcharge, rebate, cess, totalTax:base+cess };
    }

    window.renderReview = renderReview;
  });
  </script>
    <button class="download-btn" onclick="downloadPDF()">Download PDF</button>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
<script>
  document.addEventListener('DOMContentLoaded', () => {
    const steps = document.querySelectorAll('.step');
    const progressBar = document.getElementById('progressBar');
    function updateProgressBar() {
      const activeIndex = Array.from(steps).findIndex(s => s.classList.contains('active'));
      const progress = ((activeIndex + 1) / steps.length) * 100;
      progressBar.style.width = progress + '%';
    }
    const observer = new MutationObserver(updateProgressBar);
    steps.forEach(step => observer.observe(step, { attributes: true, attributeFilter: ['class'] }));
    updateProgressBar();
  });

  function downloadPDF() {
    const element = document.querySelector('.wizard');
    const opt = {
      margin:       0.5,
      filename:     'Tax_Calculation_Summary.pdf',
      image:        { type: 'jpeg', quality: 0.98 },
      html2canvas:  { scale: 2 },
      jsPDF:        { unit: 'in', format: 'letter', orientation: 'portrait' }
    };
    html2pdf().set(opt).from(element).save();
  }
</script>
</body>
</html>
