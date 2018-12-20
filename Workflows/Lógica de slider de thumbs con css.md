```
.contenedor-corto(ng-init="imgActivo = 0")
  .arrow.left(ng-click="(imgActivo = imgActivo - 3)")
  .contenedor-largo(class="activo-{{imgActivo}}")
    .imagen
      span img
    .imagen
      span img
    .imagen
      span img
    .imagen
      span img
    .imagen
      span img
    .imagen
      span img
    .imagen
      span img
  .arrow.right(ng-click="(imgActivo = imgActivo + 3)")
```
-------
```
.contenedor-corto {
    width: 100vw;
    position: relative;
    height: 300px;
    background-color: black;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    overflow: hidden;
    .arrow {
      display: block;
      @include minmaxwh(50px);
      background-color: red;
      position: absolute;
      cursor: pointer;
      z-index: 5;
      &.left {
        left: 20px;
      }
      &.right {
        right: 20px;
      }
    }
    .contenedor-largo {
      height: 100%;
      display: flex;
      @include ease-transition;
      @for $i from 1 through 15 {
        &.activo-#{$i} {
          -webkit-transform: translateX($i * -33.333vw);
          transform: translateX($i * -33.333vw);
        }
      }
      .imagen {
        height: 100%;
        width: 33.333vw;
        min-width: 33.333vw;
        display: flex;
        justify-content: center;
        align-items: center;
        @include sombra(inset 0 0 10px 0 #000);
        background-color: green;
      }
    }
  }
```
