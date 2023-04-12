<template>
    <div>

        <table>
            <thead>
                <tr>
                    <td>
                        <form @submit.prevent="handleSubmit">
                            <label  for="amount">Enter pay:</label>
                            <div  class="calculateBdgt">
                            <input type="text" id="amount" v-model="income.amount" placeholder="$0" class="bg-base-200 p-2"/>
                            <button type="submit" class="btn btn-primary ml-2">Calculate</button>
                            </div>
                        </form>
                    </td>
                </tr>

                
                <tr v-for="income in incomes" :key="income.id">
                    <th><label>Pay entered:</label>
                        {{ income.amount }}
                         <button class="btn btn-error" type="delete" @click="deleteIncome(income.id)">x</button>
                        </th>
                </tr>
            </thead>
        </table>
    </div>
        <table class=" table">
            <thead>
                <tr>
                    <th>Select</th>
                    <th class="name">Budget</th>
                    <th class="amount">Amount</th>

                </tr>

            </thead>
            <tbody>
                <tr v-for="budget in budgets" :key="budget.id">
                    <td> 
                        <input type="checkbox" v-model="budget.selected" :disabled="budget.addedToBank === true"/>
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
                    <td><p> </p></td>
                    <td class="amount totalDivided">{{totalDivided}}</td>
                </tr>
                <tr v-for="income in incomes" :key="income.id">
                    <td>Remaining:</td>
                    <td><p> </p></td>
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

<style scoped>
.calculateBdgt{
    display: flex;
    flex-direction: row;
}
.totalDivided{
    font-weight: bold;
}
.name{
    font-weight: bold;
}
</style>

