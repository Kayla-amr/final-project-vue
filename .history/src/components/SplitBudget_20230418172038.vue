
//Select weekly, biweekly, or other
<template>
    <div>

        <table>
            <thead>
                <tr>
                    <td>
                        <form @submit.prevent="handleSubmit">
                            <div class="calculateBdgt py-2">
                                <input type="text" id="amount" v-model="income.amount" placeholder="$0"
                                    class="bg-base-200 p-2" />
                                <button type="submit" class="btn btn-primary ml-2">Calculate</button>
                            </div>
                        </form>
                    </td>
                </tr>


                <tr v-for="income in incomes" :key="income.id">
                    <th><label>Bi-Weekly Pay Entered:</label>
                        {{ income.amount }}
                        <button class="btn btn-error my-2" type="delete" @click="deleteIncome(income.id)">x</button>
                    </th>
                </tr>
            </thead>
        </table>
    </div>
    <table class=" table">
        <tbody>
            <tr v-for="budget in budgets" :key="budget.id">
                <td class="name">{{ budget.name }}</td>
                <td>
                    {{ splitBudget(budget.amount) }}
                </td>

                <td>
                    <input type="checkbox" v-model="budget.selected" :disabled="budget.addedToBank === true" />
                    <button class="btn btn-primary" type="transfer" >undo</button>
                </td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td class="name">Divided Total:</td>
                <td>
                    <p> </p>
                </td>
                <td class="amount totalDivided">{{ totalDivided }}</td>
            </tr>
            <tr v-for="income in incomes" :key="income.id">
                <td for="amount" class="name p-2" >
                    {{ firstEntry ? 'Remainder:' : 'Possible Remainder:' }}
                </td>
                <td>
                    <p> </p>
                </td>
                <td>{{ remainder(income.amount) }}</td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
export default {
    name: 'split-budget',
    props: {
        budgets: Array,
        incomes: Array,
        banks: Array
    },
    emits: ['add:income'],
    computed: {
        total() {
            return this.budgets.reduce((total, budget) => {
                return total + budget.amount
            }, 0)
        },
        totalDivided() {
            return this.budgets.reduce((total, budget) => {
                return Math.ceil(total + (budget.amount / 2))
            }, 0)
        },

    },
    data() {
        return {
            income: {
                name: '',
                amount: ''
            },
            firstEntry: true
        }
    },
    methods: {
        handleSubmit() {
            this.$emit('add:income', this.income)
            this.income = {
                name: '',
                amount: ''
            }
            this.firstEntry = false
        },
        deleteIncome(id) {
            const index = this.incomes.findIndex(income => income.id === id)
            // eslint-disable-next-line vue/no-mutating-props
            this.incomes.splice(index, 1)
        },
        splitBudget(amount) {
            return Math.ceil(amount / 2)
        },
        remainder(amount) {
            return Math.ceil(amount - this.totalDivided)
        },
        undoSelectedBudget(budget) {
        if (budget.addedToBank) {
            this.$emit("undo:selectedBudget", budget);
        }
    },
},

    In the bank-transfer component, listen for the undo:selectedBudget event and bind it to a new method called handleUndoSelectedBudget:

html

<split-budget
    :budgets="budgets"
    :incomes="incomes"
    :banks="banks"
    @add:income="addIncome"
    @undo:selectedBudget="handleUndoSelectedBudget"
></split-budget>

    In the bank-transfer component, create a new method called handleUndoSelectedBudget. This method will receive a budget as an argument, find the bank to which the budget was added, remove the budget amount, and set the addedToBank property of the budget to false:

javascript
methods: {
    // ...
    handleUndoSelectedBudget(budget) {
        const bank = this.banks.find(
            (bank) => bank


    }
}
</script>

<style scoped>
.calculateBdgt {
    display: flex;
    flex-direction: row;
}

.totalDivided {
    font-weight: bold;
}

.name {
    font-weight: bold;
}</style>

