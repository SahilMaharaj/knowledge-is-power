<template>
  <div id="container">
    <Header />
    <Nuxt />
  </div>
</template>


<style>
@font-face {
  font-family: 'Helvetica Neue Heavy';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url('~/static/fonts/HelveticaNeueCyr-Heavy.woff2');
}

@font-face {
  font-family: 'Helvetica Neue Medium';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url('~/static/fonts/HelveticaNeueCyr-Medium.woff2');
}

@font-face {
  font-family: 'Helvetica Neue Thin';
  font-style: normal;
  font-weight: 100;
  font-display: swap;
  src: url('~/static/fonts/HelveticaNeueCyr-Thin.woff2');
}

  html {
    font-family: 'Helvetica Neue Medium', 'Arial';
    font-size: 16px;
    word-spacing: 1px;
    -ms-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%;
    -moz-osx-font-smoothing: grayscale;
    -webkit-font-smoothing: antialiased;
    box-sizing: border-box;
}

  body {
    margin: 0;
    padding: 0;
  }

  *,
  *::before,
  *::after {
      box-sizing: border-box;
      margin: 0;
  }

  #container {
    max-width: 1280px;
    margin: 0 auto;
    padding: 0 15px;
  }

  
</style>