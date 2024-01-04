<template>
  <div class="home-container">
    <h1>会計管理アプリ</h1>

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
          <li v-for="(participant, index) in participants" :key="participant">
            {{ participant }}
            <button @click="removeParticipant(index)">削除</button>
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
          <li v-for="(drink, index) in drinks" :key="drink.name">
            {{ drink.name }} - ¥{{ drink.price }}
            <button @click="removeDrink(index)">削除</button>
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
        <input type="number" placeholder="消費量を入力(杯)" v-model.number="drinkAmount" />
        <button @click="calculateConsumption">計算を追加</button>
      </div>
    </section>

    <!-- 計算結果の表示セクション -->
    <section class="results">
      <h2>計算結果</h2>
      <ul>
        <li v-for="result in consumptionResults" :key="result.participant">
          {{ result.participant }}: {{ result.amount }} 杯
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
      newParticipantName: '', // 新しい参加者名の入力用
      participants: [], // 参加者リスト
      newDrink: { // 新しい飲料の情報
        name: '',
        price: ''
      },
      drinks: [], // 飲料リスト
      selectedParticipant: '', // 選択された参加者
      drinkAmount: '', // 飲料の消費量
      consumptionResults: [] // 消費量計算結果のリスト
    };
  },
  methods: {
    // 参加者を追加するメソッド
    addParticipant() {
      if (this.newParticipantName) {
        this.participants.push(this.newParticipantName);
        this.newParticipantName = '';
      }
    },
    removeParticipant(index) {
      this.participants.splice(index, 1);
    },
     // 飲料を追加するメソッド
    addDrink() {
      if (this.newDrink.name && this.newDrink.price > 0) {
        this.drinks.push({ ...this.newDrink });
        this.newDrink.name = '';
        this.newDrink.price = 0;
      }
    },
    // 飲料消費量を計算するメソッド
    calculateConsumption() {
      if (this.selectedParticipant && this.drinkAmount > 0) {
        const result = {
          participant: this.selectedParticipant,
          amount: this.drinkAmount
        };
        this.consumptionResults.push(result);
        // フォームをリセット
        this.selectedParticipant = '';
        this.drinkAmount = 0;
      }
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
.results {
  margin-top: 2rem;
  padding: 1rem;
  background-color: #f7f7f7;
  border: 1px solid #ddd;
}

</style>
