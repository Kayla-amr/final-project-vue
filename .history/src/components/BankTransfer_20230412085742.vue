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
                    <td><button @click="transferSelectedBudget">Transfer Selected Budget</button></td>
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
        banks: Array
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
        handleSubmit(){
            this.$emit('add:bank', this.bank)
            this.bank = {
                name: "",
                amount: 0
            }
        },
        transferSelectedBudget() {
    const selectedBudgets = this.budgets.filter(budget => budget.selected);
    const totalAmount = selectedBudgets.reduce((total, budget) => total + budget.amount, 0);
    const selectedBank = this.banks.find(bank => bank.selected);

    if (!selectedBank) {
      alert("Please select a bank to transfer the budget to.");
      return;
    }

    const updatedBudgets = this.budgets.map(budget => {
      if (budget.selected) {
        return {
          ...budget,
          selected: false,
          amount: 0,
        };
      } else {
        return budget;
      }
    });

    const updatedBanks = this.banks.map(bank => {
      if (bank.selected) {
        return {
          ...bank,
          amount: bank.amount + totalAmount,
        };
      } else {
        return bank;
      }
    });

    this.budgets = updatedBudgets;
    this.banks = updatedBanks;
  }     

        
    }
}
</script>