<template>
  <div class="home-container">
    <h1>会計管理アプリ</h1>
    
    <!-- 飲料と価格を入力するフォーム -->
    <div class="input-group">
      <label for="drink-name">飲料名:</label>
      <input type="text" id="drink-name" placeholder="飲料名を入力" />

      <label for="drink-price">価格 (¥):</label>
      <input type="number" id="drink-price" placeholder="価格を入力" />

      <button>飲料を追加</button>
    </div>

    <!-- 参加者と消費量を入力するフォーム -->
    <div class="input-group">
      <label for="participant-name">参加者名:</label>
      <input type="text" id="participant-name" placeholder="参加者名を入力" />

      <label for="drink-quantity">消費量 (杯):</label>
      <input type="number" id="drink-quantity" placeholder="消費量を入力" />

      <button>参加者を追加</button>
    </div>

    <!-- 支払い額を計算するボタン -->
    <button class="calculate-button">支払い額を計算</button>

    <!-- 計算結果を表示するエリア -->
    <div class="results">
      <h2>計算結果</h2>
      <!-- 計算結果がここに表示される -->
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld'
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

.input-group label {
  margin-right: .5rem;
}

.input-group input {
  margin-right: .5rem;
}

.calculate-button {
  display: block;
  margin: 2rem auto;
  padding: .5rem 2rem;
}

.results {
  margin-top: 2rem;
  padding: 1rem;
  background-color: #f7f7f7;
  border: 1px solid #ddd;
}
</style>

