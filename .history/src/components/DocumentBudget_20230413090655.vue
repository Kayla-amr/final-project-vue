<template>
    <div class="document-budget">
        <table class="table">
            <tbody>
                <tr class="budgetList" v-for="budget in budgets" :key="budget.id">
                    <td class="name"> <input type="text" v-model="budget.name" class="bg-base-100 p-2"></td>
                    <td class="amount"><input type="text" v-model="budget.amount" class="bg-base-100 p-2"></td>
                    <td class="button"><button class="btn btn-error" @click="removeBudget(budget.id)">X</button></td>
                    <!-- The button to open modal -->
<label for="my-modal-3" class="btn">open modal</label>

<!-- Put this part before </body> tag -->
<input type="checkbox" id="my-modal-3" class="modal-toggle" />
<div class="modal">
  <div class="modal-box relative">
    <label for="my-modal-3" class="btn btn-sm btn-circle absolute right-2 top-2">✕</label>
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
  </div>
</div>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td class="name">Total Expenses:</td>
                    <td class="amount">{{ total }}</td>
                    <td><button class="btn btn-error my-2" @click="clearAll()">Clear</button></td>
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