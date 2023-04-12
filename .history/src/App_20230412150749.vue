<script setup>
import OutcomeForm from './components/OutcomeForm.vue';
import DocumentBudget from './components/DocumentBudget.vue';
import SplitBudget from './components/SplitBudget.vue';
import BankTransfer from './components/BankTransfer.vue';
</script>

<template>
    
    <div id="app" class="">
        <div class="card w-96 bg-base-100 shadow-xl">
  <div class="card-body">
    <h2 class="card-title">Card title!</h2>
    <p>If a dog chews shoes whose shoes does he choose?</p>
    <div class="card-actions justify-end">
      <button class="btn btn-primary">Buy Now</button>
    </div>
  </div>
</div>
        <div class="budgetFormList">
        </div>
        <div class="incomeSplitList">
            <split-budget :budgets="budgets" @add:income="addIncomes" :incomes="incomes"/>
        </div>
        <div class="bankTransferList" >
            <bank-transfer  @add:bank="addBanks" :banks="banks" :budgets="budgets"/>
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
                    selected: false,
                    addedToBank: false
                },
                {
                    id: 2,
                    name: "Rent",
                    amount: 687,
                    selected: false,
                    addedToBank: false
                },
                {
                    id: 3,
                    name: "Electric",
                    amount: 5,
                    selected: true,
                    addedToBank: false
                },
                {
                    id: 4,
                    name: "Disney+",
                    amount: 10,
                    selected: false,
                    addedToBank: false
                },
                {
                    id: 5,
                    name: "Apple Music",
                    amount: 5.99,
                    selected: true,
                    addedToBank: false
                },
                
                {
                    id: 6,
                    name: "Phone",
                    amount: 25,
                    selected: true,
                    addedToBank: false
                },
                {
                    id: 7,
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
            

            const budgetToBank = this.budgets.find(budget => budget.id === bank.id)
            budgetToBank.addedToBank = true;
            this.banks = [...this.banks, newBanks]
        },

    }
}
</script>
