<template>
  <div class="container">
    <header class="header">
    <div class="header__wrapper">
      <a href="#" class="logo">Logo</a>
      <nav class="header__nav">
        <ul class="header__nav-list">
          <li class="header__nav-item">Текст</li>
          <li class="header__nav-item">Текст</li>
          <li class="header__nav-item">Текст</li>
        </ul>
      </nav>
    </div>
    <div class="header__wrapper">
      <button class="button button_mr-20">Зарегестрироваться</button>
      <button class="button">Войти</button>
    </div>
  </header>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss">
  .logo {
    margin-right: 50px;
    color: #000;
    font-size: 35px;
    font-weight: 700;
    text-decoration: none;
  }
  .header {
    display: flex;
    justify-content: space-between;
    padding-top: 20px;
    padding-bottom: 20px;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;

    background: transparent;
  }

  .header__wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header__nav-list {
    display: flex;
    padding: 0;
    margin: 0;
    list-style: none;
  }

  .header__nav-item {
    margin-right: 40px;
  }

  .button {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 18px 30px;

    border-radius: 10px;
    border: 1px solid #FFFFFF;
    background-color: Transparent;

    color: #fff;
    font-weight: 700;
    font-size: 16px;

    cursor: pointer;
    &_mr-20 {
      margin-right: 20px;
    }
  }
</style>
