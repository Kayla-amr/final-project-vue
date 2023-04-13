<template>
    <div class="document-budget">
        <table class="table">
            <tbody>
                <tr class="budgetList" v-for="budget in budgets" :key="budget.id">
                    <td class="name"> <input type="text" v-model="budget.name" class="bg-base-200"></td>
                    <td class="amount"><input type="text" v-model="budget.amount" class="bg-base-200 p-2"></td>
                    <td><button class="btn btn-neutral" @click="removeBudget(budget.id)">X</button></td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td class="name">Total</td>
                    <td class="amount">{{ total }}</td>
                    <td><p> </p></td>
                </tr>
                <tr>
                    <td><p> </p></td>
                    <td><button class="btn btn-error my-2" @click="clearAll()">Clear all</button></td>
                    <td><p> </p></td>
                </tr>
            </tfoot>
        </table>
    </div>

</template>

<script>
export default {
    name: 'document-budget',
    props: {
        budgets: Array
    },
    computed: {
        total() {
    return this.budgets.reduce((total, budget) => {
      return total + parseFloat(budget.amount)
    }, 0)
  }
       
    },
    methods: {
        removeBudget(id) {
            const index = this.budgets.findIndex(budget => budget.id === id)
            // eslint-disable-next-line vue/no-mutating-props
            this.budgets.splice(index, 1)
        },
        clearAll(){
            // eslint-disable-next-line vue/no-mutating-props
            this.budgets.splice(0, this.budgets.length)
        },
    }
}
</script>