<template>
  <div class="container-sm">
    <div class="container-sm1">
      <header class="header">
        <div class="logo">
          <img src="https://seeklogo.com/images/Y/yamaha-mt-series-logo-CF96CCDF05-seeklogo.com.png?v=638133564910000000"
            alt="">
          <h2 class="nombre">MOTOS-EDG</h2>
        </div>
        <i class="fa fa-shopping-cart" @click="abrirModal" id="carrito" aria-hidden="true">
          <span class="cart-counter">{{ carrito.length }}</span></i>
        <div class="modal" v-if="mostrarTabla">
          <div class="modal-content">
            <h3 class="agregado">Agregados</h3>
            <div class="table-container">
              <table>
                <thead>
                  <tr>
                    <th>Imagen</th>
                    <th>Nombre</th>
                    <th>Precio</th>
                    <th>Cantidad</th>
                    <th>Sub total</th>
                    <th>Eliminar</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item, index) in carrito" :key="index">
                    <td><img :src="item.img" class="modal-img" alt="Producto"></td>
                    <td>{{ item.nombre }}</td>
                    <td>{{ formatoMoneda(item.precio) }}</td>
                    <td class="ctn">
                      <button @click="menos(item)">➖</button>
                      {{ item.cantidad }}
                      <button @click="mas(item)">➕</button>
                    </td>
                    <td>{{ calcularSubtotal(item) }}</td>
                    <td>
                      <button class="btn5" @click="eliminarProducto(index)">❌</button>
                    </td>
                  </tr>
                  <tr>
                    <td colspan="2">Total:</td>
                    <td>{{ calcularTotal() }}COP</td>
                  </tr>
                </tbody>
              </table>
              <button class="btn4" @click="cerrarModal">Cerrar</button>
              <button class="btn4" @click="limpiarModal">Limpiar</button>
            </div>
          </div>
        </div>
      </header>
      <div class="content">
        <h2 class="buscador">¿Qué deseas buscar?</h2>
        <div class="search-box">
          <input v-model="terminoBusqueda" type="text" class="input" placeholder="Buscar..." />
          <i class="fa fa-search" aria-hidden="true"></i>
        </div>
      </div>
    </div>
    <div class="container-sm11">
      <div class="sm11">
        <i class="fa fa-motorcycle" aria-hidden="true"></i>
        <a href="https://www.incolmotos-yamaha.com.co/home/" target="_blank">Motos y Mas</a>
      </div>
      <div class="sm11">
        <i class="fa fa-wrench" aria-hidden="true"></i>
        <a href="https://goo.gl/maps/1132VqfErsskq39TA" target="_blank">Reparaciones y Repuestos</a>
      </div>
      <div class="sm11">
        <i class="fa fa-map" aria-hidden="true"></i>
        <a href="https://goo.gl/maps/xv5qBvDN2beHyEBX7" target="_blank">Ubicacion de Locales</a>
      </div>
    </div>
    <div class="container-sm13">
      <h4 class="sm13">Deportivas</h4>
      <h4 class="sm13">Super Deportivas</h4>
      <h2 class="sm13">CATALOGO DE MOTOS</h2>
      <h4 class="sm13">Urbanas</h4>
      <h4 class="sm13">Todoterreno</h4>
    </div>
    <div id="ctarjetas" class="tarjetas-grid">
      <div v-for="(tarjetas, i) in filtroTarjeta" :key="i" class="tarjeta">
        <img class="imagentarjeta" :src="tarjetas.img" alt="">
        <div class="informacion">
          <h1 class="info2">{{ tarjetas.nombre }}</h1>
          <h1 class="info">Modelo: {{ tarjetas.Modelo }}</h1>
          <h1 class="info">cantidad: {{ tarjetas.cantidad }}</h1>
          <div class="icon">
            <div class="rating">
              <input value="5" name="rating" id="star5" type="radio">
              <label for="star5"></label>
              <input value="4" name="rating" id="star4" type="radio">
              <label for="star4"></label>
              <input value="3" name="rating" id="star3" type="radio">
              <label for="star3"></label>
              <input value="2" name="rating" id="star2" type="radio">
              <label for="star2"></label>
              <input value="1" name="rating" id="star1" type="radio">
              <label for="star1"></label>
            </div>
          </div>
          <div class="carrito2">
            <h1 class="info">{{ formatoMoneda(tarjetas.precio) }}</h1>
            <br>
            <button class="btn" @click="agregarCarrito(tarjetas)" v-if="!tarjetas.agregado">🛒</button>
            <button class="btn2" v-else>agregado</button>
          </div>
        </div>
      </div>
    </div>
    <div class="container-sm12">
      <div class="copy"> &copy; Edwin Dominguez</div>
    </div>
  </div>
