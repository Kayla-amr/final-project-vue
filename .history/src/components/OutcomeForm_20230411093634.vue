import CurrencyInput from './CurrencyInput.vue'

<template>
  <div>
    <form @submit.prevent="handleSubmit" class="outcomeForm">
      <table>
        <thead>
          <tr>
            <th>Monthly Payments</th>
            <th>Amount</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>
              <input class="name" type="text" v-model="budgets.name" placeholder="Enter name...">
            </td>
            <td class="otd">
              <input class="amount" type="text" v-model="budgets.amount" placeholder="$0" />
            
              <button type="submit">+</button>
            </td>
          </tr>
        </tbody>
      </table>
      
    </form>

  </div>
  
</template>

///

<script>

export default {
  name: 'outcome-form',
  data() {
    return {
      budgets: {
        name: "",
        amount: 0
      }
    }
  },
  methods: {
    handleSubmit() {
      const budget = {
        name: this.budgets.name,
        amount: this.budgets.amount
      }
      this.$emit('add:budget', budget)
    }
  }
}
</script>

<style>
table{
    width: 100%;
}
.outcomeForm {
    margin: 0 auto;
    width: 50%;
    border: 1px solid black;
    border-collapse: collapse;
}

.outcomeForm th{
  width: 100%;
}
.otd {
  display: flex;
}
.outcomeForm tr{
    border: none;
    justify-content: space-between;
    
}

input{  
  padding: 2% 0;
  margin: 2% ;
  width: inherit;
  border: none;
}

.name{
    width: 50%;
}
.amount{
    width: 50px;
}
</style>