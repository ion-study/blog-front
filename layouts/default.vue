<template>
  <div class="container">
    <div>
      <div class="header">
        <h2>I-ON blog project</h2>
      </div>
      <nuxt />
      <div class="footer">
        <h5>I-ON Communications - daisy / jayden / ann</h5>
      </div>
    </div>
  </div>
</template>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
  width:100%;
  max-width: 165px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
  width:100%;
  max-width: 165px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}

.container {
  margin: 0 auto;
  //min-height: 100vh;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.header{
  width:100%;
  margin: 20px 0;
  border:1px solid #acacac;
}
.footer{
  width:100%;
  margin: 20px 0;
  padding: 10px;
  text-align:right;
  border:1px solid #acacac;
}

.c-green{
  color:#3b8070;
}
.pb30{
padding-bottom:30px;
}
.pt30{
padding-top:30px;
}
ul li{
list-style:none;
}
</style>
