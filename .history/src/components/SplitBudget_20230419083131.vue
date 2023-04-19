
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
        <thead>
            <tr>
                <th class="name">Expense Name</th>
                <th class="amount">Divided Amount</th>
                <th class="selected">Select</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="budget in budgets" :key="budget.id">
                <td class="name">{{ budget.name }}</td>
                <td>
                    {{ splitBudget(budget.amount) }}
                </td>

                <td>
                    <input type="checkbox" v-model="budget.selected" :disabled="budget.addedToBank === true" />
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
                    {{ firstEntry ? 'Balance After Expense:' : 'Possible Balance:' }}
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
        }
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
    padding: 5px;
}
table th{
    padding: 1rem 5px;
}
</style>

