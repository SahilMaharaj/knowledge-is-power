<template>
  <nav>
    <div id="logo">
      <h1>Knowledge Graph Search</h1>
    </div>
  </nav>  
</template>

<style scoped>
nav {
  margin: 10px 0;
}

#logo {
  font-family: 'Helvetica Neue Heavy';
  font-size: 24px;  
}

h1 {
  background: #f09433;
  background: linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
  background: -moz-linear-gradient(45deg, #f09433 0%, #e6683c 25%, #dc2743 50%, #cc2366 75%, #bc1888 100%); 
  background: -webkit-linear-gradient(45deg, #f09433 0%,#e6683c 25%,#dc2743 50%,#cc2366 75%,#bc1888 100%);
  background-clip: text;
	-webkit-background-clip: text;
  color: transparent;
  padding: 5px 0;
}

@media all and (max-width: 500px) {
  h1 {
    font-size: 1.2em;
    line-height: 24px;
  }
}
</style>
