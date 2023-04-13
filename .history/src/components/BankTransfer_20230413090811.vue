<template>
    <div>
        <form @submit.prevent="handleSubmit">
            <table>
                <thead>
                    <tr>
                        <th>
                            <label for="name">Bank Name:</label>
                            <div class="bankName">
                            <input class="bg-base-200 p-2" type="text" id="bankName" v-model="bank.name" placeholder="Bank Name" />
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
            <tbody>
                <tr v-for="bank in banks" :key="bank.id">
                    <td>{{ bank.name }}</td>
                    <td>{{ bank.amount }}</td>
                    <div class="bankbtn flex">
                        <td class="button"><button class="btn btn-primary" type="transfer" @click="transferSelectedBudget(bank.id)">-></button></td>
                        <!-- The button to open modal -->
<label for="my-modal-3" class="btn">i</label>

<!-- Put this part before </body> tag -->
<input type="checkbox" id="my-modal-3" class="modal-toggle" />
<div class="modal">
  <div class="modal-box relative">
    <label for="my-modal-3" class="btn btn-sm btn-circle absolute right-2 top-2">✕</label>
    <h3 class="text-lg font-bold">Congratulations random Internet user!</h3>
    <p class="py-4">You've been selected for a chance to get one year of subscription to use Wikipedia for free!</p>
  </div>
</div>
                        <td class="button"> <button class="btn btn-error" type="delete" @click="removeBank(bank.id)">x</button></td>
                    </div>
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
            const selectedBudgets = this.budgets.filter(budget => budget.selected);
            const statusChange = selectedBudgets.map(budget => {
                budget.addedToBank = true;
                return budget;
            });
            const totalSelectedBudgetsAmount = selectedBudgets.reduce((total, budget) => {
                return Math.ceil(total + budget.amount / 2);
            }, 0);

            if (selectedBudgets.length > 0) {
                const bank = this.banks.find(bank => bank.id === id);
                bank.amount += totalSelectedBudgetsAmount;
                statusChange.forEach(budget => {
                    budget.addedToBank = true;
                });
            }
            
        },
        clearTransfer() {
            this.banks.forEach(bank => {
                bank.amount = 0;
            })
            this.budgets.forEach(budget => {
                budget.addedToBank = false;
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