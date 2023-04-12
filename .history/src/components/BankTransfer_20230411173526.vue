<template>
    <div>
        <form @submit.prevent="handleSubmit">
            <table>
                <thead>
                    <tr>
                        <th>
                            Transfer
                        </th>
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

                <tr v-for="bank in banks" :key="bank.id" @click="selectBank(bank)">
                    <td>
                        <button @click=""
                        >
                    </td>
                    <td>{{ bank.name }}</td>
                    <td>{{ bank.amount }}</td>
                </tr>

            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'bank-transfer',
    props: {
        banks: Array
    },
    emits: ['add:bank'],
    computed: {
        total() {
            return this.banks.reduce((total, bank) => total + bank.amount, 0)
        }

    },
    data() {
        return {
            bank: {
                name: '',
                amount: 0
            },
            selectedBudget: null,
            selectedBank: null,
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
        selectBudget(budget) {
            this.selectedBudget = budget;
        },
        selectBank(bank) {
            this.selectedBank = bank;
        }

    }
}
</script>