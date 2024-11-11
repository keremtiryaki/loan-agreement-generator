<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
interface LoanData {
  lenderName: string
  lenderAddress: string
  borrowerName: string
  borrowerAddress: string
  loanAmount: string
  repaymentAmount: string
  disbursementMethod: string
  startDate: string
  lenderFont: string
  borrowerFont: string
  lenderCountry: string
  borrowerCountry: string
  interestRate: string
  repaymentDays: string
  lenderBusinessName: string
  borrowerBusinessName: string
}

// Import section


// Replace localStorage loading with hash loading
const getDataFromHash = () => {
  try {
    const hash = decodeURIComponent(window.location.hash.slice(1))
    return hash ? JSON.parse(atob(hash)) : null
  } catch (e) {
    console.error('Error parsing hash data:', e)
    return null
  }
}

const savedData = getDataFromHash()
const initialData = savedData || {
  lenderName: 'name surname',
  lenderAddress: 'address',
  borrowerName: 'borrower name surname',
  borrowerAddress: 'address',
  loanAmount: '$20,000 USD',
  repaymentAmount: '$25,000 USD',
  disbursementMethod: 'Wire Transfer',
  startDate: new Date().toISOString().split('T')[0],
  lenderFont: 'Great Vibes',
  borrowerFont: 'Pinyon Script',
  lenderCountry: 'United States',
  borrowerCountry: 'United Kingdom',
  interestRate: '15% per month',
  repaymentDays: '60 days',
  lenderBusinessName: 'lender business name',
  borrowerBusinessName: 'borrower business name'
}

const loanData = ref<LoanData>(initialData)

// Replace localStorage watch with hash watch
watch(loanData, (newValue) => {
  const jsonStr = JSON.stringify(newValue)
  const base64 = btoa(jsonStr)
  window.location.hash = encodeURIComponent(base64)
}, { deep: true })

// Add hash change listener
onMounted(() => {
  window.addEventListener('hashchange', () => {
    const newData = getDataFromHash()
    if (newData) {
      loanData.value = newData
    }
  })
})

const signatureFonts = [
  { label: 'Dancing Script', value: 'Dancing Script' },
  { label: 'Sacramento', value: 'Sacramento' },
  { label: 'Parisienne', value: 'Parisienne' },
  { label: 'Great Vibes', value: 'Great Vibes' },
  { label: 'Alex Brush', value: 'Alex Brush' },
  { label: 'Mr Dafoe', value: 'Mr Dafoe' },
  { label: 'Mrs Saint Delafield', value: 'Mrs Saint Delafield' },
  { label: 'Allura', value: 'Allura' },
  { label: 'Ruthie', value: 'Ruthie' },
  { label: 'Petit Formal Script', value: 'Petit Formal Script' },
  { label: 'Tangerine', value: 'Tangerine' },
  { label: 'Pinyon Script', value: 'Pinyon Script' },
  { label: 'Rouge Script', value: 'Rouge Script' }
]

const printAgreement = () => {
  window.print()
}

const shareLinkBtnText = ref('Copy Share Link');
const copyShareLink = () => {
  navigator.clipboard.writeText(window.location.href);
  shareLinkBtnText.value = 'Copied!'
  setTimeout(() => {
    shareLinkBtnText.value = 'Copy Share Link'
  }, 2000)
}
</script>

