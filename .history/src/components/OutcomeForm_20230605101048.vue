<template>
  <div>
    <form @submit.prevent="handleSubmit" class="outcomeForm">
      <table class="table">
        <tbody>
          <tr>
            <td>
              <input class="name bg-base-200 p-3 ml-2" type="text" v-model="budgets.name" placeholder="Enter name...">
            </td>
            <td class="otd">
              <input class="amount bg-base-200 p-2 ml-2 w-1/4" type="text" v-model="budgets.amount" placeholder="$0" />
            
              <button type="submit" class=" btn btn-primary ml-2">+</button>
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

<style scoped>
.otd {
  display: flex;
}
</style>