<template>
    <nav class="navigation-bar">
      <ul>
        <li><router-link to="/">ホーム</router-link></li>
      </ul>
    </nav>
  </template>
  
  <script>
  export default {
    name: 'NavigationBar'
  };
  </script>
  
  <style>
  .nav-bar {
  background-color: #f0f0f0;
  padding: 10px 20px;
}

.nav-bar a {
  margin-right: 10px;
  text-decoration: none;
  color: #333;
}

.nav-bar a.router-link-exact-active {
  color: #000;
  text-decoration: underline;
}
  </style>
  