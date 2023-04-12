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
          <td><button @click="transferSelectedBudget(bank.id)">Add Amount</button></td>
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
  name: "BankTransfer",
  data() {
    return {
      bank: {
        name: "",
        amount: 0,
      },
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
        },
      ],
      selectedBudget: null,
    };
  },
  methods: {
    handleSubmit() {
      this.$emit("add:bank", this.bank);
      this.bank.name = "";
      this.bank.amount = 0;
    },
    transferSelectedBudget(bankId) {
      if (this.selectedBudget === null) {
        alert("Please select a budget to transfer.");
        return;
      }

      const budget = this.banks.find((b) => b.id === bankId);

      budget.amount += this.selectedBudget.amount;
      this.selectedBudget = null;
    },
    removeBank(id) {
      const index = this.banks.findIndex((bank) => bank.id === id);
      if (index > -1) {
        this.banks.splice(index, 1);
      }
    },
  },
};
</script>
