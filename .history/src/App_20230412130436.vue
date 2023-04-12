<script setup>
import OutcomeForm from './components/OutcomeForm.vue';
import DocumentBudget from './components/DocumentBudget.vue';
import SplitBudget from './components/SplitBudget.vue';
import BankTransfer from './components/BankTransfer.vue';


</script>

<template>
    
    <div id="app">
        <div class="budgetFormList">
            <outcome-form @add:budget="addBudgets" />
            <document-budget :budgets="budgets"/>
        </div>
        <div class="incomeSplitList">
            <split-budget :budgets="budgets" @add:income="addIncomes" :incomes="incomes"/>
        </div>
        <div class="bankTransferList" >
            <bank-transfer  @add:bank="addBanks" :banks="banks" :budgets="budgets" />
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
                    name: "Insurance",
                    amount: 101.84,
                    selected: false
                },
                {
                    id: 2,
                    name: "Rent",
                    amount: 687,
                    selected: false
                },
                {
                    id: 3,
                    name: "Electric",
                    amount: 5,
                    selected: true
                },
                {
                    id: 4,
                    name: "Disney+",
                    amount: 10,
                    selected: false
                },
                {
                    id: 5,
                    name: "Apple Music",
                    amount: 5.99,
                    selected: true
                },
                
                {
                    id: 6,
                    name: "Phone",
                    amount: 25,
                    selected: true
                },
                {
                    id: 7,
                    name: "Computer",
                    amount: 100,
                    selected: false
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
                    name: "Wells Fargo",
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

            this.banks = [...this.banks, newBanks]
        },

        splitAmount(){
            const handleChange = (event) =
        },
        

    }
}
</script>

<style>
#app {
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    height: 100%;
    color: #333;
}

.budgetFormList {
    width: 100%;
}

.incomeSplitList {
    width: 100%;
}
</style>