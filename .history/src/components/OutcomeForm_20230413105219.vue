<template>
  <div>


    <thead>
      <form @submit.prevent="handleSubmit" class="outcomeForm"></form>
      <tr>
        <th>
          <input class="name bg-base-200 p-3" type="text" v-model="budgets.name" placeholder="Enter name...">
        </th>
        <th class="otd">
          <input class="amount bg-base-200 p-2 m-2" type="text" v-model="budgets.amount" placeholder="$0" />

          <button type="submit" class=" btn btn-primary m-2">+</button>
        </th>
      </tr>
      
      </form>
    </thead>
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
        amount: ""
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