</template>
  
<script setup>
import { ref, computed } from "vue";
const tarjeta = ref([
  { nombre: "YAMAHA MT-09", precio: "55000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://motos-b60.kxcdn.com/sites/default/files/yamaha_mt09_2019.jpg" },
  { nombre: "YAMAHA MT-15", precio: "15000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://static.wixstatic.com/media/3104c9_0bfdde80427c44349e13126347c87b8b~mv2.png/v1/fill/w_480,h_480,al_c,q_85,usm_0.66_1.00_0.01,enc_auto/3104c9_0bfdde80427c44349e13126347c87b8b~mv2.png" },
  { nombre: "YAMAHA MT-07", precio: "35000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://www.incolmotos-yamaha.com.co/wp-content/uploads/2019/05/mt07-gris.jpg" },
  { nombre: "YAMAHA MT-10", precio: "65000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://motos-b60.kxcdn.com/sites/default/files/yamaha-mt10-2022.jpg" },
  { nombre: "YAMAHA MT-03", precio: "45000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://www.yamaha-motor.com.mx/images/motos/mt03abs23_2.jpg" },
  { nombre: "YAMAHA R1",    precio: "100000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://www.incolmotos-yamaha.com.co/wp-content/uploads/2020/02/R1-2022-COLOR_AZUL.jpg" },
  { nombre: "YAMAHA R6",    precio: "60000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://s3.eu-west-1.amazonaws.com/cdn.motorbikemag.es/wp-content/uploads/2020/11/Yamaha-R6-RACE-2021-2.jpg" },
  { nombre: "YAMAHA R15",   precio: "14000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://dhqlmcogwd1an.cloudfront.net/images/phocagallery/yamaha/yzf-r15/01-yamaha-yzf-r15-2022-estudio-azul-620.jpg" },
  { nombre: "YAMAHA FZ 250", precio: "14000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://meyermotosyamaha.com/wp-content/uploads/2018/02/fz25_azul.jpg" },
  { nombre: "YAMAHA FZ 2.0", precio: "10000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://www.yamaha-motor.com.mx/images/motos/fz_2023_3.jpg" },
  { nombre: "YAMAHA XTZ125", precio: "9000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://www.yamahamotos.cl/wp-content/uploads/2019/06/XTZ-01-azul.jpg" },
  { nombre: "YAMAHA XTZ150", precio: "12000000", cantidad: "1", Modelo: "2019", año: 2023, img: "https://www.yamahamotos.cl/wp-content/uploads/2018/07/azul1.jpg" },
]);
const carrito = ref([]);
const mostrarTabla = ref(false);
const terminoBusqueda = ref('');

const filtroTarjeta = computed(() => {
  return terminoBusqueda.value === ''
    ? tarjeta.value
    : tarjeta.value.filter(item => item.nombre.toLowerCase().includes(terminoBusqueda.value.toLowerCase()));
    
});

const agregarCarrito = (item) => {
  carrito.value.push(item);
  item.agregado = true;
};

const eliminarProducto = (index) => {
  const removedItem = carrito.value.splice(index, 1)[0];
  removedItem.agregado = false;

  const tarjetaIndex = tarjeta.value.findIndex(item => item.nombre === removedItem.nombre);
  if (tarjetaIndex !== -1) {
    tarjeta.value[tarjetaIndex].cantidad = "1";
  }
};

