<template>
  <div class="sliders">
    <div v-if="!isSubmitted" id="sliders-page">
      <div class="col">
        <div class="row">
          <div class="col">
            <p>
              Estimate your monthly mortgage payments based on the home price.
            </p>
            <p class="text-italic">
              Note: Calculators display default values. Enter new figures to
              override.
            </p>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <form action>
              <div class="row">
                <div id="input" class="col col-md-6">
                  <div id="gross-income-input-group" class="form-group">
                    <label for="gross-income-input-field"
                      >Home Purchase Price</label
                    >
                    <div class="input-group mb-2">
                      <div class="input-group-prepend">
                        <div class="input-group-text">$</div>
                      </div>
                      <input
                        id="gross-income-input-field"
                        v-model.number="$v.grossIncome.$model"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            $v.grossIncome.$error || $v.grossIncome.$invalid
                        }"
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
                      <div class="invalid-feedback">
                        <span
                          v-if="
                            !$v.grossIncome.between || !$v.grossIncome.required
                          "
                          >{{ errorMsgPre }} ${{
                            Number(
                              $v.grossIncome.$params.between.min
                            ).toLocaleString()
                          }}
                          and ${{
                            Number(
                              $v.grossIncome.$params.between.max
                            ).toLocaleString()
                          }}</span
                        >
                      </div>
                    </div>
                  </div>
                  <div id="down-payment-input-group" class="form-group">
                    <label for="down-payment-input-field">Down Payment</label>
                    <div class="input-group mb-2">
                      <input
                        id="down-payment-input-field"
                        v-model.number="$v.downPaymentPercent.$model"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            $v.downPaymentPercent.$error ||
                            $v.downPaymentPercent.$invalid
                        }"
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
                        step="0.25"
                      />
                      <div class="invalid-feedback">
                        <span
                          v-if="
                            !$v.downPaymentPercent.between ||
                              !$v.downPaymentPercent.required
                          "
                          >{{ errorMsgPre }}
                          {{
                            Number(
                              $v.downPaymentPercent.$params.between.min
                            ).toLocaleString()
                          }}% and
                          {{
                            Number(
                              $v.downPaymentPercent.$params.between.max
                            ).toLocaleString()
                          }}%</span
                        >
                      </div>
                    </div>
                  </div>
                  <div id="interest-rate-input-group" class="form-group">
                    <label for="interest-rate-input-field">Interest Rate</label>
                    <div class="input-group mb-2">
                      <input
                        id="interest-rate-input-field"
                        v-model.number="$v.interestRatePercent.$model"
                        type="number"
                        step="0.25"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            $v.interestRatePercent.$error ||
                            $v.interestRatePercent.$invalid
                        }"
                      />
                      <div class="input-group-append">
                        <div class="input-group-text">%</div>
                      </div>
                      <input
                        id="interest-rate-input-range"
                        v-model.number="interestRate"
                        type="range"
                        class="custom-range"
                        min="2.5"
                        max="11"
                        step="0.25"
                      />
                      <div class="invalid-feedback">
                        <span
                          v-if="
                            !$v.interestRatePercent.between ||
                              !$v.interestRatePercent.required
                          "
                          >{{ errorMsgPre }}
                          {{
                            Number(
                              $v.interestRatePercent.$params.between.min
                            ).toLocaleString()
                          }}% and
                          {{
                            Number(
                              $v.interestRatePercent.$params.between.max
                            ).toLocaleString()
                          }}%</span
                        >
                      </div>
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
                        <div class="input-group-text">yrs</div>
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
                          v-model.number="$v.hoi.$model"
                          type="number"
                          class="form-control"
                          :class="{
                            'is-invalid': $v.hoi.$error || $v.hoi.$invalid
                          }"
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
                        <div class="invalid-feedback">
                          <span v-if="!$v.hoi.between || !$v.hoi.required"
                            >{{ errorMsgPre }} ${{
                              Number(
                                $v.hoi.$params.between.min
                              ).toLocaleString()
                            }}
                            and ${{
                              Number(
                                $v.hoi.$params.between.max
                              ).toLocaleString()
                            }}</span
                          >
                        </div>
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
                          v-model.number="$v.hoa.$model"
                          type="number"
                          class="form-control"
                          :class="{
                            'is-invalid': $v.hoa.$error || $v.hoa.$invalid
                          }"
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
                        <div class="invalid-feedback">
                          <span v-if="!$v.hoa.between || !$v.hoa.required"
                            >{{ errorMsgPre }} ${{
                              Number(
                                $v.hoa.$params.between.min
                              ).toLocaleString()
                            }}
                            and ${{
                              Number(
                                $v.hoa.$params.between.max
                              ).toLocaleString()
                            }}</span
                          >
                        </div>
                      </div>
                    </div>
                    <div id="property-tax-input-group" class="form-group">
                      <label for="property-tax-input-field"
                        >Property Taxes</label
                      >
                      <div class="input-group mb-2">
                        <div class="input-group-prepend">
                          <div class="input-group-text">$</div>
                        </div>
                        <input
                          id="property-tax-input-field"
                          v-model.number="$v.propertyTax.$model"
                          type="number"
                          class="form-control"
                          :class="{
                            'is-invalid':
                              $v.propertyTax.$error || $v.propertyTax.$invalid
                          }"
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
                        <div class="invalid-feedback">
                          <span
                            v-if="
                              !$v.propertyTax.between ||
                                !$v.propertyTax.required
                            "
                            >{{ errorMsgPre }} ${{
                              Number(
                                $v.propertyTax.$params.between.min
                              ).toLocaleString()
                            }}
                            and ${{
                              Number(
                                $v.propertyTax.$params.between.max
                              ).toLocaleString()
                            }}</span
                          >
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div id="result" class="col col-md-6">
                  <div class="sticky">
                    <nav>
                      <div
                        id="results-nav-tab"
                        class="nav nav-tabs"
                        role="tablist"
                      >
                        <a
                          id="nav-purchase-info-tab"
                          class="nav-item nav-link active"
                          data-toggle="tab"
                          href="#nav-purchase-info"
                          role="tab"
                          aria-controls="nav-purchase-info"
                          aria-selected="true"
                          >Purchase Info</a
                        >
                        <a
                          id="nav-payment-info-tab"
                          class="nav-item nav-link"
                          data-toggle="tab"
                          href="#nav-payment-info"
                          role="tab"
                          aria-controls="nav-payment-info"
                          aria-selected="false"
                          >Loan Info</a
                        >
                      </div>
                    </nav>
                    <div id="nav-tabContent" class="tab-content">
                      <div
                        id="nav-purchase-info"
                        class="tab-pane fade show active"
                        role="tabpanel"
                        aria-labelledby="nav-purchase-info"
                      >
                        <ul class="list-group">
                          <li class="list-group-item">
                            Monthly Mortgage Payment: ${{
                              Math.round(maxHomePurchasePrice)
                            }}
                          </li>
                          <li class="list-group-item">
                            PMI: ${{ Math.round(downPayment) }}
                          </li>
                          <li class="list-group-item">
                            HOA: ${{ Math.round(mortgageLoanAmount) }}
                          </li>
                          <li class="list-group-item">
                            Taxes & Insurance: ${{
                              Math.round(mortgageLoanAmount)
                            }}
                          </li>
                          <li class="list-group-item">
                            Principal & Interest: ${{
                              Math.round(mortgageLoanAmount)
                            }}
                          </li>
                        </ul>
                      </div>
                      <div
                        id="nav-payment-info"
                        class="tab-pane fade"
                        role="tabpanel"
                        aria-labelledby="nav-payment-info"
                      >
                        <ul class="list-group">
                          <li class="list-group-item">
                            Mortgage Loan Amount: ${{
                              Math.round(mortgageLoanAmount)
                            }}
                          </li>
                          <li class="list-group-item">
                            Down Payment: ${{ Math.round(totalMonthlyPayment) }}
                          </li>
                          <li class="list-group-item">
                            Term: {{ term }} years
                          </li>
                        </ul>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="col">
                  <hr />
                  <div class="row">
                    <div id="form-mod-group" class="form-group col col-md-6">
                      <button
                        type="button"
                        class="btn btn-base"
                        @click="simpleView = !simpleView"
                      >
                        {{ simpleView ? "Advanced View" : "Simple View" }}
                      </button>
                      <span class="divider"> | </span>
                      <button type="button" class="btn btn-base" @click="reset">
                        Reset
                      </button>
                    </div>
                    <div id="submit-form-group" class="form-group col col-md-6">
                      <button
                        class="btn btn-block resultsBtn btn-primary"
                        type="submit"
                        :disabled="$v.$invalid"
                        @click.prevent="isSubmitted = !isSubmitted"
                      >
                        See Results <i class="fa fa-caret-right" />
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-if="isSubmitted" id="result-page">
      <div class="col">
        <ul class="list-group">
          <p>YOU CAN EXPECT TO PAY:</p>
          <li class="list-group-item">
            Home Purchase Price: ${{ Math.round(maxHomePurchasePrice) }}
          </li>
          <li class="list-group-item">
            Mortgage Loan Amount: ${{ Math.round(mortgageLoanAmount) }}
          </li>
          <li class="list-group-item">
            Total Mortgage Payment: ${{ Math.round(mortgageLoanAmount) }}
          </li>
          <p>EXPECTED FUNDS NEEDED AT CLOSING:</p>
          <li class="list-group-item">
            Total Cash Required: ${{ Math.round(grossIncome) }}
          </li>
          <li class="list-group-item">
            Down Payment: ${{ Math.round(totalDebts) }}
          </li>
          <li class="list-group-item">
            Closing Costs: ${{ Math.round(carLoan) }}
          </li>
          <p>INFORMATION ABOUT YOUR POTENTIAL MORTGAGE:</p>
          <li class="list-group-item">
            Mortgage Loan Amount: ${{ Math.round(downPayment) }}
          </li>
          <li class="list-group-item">Term: ${{ Math.round(closingCost) }}</li>
          <li class="list-group-item">
            Total Mortgage Payment: ${{ Math.round(closingCost) }}
          </li>
          <li class="list-group-item">
            Principal & Interest (P&I) Payment: ${{ Math.round(closingCost) }}
          </li>
          <li class="list-group-item">
            Property Taxes: ${{ Math.round(closingCost) }}
          </li>
          <li class="list-group-item">
            Homeowners Insurance: ${{ Math.round(closingCost) }}
          </li>
          <li class="list-group-item">PMI: ${{ Math.round(closingCost) }}</li>
          <li class="list-group-item">
            HOA Fees: ${{ Math.round(closingCost) }}
          </li>
          <li class="list-group-item">LTV: ${{ Math.round(closingCost) }}</li>
        </ul>
      </div>
      <button
        class="btn btn-block btn-primary"
        @click.prevent="isSubmitted = !isSubmitted"
      >
        Back to Calculator &#8677;
      </button>
      <p>
        These calculator results are estimates based on your inputs. Contact a
        bank, credit union, housing advisor, or lender to determine your loan
        eligibility and accurate costs. Fannie Mae does not offer mortgage loans
        to consumers and this in no way indicates approval or financing of a
        mortgage loan.
      </p>
    </div>
  </div>
