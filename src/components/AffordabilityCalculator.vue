<template>
  <div class="hello">
    <div class="container">
      <div class="col">
        <div class="row">
          <div class="col">
            <p>Affordability Calculator</p>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <p>Figure out the maximum home price you can afford before you start house hunting.</p>
            <p>Note: Calculators display default values. Enter new figures to override.</p>
          </div>
        </div>
        <div class="row">
          <div class="col-6" id="input">
            <form action>
              <div class="form-group" id="gross-income-input-group">
                <label for="gross-income-input-field">Gross Income</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    v-model.number="grossIncome"
                    type="number"
                    class="form-control"
                    id="gross-income-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/mo</div>
                  </div>
                  <input
                    v-model.number="grossIncome"
                    type="range"
                    class="custom-range"
                    min="500"
                    max="50000"
                    value="4000"
                    id="gross-income-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="car-loan-input-group">
                <label for="car-loan-input-field">Car Loan</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    v-model.number="carLoan"
                    type="number"
                    class="form-control"
                    id="car-loan-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/mo</div>
                  </div>
                  <input
                    v-model.number="carLoan"
                    type="range"
                    class="custom-range"
                    min="0"
                    max="5000"
                    id="car-loan-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="credit-card-input-group">
                <label for="credit-card-input-field">Credit Cards</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    v-model.number="creditCard"
                    type="number"
                    class="form-control"
                    id="credit-card-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/mo</div>
                  </div>
                  <input
                    v-model.number="creditCard"
                    type="range"
                    class="custom-range"
                    min="0"
                    max="5000"
                    id="credit-card-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="other-debts-input-group">
                <label for="other-debts-input-field">Student Loan/Other Debts</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    v-model.number="studentLoan"
                    type="number"
                    class="form-control"
                    id="other-debts-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/mo</div>
                  </div>
                  <input
                    v-model.number="studentLoan"
                    type="range"
                    class="custom-range"
                    min="0"
                    max="5000"
                    id="other-debts-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="down-payment-input-group">
                <label for="down-payment-input-field">Down Payment</label>
                <div class="input-group mb-2">
                  <input
                    v-model.number="downPayment"
                    type="number"
                    class="form-control"
                    id="down-payment-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">%</div>
                  </div>
                  <input
                    v-model.number="downPayment"
                    type="range"
                    class="custom-range"
                    min="3"
                    max="50"
                    id="down-payment-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="interest-rate-input-group">
                <label for="interest-rate-input-field">Interest Rate</label>
                <div class="input-group mb-2">
                  <input
                    v-model.number="interestRate"
                    type="number"
                    class="form-control"
                    id="interest-rate-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">%</div>
                  </div>
                  <input
                    v-model.number="interestRate"
                    type="range"
                    class="custom-range"
                    min="2.5"
                    max="11"
                    step="0.25"
                    id="interest-rate-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="term-input-group">
                <label for="term-input-select">Term</label>
                <div class="input-group mb-2">
                  <select v-model.number="term" class="form-control" id="term-input-select">
                    <option>10</option>
                    <option>15</option>
                    <option>20</option>
                    <option>25</option>
                    <option selected>30</option>
                  </select>
                  <div class="input-group-append">
                    <div class="input-group-text">years</div>
                  </div>
                </div>
              </div>
              <div class="form-group" id="hoi-input-group">
                <label for="hoi-input-field">Homeowner's Insurance</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    v-model.number="hoi"
                    type="number"
                    class="form-control"
                    id="hoi-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/yr</div>
                  </div>
                  <input
                    v-model.number="hoi"
                    type="range"
                    class="custom-range"
                    min="0"
                    max="4000"
                    id="hoi-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="hoa-input-group">
                <label for="hoa-input-field">Homeowner's Association (HOA) or Condo Fees</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    v-model.number="hoa"
                    type="number"
                    class="form-control"
                    id="hoa-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/mo</div>
                  </div>
                  <input
                    v-model.number="hoa"
                    type="range"
                    class="custom-range"
                    min="5"
                    max="600"
                    id="hoa-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="property-tax-input-group">
                <label for="property-tax-input-field">Property Taxes</label>
                <div class="input-group mb-2">
                  <div class="input-group-prepend">
                    <div class="input-group-text">$</div>
                  </div>
                  <input
                    v-model.number="propertyTax"
                    type="number"
                    class="form-control"
                    id="property-tax-input-field"
                  />
                  <div class="input-group-append">
                    <div class="input-group-text">/yr</div>
                  </div>
                  <input
                    v-model.number="propertyTax"
                    type="range"
                    class="custom-range"
                    min="400"
                    max="25000"
                    id="property-tax-input-range"
                  />
                </div>
              </div>
              <div class="form-group" id="form-mod-group">
                <div class="row">
                  <div class="col">
                    <button type="button" class="btn btn-secondary btn-block">Simple View &#8613;</button>
                  </div>
                  <div class="col">
                    <button type="button" class="btn btn-secondary btn-block">Reset &#8634;</button>
                  </div>
                </div>
              </div>
              <div class="form-group" id="submit-form-group">
                <button class="btn btn-block btn-primary" type="submit">See Results &#8677;</button>
              </div>
            </form>
          </div>
          <div class="col-6" id="result">
            <div class="sticky">
              <p class="background-lightblue text-center rounded">results go here (sticky)</p>
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
    return {
      grossIncome: 4000,
      carLoan: 500,
      creditCard: 100,
      studentLoan: 150,
      downPayment: 5,
      interestRate: 4.25,
      term: 30,
      hoi: 1200,
      hoa: 50,
      propertyTax: 1500
    };
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
