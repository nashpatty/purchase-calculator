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
                        v-model.number="$v.homePurchasePrice.$model"
                        @input="updateFees"
                        type="number"
                        class="form-control"
                        :class="{
                          'is-invalid':
                            $v.homePurchasePrice.$error ||
                            $v.homePurchasePrice.$invalid
                        }"
                      />
                      <input
                        id="gross-income-input-range"
                        v-model.number="homePurchasePrice"
                        @input="updateFees"
                        type="range"
                        class="custom-range"
                        min="10000"
                        max="1000000"
                      />
                      <div class="invalid-feedback">
                        <span
                          v-if="
                            !$v.homePurchasePrice.between ||
                              !$v.homePurchasePrice.required
                          "
                          >{{ errorMsgPre }} ${{
                            Number(
                              $v.homePurchasePrice.$params.between.min
                            ).toLocaleString()
                          }}
                          and ${{
                            Number(
                              $v.homePurchasePrice.$params.between.max
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
                        v-model.number="interestRatePercent"
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
                              Math.round(totalMonthlyMortgage)
                            }}
                          </li>
                          <li class="list-group-item">
                            PMI: ${{ Math.round(monthlyMortgageInsurance) }}
                          </li>
                          <li class="list-group-item">
                            HOA: ${{ Math.round(hoa) }}
                          </li>
                          <li class="list-group-item">
                            Taxes & Insurance: ${{
                              Math.round(monthlyPropertyTax + monthlyHoi)
                            }}
                          </li>
                          <li class="list-group-item">
                            Principal & Interest: ${{
                              Math.round(monthlyPrincipalInterestPayment)
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
                            Down Payment: ${{ Math.round(downPayment) }}
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
            Home Purchase Price: ${{ Math.round(homePurchasePrice) }}
          </li>
          <li class="list-group-item">
            Mortgage Loan Amount: ${{ Math.round(mortgageLoanAmount) }}
          </li>
          <li class="list-group-item">
            Total Mortgage Payment: ${{ Math.round(totalMonthlyMortgage) }}
          </li>
          <p>EXPECTED FUNDS NEEDED AT CLOSING:</p>
          <li class="list-group-item">
            Total Cash Required: ${{ Math.round(downPayment + closingCost) }}
          </li>
          <li class="list-group-item">
            Down Payment: ${{ Math.round(downPayment) }}
          </li>
          <li class="list-group-item">
            Closing Costs: ${{ Math.round(closingCost) }}
          </li>
          <p>INFORMATION ABOUT YOUR POTENTIAL MORTGAGE:</p>
          <li class="list-group-item">
            Mortgage Loan Amount: ${{ Math.round(mortgageLoanAmount) }}
          </li>
          <li class="list-group-item">Term: ${{ Math.round(term) }}</li>
          <li class="list-group-item">
            Total Mortgage Payment: ${{ Math.round(totalMonthlyMortgage) }}
          </li>
          <li class="list-group-item">
            Principal & Interest (P&I) Payment: ${{
              Math.round(monthlyPrincipalInterestPayment)
            }}
          </li>
          <li class="list-group-item">
            Property Taxes: ${{ Math.round(monthlyPropertyTax) }}
          </li>
          <li class="list-group-item">
            Homeowners Insurance: ${{ Math.round(monthlyHoi) }}
          </li>
          <li class="list-group-item">
            PMI: ${{ Math.round(monthlyMortgageInsurance) }}
          </li>
          <li class="list-group-item">HOA Fees: ${{ Math.round(hoa) }}</li>
          <li class="list-group-item">LTV: ${{ Math.round(ltv) }}</li>
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
    homePurchasePrice: {
      required,
      between: between(10000, 1000000)
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
      between: between(0, 4000)
    },
    hoa: {
      required,
      between: between(0, 1000)
    },
    propertyTax: {
      required,
      between: between(400, 25000)
    }
  },
  computed: {
    downPayment() {
      return this.homePurchasePrice * (this.downPaymentPercent / 100);
    },
    mortgageLoanAmount() {
      return this.homePurchasePrice - this.downPayment;
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
    closingCost() {
      return this.homePurchasePrice * this.closingCostMultiplier;
    },
    monthlyMortgageInsurance() {
      return (this.pmi * this.mortgageLoanAmount) / 12;
    },
    monthlyPrincipalInterestPayment() {
      return this.pmt(
        this.interestRatePercent / 100 / 12,
        this.term * 12,
        this.mortgageLoanAmount
      );
    },
    monthlyPropertyTax() {
      return this.propertyTax / 12;
    },
    monthlyHoi() {
      return this.hoi / 12;
    },
    totalMonthlyMortgage() {
      return (
        this.monthlyMortgageInsurance +
        this.monthlyPrincipalInterestPayment +
        this.monthlyPropertyTax +
        this.monthlyHoi +
        this.hoa
      );
    },
    ltv() {
      return (this.mortgageLoanAmount / this.homePurchasePrice) * 100;
    }
  },
  methods: {
    init() {
      return {
        isSubmitted: false,
        homePurchasePrice: 200000,
        downPaymentPercent: 5,
        interestRatePercent: 4.25,
        term: 30,
        hoi: 1200,
        hoa: 50,
        propertyTax: 1500,
        simpleView: true,
        closingCostMultiplier: 0.025,
        errorMsgPre: "Please enter a valid amount between"
      };
    },
    reset() {
      this.homePurchasePrice = this.init().homePurchasePrice;
      this.downPaymentPercent = this.init().downPaymentPercent;
      this.interestRatePercent = this.init().interestRatePercent;
      this.term = this.init().term;
      this.hoi = this.init().hoi;
      this.hoa = this.init().hoa;
      this.propertyTax = this.init().propertyTax;
    },
    pmt(rate, period, pv) {
      return (pv * rate) / (1 - (1 + rate) ** (-1 * period));
    },
    handleNegativeNums(n) {
      if (n < 0) {
        return 0;
      }
      return n;
    },
    updateFees() {
      let dynamicHoa = Math.round((this.homePurchasePrice * 0.0025) / 12);
      this.hoa =
        dynamicHoa > this.$v.hoa.$params.between.max
          ? this.$v.hoa.$params.between.max
          : dynamicHoa;
      let dynamicHoi = Math.round(this.homePurchasePrice * 0.0075);
      this.hoi =
        dynamicHoi > this.$v.hoi.$params.between.max
          ? this.$v.hoi.$params.between.max
          : dynamicHoi;
      let dynamicTax = Math.round(this.homePurchasePrice * 0.0125);
      this.propertyTax =
        dynamicTax < this.$v.propertyTax.$params.between.min
          ? this.$v.propertyTax.$params.between.min
          : dynamicTax;
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