const limpiarModal = () => {
  carrito.value.forEach(item => {
    item.agregado = false;
    item.cantidad = 1;
  });
  carrito.value = [];
  /* cerrarModal() */
};

const abrirModal = () => {
  mostrarTabla.value = true;
};

const cerrarModal = () => {
  mostrarTabla.value = false;
};

const mas = (item) => {
  item.cantidad++;
};

const menos = (item) => {
  if (item.cantidad > 0) {
    item.cantidad--;
  }
};

const calcularSubtotal = (item) => {
  return formatoMoneda(parseFloat(item.precio) * item.cantidad);
};

const calcularTotal = () => {
  let total = 0;
  for (const item of carrito.value) {
    total += parseFloat(item.precio) * item.cantidad;
  }
  return formatoMoneda(total);
};

const formatoMoneda = (item2) => {
  const formatter = new Intl.NumberFormat("es-CO", {
    style: "currency",
    currency: "COP",
    minimumFractionDigits: 0,
  });
  return formatter.format(item2);
};

</script>
  
<style scoped>
.input6 {
  color: #000000;
  width: 35px;
}

.carrito2 {
  display: flex;
  justify-content: space-between;
  background: #0a0233a6;
}

.icon {
  border-bottom: 1px solid black;
}

.cart-counter {
  position: relative;
  left: 30px;
  bottom: 45px;
  background-color: rgb(0, 0, 0);
  color: rgb(255, 255, 255);
  font-size: 12px;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container-sm {
  max-width: 100%;
  height: 100vh;
}

.header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #00000080;
}

.header i {
  color: rgb(255, 255, 255);
  font-size: 40px;
  margin-right: 150px;
}

.nombre {
  color: #ffffff;
  margin-left: 150px;
}

.logo {
  display: flex;
  align-items: center;
}

.logo img {
  position: absolute;
  background-position: center;
  background-size: contain;
  height: 70px;
  left: 90px;
}

#carrito {
  margin-top: 15px;
}

.container-sm1 {
  position: relative;
  height: 550px;
  overflow: hidden;
}

.container-sm1::before {
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url("imagenes/mt6.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  z-index: -5;
}

.agregado {
  text-align: center;
}

.container-sm11 {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  background: linear-gradient(25deg, rgb(0, 0, 0), rgb(0, 30, 45));
  height: 8%;
}

.sm11 {
  display: flex;
  align-items: center;
  margin-left: 10%;
  margin-right: 10%;
  color: rgb(255, 255, 255);
}

.container-sm13 {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 25px;
  background: linear-gradient(90deg,
      rgb(1, 24, 35),
      rgba(205, 203, 203, 0.79),
      rgba(205, 203, 203, 0.79),
      rgba(205, 203, 203, 0.79),
      rgb(1, 24, 35));
}

.sm13 {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
  color: rgb(6, 8, 72);
  flex-grow: 1;
  border-radius: 2px;
  background: linear-gradient(30deg, rgba(255, 255, 255, 0.723), rgba(0, 0, 0, 0.033));
  border-right: 2px solid rgb(3, 14, 53);
}

.sm11 i {
  margin-right: 10px;
  font-size: 25px;
}

.sm11 a {
  cursor: pointer;
  text-decoration: solid;
  color: rgb(72, 93, 255);
}

.container-sm12 {
  background-color: rgb(255, 255, 255);
  height: 8%;
}

.content {
  display: grid;
  text-align: center;
  margin-top: 200px;
  justify-content: center;
  align-items: center;
}

.buscador {
  color: white;
  font-size: 35px;
  margin-bottom: 10px;
}

.search-box {
  display: flex;
  align-items: center;
  background-color: #f2f2f2c7;
  border: none;
  border-radius: 5px;
  padding: 5px;
}

.search-box input {
  width: 400px;
  background: #c5c1c107;
  border: none;
  padding: 8px;
  border-radius: 5px;
}

.search-box i {
  color: #333;
  font-size: 18px;
  margin-left: 5px;
  cursor: pointer;
}

.table-container {
  max-height: 400px;
  overflow-y: auto;
  transition: bottom 1.3s ease-in-out;
}

.tarjetas-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(270px, 2fr));
  gap: 60px;
  background-image: url("imagenes/mt2.jpg");
  background-size: cover;
  background-position: center;
  object-fit: cover;
  padding: 29px;
}

