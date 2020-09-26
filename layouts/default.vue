<template>
  <div>
    <video id="myVideo" autoplay muted loop>
      <source src="../assets/DJ Sona Animated Splash Preview.mp4" type="video/mp4">
    </video>
    <b-container>
      <b-breadcrumb>
        <b-breadcrumb-item to="/">
          <b-icon icon="house-fill" scale="1.25" shift-v="1.25" aria-hidden="true" />
          Home
        </b-breadcrumb-item>
      </b-breadcrumb>
      <b-jumbotron align="center">
        <template v-slot:header>
          Muse House
        </template>

        <template v-slot:lead>
          Search for your favorite song on Apple music
        </template>
        <b-img rounded="circle" src="../assets/logo.png" />
      </b-jumbotron>
    </b-container>

    <Nuxt />
  </div>
</template>

<style>
html {
  font-family:
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
  margin-top: 20px;
}
.jumbotron{
  position: relative;
}
.navbar{
  position:relative;
}
#myVideo {
  object-fit: cover;
  width: 100vw;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  overflow: hidden;
}

*,
*::before,
*::after {
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
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}

</style>
