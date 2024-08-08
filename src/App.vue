<template>
  <header>
    <!--Usar un router link es similar a usar <a href>-->
    <router-link to="/">
      <h1><span>Movies</span>App</h1>
    </router-link>
  </header>
  <main>
    <router-view />
  </main>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', 'sans-serif';
  /*
  &::selection{
    background: transparentize(#42B883, 0.5);
  }*/
}

body {
  background-color: #35495E;
}

a {
  text-decoration: none; //quitamos la decoracion en los enlaces, en este caso el del router link
}

header {
  display: flex; //para que se mueva el elemento
  align-items: center;
  justify-content: center; //indica la alineacion
  padding: 20px 16px; //10 para arriba, 16 a los costados
  background-color: #2c3d4e;
  box-shadow: 0px 0px 6px rgba(0, 0, 0, 1); //sombra hacia abajo

  h1{
    color: #FFF;
    font-size: 28px;
  

    span{
      color: #42b883; //coloca en verde el span
    }
  }
}
</style>
