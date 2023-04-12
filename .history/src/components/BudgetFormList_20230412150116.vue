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
            <bank-transfer  @add:bank="addBanks" :banks="banks" :budgets="budgets"/>
        </div>
    </div>
</template>
