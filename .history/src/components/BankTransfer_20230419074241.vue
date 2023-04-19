<template>
    <div>
        <form @submit.prevent="handleSubmit">
            <table>
                <thead>
                    <tr>
                        <th>
                            <div class="bankName py-2">
                            <input class="bg-base-200 p-2" type="text" id="bankName" v-model="bank.name" placeholder="New Bank" />
                            <button class="btn btn-primary ml-2" type="submit">+</button>
                        </div>
                        </th>
                    </tr>
                </thead>
            </table>
        </form>
    </div>
    <div>
        <table class="table">
            <thead>
                <th>Bank Name</th>
                <th>Amount</th>
                <th>Transfer</th>
                <th>Remove</th>
            </thead>
            <tbody>
                <tr v-for="bank in banks" :key="bank.id">
                    <td>{{ bank.name }}</td>
                    <td>{{ bank.amount }}</td>
                        <td class="button"><button name="arrow-forward-outline" class="btn btn-primary" type="transfer" @click="transferSelectedBudget(bank.id)">+</button></td>
              
                    <td class="button"><button class="btn btn-error" type="delete" @click="removeBank(bank.id)">x</button></td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td><p></p></td>
                    <td><p></p></td>
                    <td><button class="btn btn-error" type="clear" @click="clearTransfer">Clear</button></td>
                </tr>
            </tfoot>
        </table>
    </div>
</template>

//balance, cost, end of month balance, red style of negative balance, green style of positive balance, dropdown to show individual expenses they added

<script>
export default {
    name: 'bank-transfer',
    props: {
        banks: Array,
        budgets: Array
    },
    emits: ['add:bank'],
    computed: {
        total() {
            return this.banks.reduce((total, bank) => {
                return total + bank.amount
            }, 0)
        }

    },
    data() {
        return {
            bank: {
                name: '',
                amount: 0
            }
        }
    },
    methods: {
        removeBank(id) {
            const index = this.banks.findIndex(bank => bank.id === id)
            // eslint-disable-next-line vue/no-mutating-props
            this.banks.splice(index, 1)
        },
        handleSubmit() {
            this.$emit('add:bank', this.bank)
            this.bank = {
                name: "",
                amount: 0
            }
        },
        transferSelectedBudget(id) {
    const selectedBudgets = this.budgets.filter(budget => budget.selected && !budget.addedToBank);

    const totalSelectedBudgetsAmount = selectedBudgets.reduce((total, budget) => {
        return Math.ceil(total + budget.amount / 2);
    }, 0);

    if (selectedBudgets.length > 0) {
        const bank = this.banks.find(bank => bank.id === id);
        bank.amount += totalSelectedBudgetsAmount;
        selectedBudgets.forEach(budget => {
            budget.addedToBank = true;
        });
    }
}
,
        clearTransfer() {
            this.banks.forEach(bank => {
                bank.amount = 0;
            })
            this.budgets.forEach(budget => {
                budget.addedToBank = false;
                budget.selected = false;
            })
        }

    }
}
</script>

<style scoped>
.bankName {
    display: flex;
    flex-direction: row;
}

</style>