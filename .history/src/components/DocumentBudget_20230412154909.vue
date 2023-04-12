<template>
    <div class="document-budget">
        <table>
            <tbody>
                <tr class="budgetList" v-for="budget in budgets" :key="budget.id">
                    <td class="name"> <input type="text" v-model="budget.name" class="bg-base-200 p-2"></td>
                    <td class="amount"><input type="text" v-model="budget.amount" class="bg-base-200 p-2"></td>
                    <td><button class="btn btn-neutral" @click="removeBudget(budget.id)">X</button></td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td class="name">Total</td>
                    <td class="amount">{{ total }}</td>
                </tr>
                <tr><button class="btn btn" @click="clearAll()">Clear all</button></tr>
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

<style>
.document-budget {
    margin: 0 auto;
    width: 100%;
    border: 1px solid black;
}
tr{
    width: 100%;
    display: flex;
    border: none;
    justify-content: center;
    border-bottom: 1px solid black;
    padding: 5px;
    
}
td{
    border: none;
    padding: 2%;
    margin: 2%;
}

tfoot td{
    width: 100%;
}
.document-budget .amount button{
    margin-left: 10px;
}
.amount{
    display: flex;
    flex-direction: row;
    align-items: center;
}

.budgetDivided{
    width: 20%;
}

.budgetDivided td{
    padding: 10px;
    display: flex;
    justify-content: center;
}

</style>