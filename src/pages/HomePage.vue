<template>
  <div class="home flex-grow-1 d-flex flex-column align-items-center justify-content-center">
    <div class="container-fluid">
      <div class="row">
        <div class="col">
          <h1>Mortgage Calculator</h1>
        </div>
      </div>
      <div class="row mt-3 ">
        <div class="col">
          <div class="d-flex justify-content-center">
            <p class="mr-3">
              Amount:
            </p> <input type="number" @input="calculate" v-model="state.amount" min="0">
          </div>
        </div>
      </div>
      <div class="row mt-3">
        <div class="col">
          <div class="d-flex justify-content-center">
            <p class="mr-3">
              Interest rate:
            </p> <input type="number" @input="calculate" v-model="state.interest" min="0">
          </div>
        </div>
      </div>
      <div class="row mt-3 justify-content-center">
        <div class="col-3">
          <div class="d-flex justify-content-center">
            <p class="mr-3">
              Term:
            </p>
            <select class="custom-select"
                    id="inputGroupSelect04"
                    aria-label="Example select with button addon"
                    v-model="state.term"
                    @change="calculate"
            >
              <option value="15" selected>
                15 years
              </option>
              <option value="30">
                30 years
              </option>
            </select>
          </div>
        </div>
      </div>
      <br><br>
      <div class="row mt-5">
        <div class="col">
          <div>
            <p class="mr-3 text-uppercase font-weight-bold">
              Total Balance: ${{ state.balance }}
            </p>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div>
            <p class="mr-3 text-uppercase font-weight-bold">
              Monthly Payment: ${{ state.pmt }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from '@vue/reactivity'
export default {
  name: 'Home',
  setup() {
    const state = reactive({
      amount: 0,
      interest: 5.5,
      term: 15,
      balance: 0,
      pmt: 0
    })
    return {
      state,
      calculate() {
        if (!state.amount || !state.interest || !state.term) {
          state.balance = 0
          state.pmt = 0
        } else if (state.amount <= 0 || state.interest <= 0 || state.term <= 0) {
          state.balance = 0
          state.pmt = 0
        } else {
          const r = (state.interest * 0.01) / 12
          const n = state.term * 12
          const p = state.amount
          const balance = ((r * p * n) / (1 - Math.pow(1 + r, -n))).toFixed(2)
          const pmt = (balance / n).toFixed(2)
          state.balance = balance
          state.pmt = pmt
        }
      }
    }
  }
}
</script>

<style scoped lang="scss">
.home{
  text-align: center;
  user-select: none;
  > img{
    height: 200px;
    width: 200px;
  }
}
p{
  font-size: 1.5rem;
}
input{
  height: 2.5rem;
}
select{
  height: 2.5rem;
}
</style>