</template>


<script>
import { required, between } from "vuelidate/lib/validators";

export default {
  name: "PurchaseCalculator",
  data() {
    return this.init();
  },
  validations: {
    grossIncome: {
      required,
      between: between(500, 50000)
    },
    carLoan: {
      required,
      between: between(0, 5000)
    },
    creditCard: {
      required,
      between: between(0, 5000)
    },
    studentLoan: {
      required,
      between: between(0, 5000)
    },
    downPaymentPercent: {
      required,
      between: between(3, 50)
    },
    interestRatePercent: {
      required,
      between: between(2.5, 11)
    },
    hoi: {
      required,
      between: between(0, 24000)
    },
    hoa: {
      required,
      between: between(0, 1000)
    },
    propertyTax: {
      required,
      between: between(0, 40000)
    }
  },
  computed: {
    maxHomePurchasePrice() {
      let max = this.mortgageLoanAmount + this.downPayment;
      return this.handleNegativeNums(max);
    },
    downPayment() {
      let down =
        this.mortgageLoanAmount / (1 - this.downPaymentPercent / 100) -
        this.mortgageLoanAmount;
      return this.handleNegativeNums(down);
    },
    mortgageLoanAmount() {
      let loan = this.pv(
        this.interestRate / 12,
        this.term * 12,
        this.principalInterestPayment
      );
      return this.handleNegativeNums(loan);
    },
    loanToValue() {
      return this.downPayment / this.mortgageLoanAmount;
    },
    estimatedMonthlyPayment() {
      return this.grossIncome * this.dti - this.totalDebts;
    },
    dtiPercent() {
      return this.dti * 100;
    },
    interestRate() {
      return this.interestRatePercent / 100;
    },
    closingCost() {
      return Math.round(this.maxHomePurchasePrice * this.closingCostMultiplier);
    },
    totalMonthlyPayment() {
      let payment =
        this.propertyTaxMonthly +
        this.hoiMonthly +
        this.hoa +
        this.monthlyMortagePremium +
        this.principalInterestPayment;
      if (this.maxHomePurchasePrice <= 0) {
        return 0;
      }
      return this.handleNegativeNums(payment);
    },
    principalInterestPayment() {
      if (this.estimatedMonthlyPayment <= 0) {
        return 0;
      }
      let payment =
        this.estimatedMonthlyPayment -
        (this.propertyTaxMonthly +
          this.hoiMonthly +
          this.hoa +
          this.monthlyMortagePremium);
      return this.handleNegativeNums(payment);
    },
    pmi() {
      if (this.downPaymentPercent >= 20) {
        return 0;
      }
      if (this.downPaymentPercent >= 10) {
        return 0.0044;
      }
      if (this.downPaymentPercent >= 5) {
        return 0.0062;
      }
      return 0.011;
    },
    monthlyMortagePremium() {
      if (this.guessLoanAmount <= 0) {
        return 0;
      }
      let premium = (this.pmi * this.guessLoanAmount) / 12;
      return this.handleNegativeNums(premium);
    },
    guessLoanAmount() {
      return this.pv(
        this.interestRate / 12,
        this.term * 12,
        this.estimatedMonthlyPayment - this.grossIncome * 0.05
      );
    },
    totalDebts() {
      return this.carLoan + this.creditCard + this.studentLoan;
    },
    propertyTaxMonthly() {
      return this.propertyTax / 12;
    },
    hoiMonthly() {
      return this.hoi / 12;
    }
  },
  methods: {
    init() {
      return {
        isSubmitted: false,
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
        dti: 0.36,
        closingCostMultiplier: 0.025,
        errorMsgPre: "Please enter a valid amount between"
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
    },
    pv(rate, nper, pmt) {
      let x = (1 + rate) ** nper;
      return (pmt * ((x - 1) / rate)) / x;
    },
    handleNegativeNums(n) {
      if (n < 0) {
        return 0;
      }
      return n;
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
