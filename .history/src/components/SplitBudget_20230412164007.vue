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
        <table class="budgetDivided table">
            <thead>
                <tr>
                    <th>
                        <form @submit.prevent="handleSubmit">
                            <label  for="amount">Enter pay:</label>
                            <div  class="calculateBdgt">
                            <input type="text" id="amount" v-model="income.amount" placeholder="$0" class="bg-base-200 p-2"/>
                            <button type="submit" class="btn btn-primary ml-2">Calculate</button>
                            </div>
                        </form>
                    </th>

                
                <th v-for="income in incomes" :key="income.id">
                    <label>Pay entered:</label>
                        {{ income.amount }}
                         <button class="btn btn-error" type="delete" @click="deleteIncome(income.id)">x</button>
                        
                </th>
                    <th>
                        Budget Split
                    </td>

                </tr>

            </thead>
            <tbody>
                <tr v-for="budget in budgets" :key="budget.id">
                    <th> 
                        <input type="checkbox" v-model="budget.selected" :disabled="budget.addedToBank === true"/>
                    </th>
                    <th class="name">{{ budget.name }}</th>
                    <th>
                        {{ splitBudget(budget.amount) }}
                    </th>
                </tr>

            </tbody>
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
.budgetDivided{
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    height: 100%;
}
.totalDivided{
    font-weight: bold;
}
.name{
    font-weight: bold;
}
</style>

