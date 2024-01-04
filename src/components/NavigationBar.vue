<template>
    <nav class="navigation-bar">
      <ul>
        <li><router-link to="/">ホーム</router-link></li>
        <li><router-link to="/about">アバウト</router-link></li>
      </ul>
    </nav>
  </template>
  
  <script>
  export default {
    name: 'NavigationBar'
  };
  </script>
  
  <style>
  .navigation-bar ul {
    list-style-type: none;
    padding: 0;
  }
  
  .navigation-bar li {
    display: inline;
    margin-right: 20px;
  }
  </style>
  