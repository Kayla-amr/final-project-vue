<script setup>
import OutcomeForm from './components/OutcomeForm.vue';
import DocumentBudget from './components/DocumentBudget.vue';
import SplitBudget from './components/SplitBudget.vue';
import BankTransfer from './components/BankTransfer.vue';
</script>

<template>
    <div id="app">
        <div class="layoutapp">
            <h1 class="text-5xl font-bold text-center">Budget Tracking</h1>
            <h2 class="text-2xl font-bold text-center">Create, Edit, and Track Your Budget</h2>
            <p>
                A simple budget tracking app that allows you to create a list of expenses, calculate the divided expense's with bi-weekly pay, and add the selected expense's to your bank.
            </p>
        <div class="steps">
        <div class="card w-96 bg-base-100 shadow-xl m-2">
            <div class="card-body flex-col items-center justify-center">
                <h2 class="card-title">Create</h2>
                <p> 
                    Create a list of expenses, edit the name and amount of each expense.
                </p>
                <outcome-form @add:budget="addBudgets" />
                <document-budget :budgets="budgets" />
            </div>
        </div>

        <div class="card w-96 bg-base-100 shadow-xl m-2">
            <div class="card-body flex-col items-center">
                <h2 class="card-title"
                >Calculate</h2>
                <p> 
                    Calculate the divided expense's with bi-weekly pay.
                </p>
                <split-budget :budgets="budgets" @add:income="addIncomes" :incomes="incomes" />
            </div>
        </div>

        <div class="card w-96 bg-base-100 shadow-xl m-2">
            <div class="card-body flex-col items-center">
                <h2 class="card-title">Add</h2>
                <p> 
                    Add the selected expense's to your bank.
                    You can always create / delete a bank 
                </p>
                <bank-transfer @add:bank="addBanks" :banks="banks" :budgets="budgets" />
            </div>
        </div>
    </div>
    </div>

    </div>
</template>

<script>
export default {
    name: 'app',
    components: {
        OutcomeForm,
        DocumentBudget,
        SplitBudget,
        BankTransfer
    },
    data() {
        return {
            budgets: [
                {
                    id: 1,
                    name: "Rent",
                    amount: 687,
                    selected: false,
                    addedToBank: false
                },
                {
                    id: 2,
                    name: "Electric",
                    amount: 5,
                    selected: true,
                    addedToBank: false
                },
                {
                    id: 3,
                    name: "Disney+",
                    amount: 10,
                    selected: false,
                    addedToBank: false
                },

                {
                    id: 4,
                    name: "Phone",
                    amount: 25,
                    selected: true,
                    addedToBank: false
                },
                {
                    id: 5,
                    name: "Computer",
                    amount: 100,
                    selected: false,
                    addedToBank: false
                }
            ],
            incomes: [
                {
                    id: 1,
                    amount: 1000,
                }

            ],
            banks: [
                {
                    id: 1,
                    name: "Chase",
                    amount: 0,
                },
                {
                    id: 2,
                    name: "America First",
                    amount: 0,
                }
            ]
        }
    },

    //map method  or filter

    //property in object true or false



    methods: {
        addBudgets(budget) {
            const addId = this.budgets.length > 0 ? this.budgets[this.budgets.length - 1].id : 0;
            const id = addId + 1;

            const newBudgets = {
                id,
                ...budget
            }

            this.budgets = [...this.budgets, newBudgets]

        },
        addIncomes(income) {
            const addId = this.incomes.length > 0 ? this.incomes[this.incomes.length - 1].id : 0;
            const id = addId + 1;

            const newIncomes = {
                id,
                ...income
            }

            this.incomes = [...this.incomes, newIncomes]
        },
        addBanks(bank) {
            const addId = this.banks.length > 0 ? this.banks[this.banks.length - 1].id : 0;
            const id = addId + 1;

            const newBanks = {
                id,
                ...bank
            }


            const budgetToBank = this.budgets.find(budget => budget.id === bank.id)
            budgetToBank.addedToBank = true;
            budgetToBank = b
            this.banks = [...this.banks, newBanks]
        },

    }
}
</script>

<style>
#app {
    font-family: 'Nunito', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background-color: #f5f5f5;
    padding: 2%;
}
.steps {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: center;
    background-color: #f5f5f5;

}

th, td, input{
    text-align: center;
}
input {
    border-radius: 10px;
    width: 100%;
}

td.name, td.amount {
    padding: 0;
}
td.button{
    padding: 5px;
}
table.table{
    width: 100%;
}

.card-body p{
    flex-grow: unset;
}


</style>
