<template>
  <div class="home-container">
    <!-- 参加者の追加セクション -->
    <section>
      <h2>参加者の追加</h2>
      <div class="input-group">
        <input type="text" placeholder="参加者氏名を入力" v-model="newParticipantName" />
        <button @click="addParticipant">参加者を追加</button>
      </div>
      <!-- 参加者一覧の表示エリア -->
      <div class="participant-list">
        <h3>参加者一覧</h3>
        <ul>
          <li v-for="participant in participants" :key="participant">
            {{ participant }}
            <button @click="removeParticipant(participant)">削除</button>
          </li>
        </ul>
      </div>
    </section>

    <!-- 飲料の追加セクション -->
    <section>
      <h2>飲料の追加</h2>
      <div class="input-group">
        <input type="text" placeholder="飲料名を入力" v-model="newDrink.name" />
        <input type="number" placeholder="価格を入力" v-model.number="newDrink.price" />
        <button @click="addDrink">飲料を追加</button>
      </div>
      <!-- 飲料一覧の表示エリア -->
      <div class="drink-list">
        <h3>飲料一覧</h3>
        <ul>
          <li v-for="drink in drinks" :key="drink.name">
            {{ drink.name }} - ¥{{ drink.price }}
            <button @click="removeDrink(drink.name)">削除</button>
          </li>
        </ul>
      </div>
    </section>

    <!-- 飲料消費量の計算セクション -->
    <section>
      <h2>飲料消費量の計算</h2>
      <div class="input-group">
        <select v-model="selectedParticipant">
          <option disabled value="">参加者を選択</option>
          <option v-for="participant in participants" :key="participant" :value="participant">{{ participant }}</option>
        </select>
        <select v-model="selectedDrink">
          <option disabled value="">飲料を選択</option>
          <option v-for="drink in drinks" :key="drink.name" :value="drink.name">{{ drink.name }}</option>
        </select>
        <input type="number" placeholder="消費量を入力(杯)" v-model.number="drinkAmount" />
        <button @click="calculateConsumption">計算を追加</button>
      </div>
    </section>

    <!-- 飲料消費記録の表示セクション -->
    <section class="consumption-records">
      <h2>飲料消費記録</h2>
      <ul>
        <li v-for="(records, participant) in consumptionRecords" :key="participant">
          {{ participant }}: 
          <span v-for="(amount, drink) in records" :key="drink">
            {{ drink }} - {{ amount }} 杯
          </span>
        </li>
      </ul>
    </section>

    <!-- お会計金額の入力セクション -->
    <section>
      <h2>お会計金額</h2>
      <div class="input-group">
        <input type="number" placeholder="お会計金額を入力" v-model.number="totalBillAmount" />
      </div>
    </section>

    <!-- 計算結果の表示セクション -->
    <section class="results">
      <h2>計算結果</h2>
      <button @click="calculateEqualPayment">均等割り勘</button>
      <button @click="calculateIndividualPayment">個別割り勘</button>
      <ul v-if="showResults">
        <li v-for="result in paymentResults" :key="result.participant">
          {{ result.participant }}: ¥{{ result.payment }}
        </li>
    </ul>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      newParticipantName: '',
      participants: [],
      newDrink: {
        name: '',
        price: ''
      },
      drinks: [],
      selectedParticipant: '',
      selectedDrink: '',
      drinkAmount: '',
      totalBillAmount: '',
      showResults: false, // 計算結果の表示制御フラグ
      consumptionRecords: {},
      paymentResults: []
    };
  },
  methods: {
    addParticipant() {
      if (this.newParticipantName && !this.participants.includes(this.newParticipantName)) {
        this.participants.push(this.newParticipantName);
        this.newParticipantName = '';
      }
    },
    removeParticipant(participant) {
      this.participants = this.participants.filter(p => p !== participant);
    },
    addDrink() {
      if (this.newDrink.name && this.newDrink.price > 0 && !this.drinks.some(d => d.name === this.newDrink.name)) {
        this.drinks.push({ ...this.newDrink });
        this.newDrink = { name: '', price: 0 };
      }
    },
    removeDrink(drinkName) {
      this.drinks = this.drinks.filter(d => d.name !== drinkName);
    },
    calculateConsumption() {
      if (this.selectedParticipant && this.selectedDrink && this.drinkAmount > 0) {
        if (!this.consumptionRecords[this.selectedParticipant]) {
          this.consumptionRecords[this.selectedParticipant] = {};
        }
        if (!this.consumptionRecords[this.selectedParticipant][this.selectedDrink]) {
          this.consumptionRecords[this.selectedParticipant][this.selectedDrink] = 0;
        }
        this.consumptionRecords[this.selectedParticipant][this.selectedDrink] += this.drinkAmount;
        this.calculatePayments();
      }
    },
    calculatePayments() {
      let drinkCosts = {};
      for (const participant in this.consumptionRecords) {
        const drinks = this.consumptionRecords[participant];
        for (const drink in drinks) {
          const amount = drinks[drink];
          const drinkPrice = this.drinks.find(d => d.name === drink)?.price || 0;
          if (!drinkCosts[participant]) {
            drinkCosts[participant] = 0;
          }
          drinkCosts[participant] += drinkPrice * amount;
        }
      }

      // 食べ物のコストを均等に分割
      const foodCostPerPerson = (this.totalBillAmount - Object.values(drinkCosts).reduce((sum, cost) => sum + cost, 0)) / this.participants.length;

      // 各参加者の支払い額を計算（飲料代 + 食べ物の均等割）
      this.paymentResults = this.participants.map(participant => ({
        participant,
        payment: (drinkCosts[participant] || 0) + foodCostPerPerson
      }));
    },
    calculateEqualPayment() {
      const totalCostPerPerson = this.totalBillAmount / this.participants.length;
      this.paymentResults = this.participants.map(participant => ({
        participant,
        payment: totalCostPerPerson
      }));
      this.showResults = true;
    },
    calculateIndividualPayment() {
      this.calculatePayments();
      this.showResults = true;
    }
  }
}
</script>


<style>
.home-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  text-align: left;
}

.input-group {
  margin-bottom: 1rem;
}

.input-group input,
.input-group select {
  margin-right: .5rem;
}

.input-group button {
  margin-right: .5rem;
}

.participant-list,
.drink-list {
  margin-top: 1rem;
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  padding: 1rem;
}

.results,
.consumption-records {
  margin-top: 2rem;
  padding: 1rem;
  background-color: #f7f7f7;
  border: 1px solid #ddd;
}
</style>
