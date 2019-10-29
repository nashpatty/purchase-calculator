<template>
  <div class="container">
    <div class="col">
      <div class="row">
        <div class="col">
          <p>
            Figure out the maximum home price you can afford before you start
            house hunting.
          </p>
          <p>
            Note: Calculators display default values. Enter new figures to
            override.
          </p>
        </div>
      </div>
      <div class="row">
        <div id="input" class="col-6">
          <form action>
            <div id="gross-income-input-group" class="form-group">
              <label for="gross-income-input-field">Gross Income</label>
              <div class="input-group mb-2">
                <div class="input-group-prepend">
                  <div class="input-group-text">$</div>
                </div>
                <input
                  id="gross-income-input-field"
                  v-model.number="grossIncome"
                  type="number"
                  class="form-control"
                />
                <div class="input-group-append">
                  <div class="input-group-text">/mo</div>
                </div>
                <input
                  id="gross-income-input-range"
                  v-model.number="grossIncome"
                  type="range"
                  class="custom-range"
                  min="500"
                  max="50000"
                  value="4000"
                />
              </div>
            </div>
            <div id="car-loan-input-group" class="form-group">
              <label for="car-loan-input-field">Car Loan</label>
              <div class="input-group mb-2">
                <div class="input-group-prepend">
                  <div class="input-group-text">$</div>
                </div>
                <input
                  id="car-loan-input-field"
                  v-model.number="carLoan"
                  type="number"
                  class="form-control"
                />
                <div class="input-group-append">
                  <div class="input-group-text">/mo</div>
                </div>
                <input
                  id="car-loan-input-range"
                  v-model.number="carLoan"
                  type="range"
                  class="custom-range"
                  min="0"
                  max="5000"
                />
              </div>
            </div>
            <div id="credit-card-input-group" class="form-group">
              <label for="credit-card-input-field">Credit Cards</label>
              <div class="input-group mb-2">
                <div class="input-group-prepend">
                  <div class="input-group-text">$</div>
                </div>
                <input
                  id="credit-card-input-field"
                  v-model.number="creditCard"
                  type="number"
                  class="form-control"
                />
                <div class="input-group-append">
                  <div class="input-group-text">/mo</div>
                </div>
                <input
                  id="credit-card-input-range"
                  v-model.number="creditCard"
                  type="range"
                  class="custom-range"
                  min="0"
                  max="5000"
                />
              </div>
            </div>
            <div id="other-debts-input-group" class="form-group">
              <label for="other-debts-input-field"
                >Student Loan/Other Debts</label
              >
              <div class="input-group mb-2">
                <div class="input-group-prepend">
                  <div class="input-group-text">$</div>
                </div>
                <input
                  id="other-debts-input-field"
                  v-model.number="studentLoan"
                  type="number"
                  class="form-control"
                />
                <div class="input-group-append">
                  <div class="input-group-text">/mo</div>
                </div>
                <input
                  id="other-debts-input-range"
                  v-model.number="studentLoan"
                  type="range"
                  class="custom-range"
                  min="0"
                  max="5000"
                />
              </div>
            </div>
            <div id="down-payment-input-group" class="form-group">
              <label for="down-payment-input-field">Down Payment</label>
              <div class="input-group mb-2">
                <input
                  id="down-payment-input-field"
                  v-model.number="downPaymentPercent"
                  type="number"
                  class="form-control"
                />
                <div class="input-group-append">
                  <div class="input-group-text">%</div>
                </div>
                <input
                  id="down-payment-input-range"
                  v-model.number="downPaymentPercent"
                  type="range"
                  class="custom-range"
                  min="3"
                  max="50"
                />
              </div>
            </div>
            <div id="interest-rate-input-group" class="form-group">
              <label for="interest-rate-input-field">Interest Rate</label>
              <div class="input-group mb-2">
                <input
                  id="interest-rate-input-field"
                  v-model.number="interestRatePercent"
                  type="number"
                  class="form-control"
                />
                <div class="input-group-append">
                  <div class="input-group-text">%</div>
                </div>
                <input
                  id="interest-rate-input-range"
                  v-model.number="interestRatePercent"
                  type="range"
                  class="custom-range"
                  min="2.5"
                  max="11"
                  step="0.25"
                />
              </div>
            </div>
            <div id="term-input-group" class="form-group">
              <label for="term-input-select">Term</label>
              <div class="input-group mb-2">
                <select
                  id="term-input-select"
                  v-model.number="term"
                  class="form-control"
                >
                  <option>10</option>
                  <option>15</option>
                  <option>20</option>
                  <option>25</option>
                  <option selected>30</option>
                </select>
                <div class="input-group-append">
                  <div class="input-group-text">years</div>
                </div>
                <input
                  id="term-input-range"
                  v-model.number="term"
                  type="range"
                  class="custom-range"
                  min="10"
                  max="30"
                  step="5"
                />
              </div>
            </div>
            <div v-if="!simpleView">
              <div id="hoi-input-group" class="form-group">
                <label for="hoi-input-field">Homeowner's Insurance</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    id="hoi-input-field"
                    v-model.number="hoi"
                    type="number"
                    class="form-control"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/yr</div>
                  </div>
                  <input
                    id="hoi-input-range"
                    v-model.number="hoi"
                    type="range"
                    class="custom-range"
                    min="0"
                    max="4000"
                  />
                </div>
              </div>
              <div id="hoa-input-group" class="form-group">
                <label for="hoa-input-field"
                  >Homeowner's Association (HOA) or Condo Fees</label
                >
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    id="hoa-input-field"
                    v-model.number="hoa"
                    type="number"
                    class="form-control"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/mo</div>
                  </div>
                  <input
                    id="hoa-input-range"
                    v-model.number="hoa"
                    type="range"
                    class="custom-range"
                    min="5"
                    max="600"
                  />
                </div>
              </div>
              <div id="property-tax-input-group" class="form-group">
                <label for="property-tax-input-field">Property Taxes</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    id="property-tax-input-field"
                    v-model.number="propertyTax"
                    type="number"
                    class="form-control"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/yr</div>
                  </div>
                  <input
                    id="property-tax-input-range"
                    v-model.number="propertyTax"
                    type="range"
                    class="custom-range"
                    min="400"
                    max="25000"
                  />
                </div>
              </div>
            </div>
            <div id="form-mod-group" class="form-group">
              <div class="row">
                <div class="col">
                  <button
                    type="button"
                    class="btn btn-secondary btn-block"
                    @click="simpleView = !simpleView"
                  >
                    {{ simpleView ? "Advanced View" : "Simple View" }}
                  </button>
                </div>
                <div class="col">
                  <button
                    type="button"
                    class="btn btn-secondary btn-block"
                    @click="reset"
                  >
                    Reset &#8634;
                  </button>
                </div>
              </div>
            </div>
            <div id="submit-form-group" class="form-group">
              <button class="btn btn-block btn-primary" type="submit">
                See Results &#8677;
              </button>
            </div>
          </form>
        </div>
        <div id="result" class="col-6">
          <div class="sticky">
            <nav>
              <div class="nav nav-tabs" id="results-nav-tab" role="tablist">
                <a
                  class="nav-item nav-link active"
                  id="nav-purchase-info-tab"
                  data-toggle="tab"
                  href="#nav-purchase-info"
                  role="tab"
                  aria-controls="nav-purchase-info"
                  aria-selected="true"
                  >Purchase Info</a
                >
                <a
                  class="nav-item nav-link"
                  id="nav-payment-info-tab"
                  data-toggle="tab"
                  href="#nav-payment-info"
                  role="tab"
                  aria-controls="nav-payment-info"
                  aria-selected="false"
                  >Payment Info</a
                >
              </div>
            </nav>
            <div class="tab-content" id="nav-tabContent">
              <div
                class="tab-pane fade show active"
                id="nav-purchase-info"
                role="tabpanel"
                aria-labelledby="nav-purchase-info"
              >
                <ul class="list-group">
                  <li class="list-group-item">
                    Max Home Purchase Price: ${{ maxHomePurchasePrice }}
                  </li>
                  <li class="list-group-item">
                    Down Payment: ${{ downPayment }}
                  </li>
                  <li class="list-group-item">
                    Mortgage Loan Amount: ${{ mortgageLoanAmount }}
                  </li>
                </ul>
              </div>
              <div
                class="tab-pane fade"
                id="nav-payment-info"
                role="tabpanel"
                aria-labelledby="nav-payment-info"
              >
                <ul class="list-group">
                  <li class="list-group-item">
                    Mortgage Loan Amount: ${{ mortgageLoanAmount }}
                  </li>
                  <li class="list-group-item">
                    MonthlyPayment: ${{ monthlyPayment }}
                  </li>
                  <li class="list-group-item">Term: {{ term }} years</li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "AffordabilityCalculator",
  data() {
    return this.init();
  },
  computed: {
    maxHomePurchasePrice() {
      // todo
      return Math.round(this.grossIncome * 3.0);
    },
    downPayment() {
      return Math.round(
        (this.downPaymentPercent / 100) * this.mortgageLoanAmount
      );
    },
    mortgageLoanAmount() {
      // todo
      return Math.round(this.grossIncome * 2.5);
    },
    ltv() {
      return Math.round(this.downPayment / this.mortgageLoanAmount);
    },
    monthlyPayment() {
      return Math.round(this.mortgageLoanAmount / 12);
    },
    dtiPercent() {
      return Math.round(this.dti * 100);
    },
    closingCost() {
      return Math.round(this.mortgageLoanAmount * 0.05);
    },
    totalMortgagePayment() {
      return this.monthlyPayment();
    },
    principalInterestPayment() {
      return 1000;
    },
    pmi() {
      return 2000;
    }
  },
  methods: {
    init() {
      return {
        grossIncome: 4000,
        carLoan: 500,
        creditCard: 100,
        studentLoan: 150,
        downPaymentPercent: 5,
        interestRatePercent: 4.25,
        term: 30,
        hoi: 1200,
        hoa: 50,
        propertyTax: 1500,
        simpleView: true,
        dti: 0.36
      };
    },
    reset() {
      this.grossIncome = this.init().grossIncome;
      this.carLoan = this.init().carLoan;
      this.creditCard = this.init().creditCard;
      this.studentLoan = this.init().studentLoan;
      this.downPaymentPercent = this.init().downPaymentPercent;
      this.interestRatePercent = this.init().interestRatePercent;
      this.term = this.init().term;
      this.hoi = this.init().hoi;
      this.hoa = this.init().hoa;
      this.propertyTax = this.init().propertyTax;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sticky {
  position: -webkit-sticky;
  position: sticky;
  top: 0;
}
.background-lightblue {
  background-color: lightblue;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
