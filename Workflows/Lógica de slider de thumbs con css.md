```
.contenedor-corto
  .arrow.left(ng-click="mueveAtras()")
  .contenedor-largo(class="activo-{{imgActivo}}")
    .imagen(ng-repeat="img in elementos track by $index")
      span {{img.src}}
  .arrow.right(ng-click="mueveAdelante()")
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
-------
```
$scope.elementos = [
    {
      src: "img"
    },
    {
      src: "img"
    },
    {
      src: "img"
    },
    {
      src: "img"
    },
    {
      src: "img"
    },
    {
      src: "img"
    },
    {
      src: "img"
    }
  ];

  $scope.imgActivo = 0;
  $scope.cantElemVisibles = 3;

  $scope.mueveAtras = () => {
    if ($scope.imgActivo - 3 > 0) {
      $scope.imgActivo -= 3;
    } else {
      $scope.imgActivo = 0;
    }
  };
  $scope.mueveAdelante = () => {
    if (
      $scope.imgActivo + 3 <
      $scope.elementos.length - $scope.cantElemVisibles
    ) {
      $scope.imgActivo += 3;
    } else {
      $scope.imgActivo = $scope.elementos.length - $scope.cantElemVisibles;
    }
  };
```
