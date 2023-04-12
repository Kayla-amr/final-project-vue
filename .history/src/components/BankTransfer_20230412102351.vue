<template>
    <div>
        <form @submit.prevent="handleSubmit">
            <table>
                <thead>
                    <tr>
                        <th>
                            <label for="name">Bank Name:</label>
                            <input type="text" id="bankName" v-model="bank.name" placeholder="Bank Name" />
                            <button type="submit">+</button>
                        </th>
                    </tr>
                </thead>
            </table>
        </form>
    </div>
    <div>
        <table>
            <tbody>
                <tr v-for="bank in banks" :key="bank.id">
                    <td><button @click="transferSelectedBudget(bank.amount)">Add Amount</button></td>
                    <td>{{ bank.name }}</td>
                    <td>{{ bank.amount }}</td>
                    <td> <button type="delete" @click="removeBank(bank.id)">-</button></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

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
        transferSelectedBudget() {
            const selectedBudgets = this.budgets.filter(budget => budget.selected);
            const totalSelectedBudgetsAmount = selectedBudgets.reduce((total, budget) => {
                return Math.ceil(total + budget.amount / 2);
            }, 0);

            if (selectedBudgets.length > 0) {
                // Find the bank option to add to
                const bankToAddTo = this.banks.find(bank => bank.name === "My Bank");
                if (bankToAddTo) {
                    // If the bank option exists, update its amount property
                    bankToAddTo.amount += totalSelectedBudgetsAmount;
                } else {
                    // Otherwise, create a new bank option with the total amount
                    this.banks.push({
                        name: "My Bank",
                        amount: totalSelectedBudgetsAmount
                    });
                }
            }
        }

    }
}
</script>