<template>
  <div class="loan-form">
    <div class="form-container no-print">
      <form>
        <div class="form-group">
          <label>Lender's Full Name:</label>
          <input type="text" v-model="loanData.lenderName" >
        </div>

        <div class="form-group">
          <label>Lender's Business Name:</label>
          <input type="text" v-model="loanData.lenderBusinessName" >
        </div>

        <div class="form-group">
          <label>Lender's Address:</label>
          <input type="text" v-model="loanData.lenderAddress" >
        </div>

        <div class="form-group">
          <label>Lender's Country:</label>
          <input type="text" v-model="loanData.lenderCountry" >
        </div>

        <div class="form-group">
          <label>Lender's Signature Font:</label>
          <select v-model="loanData.lenderFont">
            <option v-for="font in signatureFonts" :key="font.value" :value="font.value">{{ font.label }}</option>
          </select>
        </div>

        <div class="form-group">
          <label>Borrower Name:</label>
          <input type="text" v-model="loanData.borrowerName" >
        </div>

        <div class="form-group">
          <label>Borrower's Business Name:</label>
          <input type="text" v-model="loanData.borrowerBusinessName" >
        </div>

        <div class="form-group">
          <label>Borrower's Address:</label>
          <input type="text" v-model="loanData.borrowerAddress" >
        </div>

        <div class="form-group">
          <label>Borrower's Country:</label>
          <input type="text" v-model="loanData.borrowerCountry" >
        </div>

        <div class="form-group">
          <label>Borrower's Signature Font:</label>
          <select v-model="loanData.borrowerFont">
            <option v-for="font in signatureFonts" :key="font.value" :value="font.value">{{ font.label }}</option>
          </select>
        </div>

        <div class="form-group">
          <label>Loan Amount:</label>
          <input type="text" v-model="loanData.loanAmount" >
        </div>

        <div class="form-group">
          <label>Repayment Amount:</label>
          <input type="text" v-model="loanData.repaymentAmount" >
        </div>

        <div class="form-group">
          <label>Interest Rate (% per month if fails to repay):</label>
          <input type="text" v-model="loanData.interestRate" >
        </div>

        <div class="form-group">
          <label>Repayment Days:</label>
          <input type="text" v-model="loanData.repaymentDays" >
        </div>

        <div class="form-group">
          <label>Disbursement Method:</label>
          <input type="text" v-model="loanData.disbursementMethod" >
        </div>

        <div class="form-group">
          <label>Agreement Date:</label>
          <input type="text" v-model="loanData.startDate" >
        </div>
      </form>
    </div>

    <div class="agreement">
      <div class="button-group">
        <button type="button" @click="printAgreement" class="print-button">Print Agreement</button>
        <br>
        <button type="button" @click="copyShareLink" class="share-button">{{ shareLinkBtnText }}</button>
      </div>
      <div class="agreement-header">
        <h2>LOAN AGREEMENT</h2>
      </div>
      <div class="agreement-content">
        <p>THIS LOAN AGREEMENT (this "Agreement") dated <span class="underline">{{ loanData.startDate }}</span></p>
        
        <p>BETWEEN:</p>
        
        <p><span class="underline">{{ loanData.lenderName }}</span>
        <template v-if="loanData.lenderBusinessName"> d/b/a <span class="underline">{{ loanData.lenderBusinessName }}</span></template>
        of <span class="underline">{{ loanData.lenderAddress }}</span><br>(the "Lender")</p>
        <p>OF THE FIRST PART</p>
        
        <p>AND</p>
        
        <p><span class="underline">{{ loanData.borrowerName }}</span>
        <template v-if="loanData.borrowerBusinessName"> d/b/a <span class="underline">{{ loanData.borrowerBusinessName }}</span></template>
        of <span class="underline">{{ loanData.borrowerAddress }}</span><br>(the "Borrower")</p>
        <p>OF THE SECOND PART</p>
        
        <p>IN CONSIDERATION OF the Lender loaning certain monies (the "Loan") to the Borrower, and the Borrower repaying the Loan to the Lender, the parties agree to keep, perform and fulfill the promises and conditions set out in this Agreement:</p>

        <h3>1. Loan Amount & Interest</h3>
        <p>The Lender promises to loan <span class="underline">{{ loanData.loanAmount }}</span> to the Borrower and the Borrower promises to repay this principal amount to the Lender, with interest payable on the unpaid principal at the rate of <span class="underline">{{ loanData.interestRate }}</span>, calculated yearly not in advance.</p>

        <h3>2. Payment</h3>
        <p>The Borrower agrees to repay the Lender a total amount of <span class="underline">{{ loanData.repaymentAmount }}</span> within <span class="underline">{{ loanData.repaymentDays }}</span> from the date of disbursement.</p>
        <p>At any time while not in default under this Agreement, the Borrower may make lump sum payments or pay the outstanding balance then owing under this Agreement to the Lender without further bonus or penalty.</p>

        <h3>3. Default</h3>
        <p>Notwithstanding anything to the contrary in this Agreement, if the Borrower defaults in the performance of any obligation under this Agreement, then the Lender may declare the principal amount owing and interest due under this Agreement at that time to be immediately due and payable.</p>

        <h3>4. Disbursement Method</h3>
        <p>The Lender will transfer the loan amount via <span class="underline">{{ loanData.disbursementMethod }}</span> to the account details provided by the Borrower.</p>

        <h3>5. Governing Law</h3>
        <p>This Agreement will be construed in accordance with and governed by the laws of 
          <span class="underline">{{ loanData.lenderCountry }}</span>
          and
          <span class="underline">{{ loanData.borrowerCountry }}</span>
          .</p>

        <h3>6. Costs</h3>
        <p>The Borrower shall be liable for all costs, expenses and expenditures including, without limitation, the complete legal costs incurred by enforcing this Agreement as a result of any default by the Borrower, and such costs will be added to the principal then outstanding and shall be due and payable immediately upon demand of the Lender.</p>

        <h3>7. Binding Effect</h3>
        <p>This Agreement will pass to the benefit of and be binding upon the respective heirs, executors, administrators, successors and permitted assigns of the Borrower and Lender. The Borrower waives presentment for payment, notice of non-payment, protest, and notice of protest.</p>

        <h3>8. Amendments</h3>
        <p>This Agreement may only be amended or modified by a written instrument executed by both the Borrower and the Lender.</p>

        <h3>9. Severability</h3>
        <p>The clauses and paragraphs contained in this Agreement are intended to be read and construed independently of each other. If any term, covenant, condition or provision of this Agreement is held by a court of competent jurisdiction to be invalid, void or unenforceable, it is the parties' intent that such provision be reduced in scope by the court only to the extent deemed necessary by that court to render the provision reasonable and enforceable and the remainder of the provisions of this Agreement will in no way be affected, impaired or invalidated as a result.</p>

        <h3>10. General Provisions</h3>
        <p>Headings are inserted for the convenience of the parties only and are not to be considered when interpreting this Agreement. Words in the singular mean and include the plural and vice versa. Words in the masculine mean and include the feminine and vice versa.</p>

        <h3>11. Entire Agreement</h3>
        <p>This Agreement constitutes the entire agreement between the parties and there are no further items or provisions, either oral or otherwise.</p>
        
        <hr>

        <div>
          <p>Lender's Signature: 
              <br>
              <span :style="{ fontFamily: loanData.lenderFont, fontSize: '2rem' }">{{ loanData.lenderName }}</span>
          </p>
          <p>Date: <span class="underline">{{ loanData.startDate }}</span></p>
        </div>
        <br>
        <hr>
        <br>
        <div>
          <p>Borrower's Signature: 
              <br>
              <span :style="{ fontFamily: loanData.borrowerFont, fontSize: '2rem' }">{{ loanData.borrowerName }}</span>
          </p>
          <p>Date: <span class="underline">{{ loanData.startDate }}</span></p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Great+Vibes&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Mr+Dafoe&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Mrs+Saint+Delafield&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Ruthie&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Dancing+Script&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Sacramento&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Parisienne&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Alex+Brush&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Allura&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Petit+Formal+Script&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Tangerine&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Pinyon+Script&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Rouge+Script&display=swap');