.tarjeta {
  background-color: #504f4fad;
  border: 2px solid #ccc;
  border-radius: 10px;
  width: 102%;
  height: 100%;
  color: #ffffff;
  box-shadow: 7px 9px 0px rgba(9, 13, 44, 0.644);
}

.imagentarjeta {
  width: 100%;
  height: 250px;
  margin-bottom: 10px;
  border-radius: 10px;
  border-bottom: 4px solid #2e2e2e2a;
}

.informacion {
  width: 100%;
  border-radius: 5px;
  background-color: #56575731;
}

.info {
  font-size: 18px;
  margin-left: 8px;
}

.info2 {
  font-size: 18px;
  text-align: center;
  margin-bottom: 25px;
}

.btn {
  padding: 0px 13px;
  background-color: #ffffff;
  color: white;
  height: 50px;
  border-radius: 50%;
  border: 2px solid rgba(187, 183, 183, 0.685);
  cursor: pointer;
  transition: background-color 0.6s;
}

.btn:hover {
  background-color: #1dbe04;
}

.btn2 {
  padding: 5px 5px;
  background-color: #40ff00;
  color: white;
  border-radius: 5px;
  border: none;
  cursor: pointer;
}

.btn4 {
  margin-left: 35px;
  margin-right: 20%;
  margin-top: 5px;
  padding: 10px 20px;
  background-color: #07132bf3;
  color: rgb(255, 255, 255);
  border: none;
  cursor: pointer;
}

.modal {
  position: absolute;
  top: 55px;
  right: 25px;
  transition: right 0.3s;
}

.modal-img {
  width: 65px;
  height: 50px;
  object-fit: cover;
  border-radius: 5px;
}

.modal-content {
  background: #ffffffd0;
  border-radius: 5px;
  z-index: 1;
  text-align: center;
}

.rating {
  display: inline-block;
  margin-left: 7px;
}

.rating input {
  display: none;
}

.rating label {
  float: right;
  cursor: pointer;
  color: #ffffff;
  transition: color 0.3s;
}

.rating label:before {
  content: '\2605';
  font-size: 30px;
}

.rating input:checked~label,
.rating label:hover,
.rating label:hover~label {
  color: #f1d21e;
  transition: color 0.6s;
}



table {
  border-collapse: collapse;
  margin-top: 10px;
}

th,
td {
  padding: 8px;
  border-bottom: 3px solid #030363;
}

th {
  background-color: #f2f2f2;
}


button {
  border: none;
  color: #000000;
  cursor: pointer;
  border-radius: 10px
}

.copy {
  position: relative;
  top: 16px;
}

@media screen and (max-width: 770px) {
  .container-sm13 {
    flex-direction: column;
    text-align: center;
    background-image: url("https://www.gifservice.fr/img/gif-vignette-large/b8a477b748b3dfa8d0013b2a645d1c56/13423-logo-yamaha-motocicletas-transporte.gif");
    background-size: contain;
    background-position: center;
    background-color: #000000;
    background-repeat: no-repeat;
  }

  .sm13 {
    margin: 9px 0;
    width: 80%;
    background: linear-gradient(30deg, rgba(255, 255, 255, 0), rgb(255, 255, 255));
    color: #000000;
  }
}

@media screen and (max-width: 400px) {
  .search-box input {
    width: 100%;
    background: #c5c1c107;
  }

  .logo {
    position: relative;
    right: 70px;
  }

  .buscador {
    text-align: center;
  }
}

@media screen and (max-width: 320px) {
  .tarjeta {
    width: 63%;
  }
}
</style>
  