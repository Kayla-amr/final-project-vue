<template>
    <div>

        <table>
            <thead>

            </thead>
            <tbody>

            </tbody>
        </table>
    </div>
    <div>
        <table class="budgetDivided">
            <thead>
                <tr>
                    <th>
                        <form @submit.prevent="handleSubmit">
                            <label for="amount">Enter pay:</label>
                            <input type="text" id="amount" v-model="income.amount" placeholder="$0" />
                            <button type="submit">Calculate Income</button>
                        </form>
                    </th>
                </tr>

                <td>Pay entered: </td>
                <tr v-for="income in incomes" :key="income.id">
                    <td>{{ income.amount }}</td>
                    <td> <button type="delete" @click="deleteIncome(income.id)">Clear</button></td>
                </tr>
                <tr>
                    <th>
                    <td>Amount split</td>
                    </th>

                </tr>

            </thead>
            <tbody>
                <tr v-for="budget in budgets" :key="budget.id">
                    <td> 
                        <input type="checkbox" v-model="budget.selected" :disabled="budget.addedToBank === true" />
                    </td>
                    <td class="name">{{ budget.name }}</td>
                    <td>
                        {{ splitBudget(budget.amount) }}
                    </td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td class="name">Total</td>
                    <td class="amount totalDivided">{{totalDivided}}</td>
                </tr>
                <tr v-for="income in incomes" :key="income.id">
                    <td>Remaining balance:</td>
                    <td>{{remainder(income.amount) }}</td>
                </tr>
            </tfoot>
        </table>
    </div>
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
            }
        }
    },
    methods: {
        handleSubmit() {
            this.$emit('add:income', this.income)
            this.income = {
                name: '',
                amount: ''
            }
        },
        deleteIncome(id) {
            const index = this.incomes.findIndex(income => income.id === id)
            // eslint-disable-next-line vue/no-mutating-props
            this.incomes.splice(index, 1)
        },
        splitBudget(amount) {
            return Math.ceil(amount / 2)
        },
        remainder(amount){
            return Math.ceil(amount - this.totalDivided)
        }
    }
}
</script>

<style>
.budgetDivided {
    margin: 0 auto;
    width: 100%;
    border: 1px solid black;
}

.budgetDivided th {
    width: 100%;
    display: flex;
    justify-content: center;
    border-bottom: 1px solid black;
}

.budgetDivided td {
    width: 100%;
    display: flex;
    white-space: nowrap;
}

.budgetDivided .name {
    display: flex;
    justify-content: flex-start;
}
</style>