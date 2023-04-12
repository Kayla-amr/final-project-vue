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
              <input class="name bg-base-200 p-2" type="text" v-model="budgets.name" placeholder="Enter name...">
            </td>
            <td class="otd">
              <input class="amoun bg-base-" type="text" v-model="budgets.amount" placeholder="$0" />
            
              <button type="submit" class=" btn btn-primary ml-3">+</button>
            </td>
          </tr>
        </tbody>
      </table>
    </form>
  </div>
</template>

//Look for string in total and make them numbers

<script>

export default {
  name: 'outcome-form',
  data() {
    return {
      budgets: {
        name: "",
        amount:""
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
    width: 100%;
    border: 1px solid black;
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
    width: 100%;
}
.amount{
    width: 50px;
}
</style>