.underline {
  text-decoration: underline;
}

.loan-form {
  display: flex;
  gap: 2rem;
  background: #fff;
  padding: 0rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: 100vh;
  overflow: hidden;
}

.form-container {
  flex: 1;
  max-width: 40%;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 1rem;
}

.agreement {
  flex: 1;
  max-width: 50%;
  padding: 0 1rem;
  border-left: 1px solid #ddd;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.agreement-header {
  flex-shrink: 0;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: bold;
}

input, select {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

button {
  background-color: #42b883;
  color: white;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  margin-top: 1rem;
}

button:hover {
  background-color: #3aa876;
}

.agreement h2 {
  color: #2c3e50;
  margin-bottom: 1rem;
}

.agreement-content {
  line-height: 1.6;
  overflow-y: auto;
  flex-grow: 1;
  padding-right: 1rem;
}

.signatures {
  margin-top: 2rem;
  display: flex;
  justify-content: space-between;
}

.signature-block {
  flex: 1;
  margin: 0 1rem;
}

.signature-block span {
  font-size: 1.5rem;
}

ul {
  margin: 0;
  padding-left: 2rem;
}

@media print {
  .no-print {
    display: none;
  }
  
  .loan-form {
    display: block;
    padding: 0;
    box-shadow: none;
    height: auto;
    overflow: visible;
  }
  
  .agreement {
    max-width: 100%;
    padding: 0;
    border: none;
    height: auto;
  }
  
  .agreement-content {
    overflow: visible;
  }

  .button-group {
    display: none;
  }
}

.print-button {
  background-color: #4a5568;
  margin-bottom: 1rem;
}

.print-button:hover {
  background-color: #2d3748;
}
</style>