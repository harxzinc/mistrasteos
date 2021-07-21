<template>
  <h1 class="text-center m-3">Cotiza con nosotros</h1>
  <div class="card m-3">
    <div class="card-header">
      <ul class="nav nav-tabs card-header-tabs">
        <li class="nav-item">
          <a :class="status.pasos.paso1">Paso (1)</a>
        </li>
        <li class="nav-item">
          <a :class="status.pasos.paso2">Paso (2)</a>
        </li>
        <li class="nav-item">
          <a :class="status.pasos.paso3">Paso (3)</a>
        </li>
      </ul>
    </div>

    <div v-if="status.paso === 'paso1'" class="card-body">
      <h5 class="card-title text-center">¿De donde a donde?</h5>
      <div class="input-group mb-3">
        <span class="input-group-text" id="direccionorigen">📍</span>
        <input
          list="datalistdireccionorigen"
          class="form-control"
          placeholder="Desde"
          aria-label="Desde"
          aria-describedby="direccionorigen"
          v-model="direccionorigen"
          @keyup="getAddress(true)"
        />
      </div>

      <div v-show="status.dirorigen">
        <div
          class="form-check"
          v-for="(direccion, key) in direccionesorigen"
          :key="key"
        >
          <input
            class="form-check-input"
            type="radio"
            name="do"
            :id="'do' + direccion.id"
            :value="direccion.id"
            @change="seleccionOrigen(direccion)"
          />
          <label class="form-check-label" :for="'do' + direccion.id">
            {{ direccion.name }}
          </label>
        </div>
      </div>

      <div class="input-group mb-3">
        <span class="input-group-text" id="direcciondestino">🚩</span>
        <input
          list="datalistdirecciondestino"
          class="form-control"
          placeholder="Hasta"
          aria-label="Hasta"
          aria-describedby="direcciondestino"
          v-model="direcciondestino"
          @keyup="getAddress(false)"
        />
      </div>

      <div v-show="status.dirdestino">
        <div
          class="form-check"
          v-for="(direccion, key) in direccionesdestino"
          :key="key"
        >
          <input
            class="form-check-input"
            type="radio"
            name="dd"
            :id="'dd' + direccion.id"
            :value="direccion.id"
            @change="seleccionDestino(direccion)"
          />
          <label class="form-check-label" :for="'dd' + direccion.id">
            {{ direccion.name }}
          </label>
        </div>
      </div>

      <button v-if="status.button" :class="btnCotizar" @click="cotizar">
        Cotizar
      </button>
      <button v-else class="btn btn-warning w-100" type="button" disabled>
        <span
          class="spinner-border spinner-border-sm"
          role="status"
          aria-hidden="true"
        ></span>
        Cotizando...
      </button>
    </div>

    <div v-if="status.paso === 'paso2'" class="card-body">
      <h5 class="card-title text-center">¡Tu cotización!</h5>
      <div class="input-group mb-3">
        <span class="input-group-text">Origen</span>
        <input
          list="datalistdireccionorigen"
          class="form-control"
          placeholder="Desde"
          aria-label="Desde"
          aria-describedby="direccionorigen"
          v-model="direccionorigen"
          disabled
        />
      </div>
      <div class="input-group mb-3">
        <span class="input-group-text" id="direcciondestino">Destino</span>
        <input
          list="datalistdirecciondestino"
          class="form-control"
          placeholder="Hasta"
          aria-label="Hasta"
          aria-describedby="direcciondestino"
          v-model="direcciondestino"
          disabled
        />
      </div>
      <div class="card text-dark bg-warning mb-3 w-100 text-center">
        <div class="card-header"><h2>Valor</h2></div>
        <div class="card-body">
          <h1 class="card-title">{{ status.cotizacion.precio }}</h1>
        </div>
      </div>
      <button class="btn btn-warning w-100" @click="continuar()">
        Continuar
      </button>
    </div>

    <div v-if="status.paso === 'paso3'" class="card-body">
      <h5 class="card-title text-center">Datos de contacto</h5>
      <div class="input-group mb-3">
        <span class="input-group-text">👤</span>
        <input
          class="form-control"
          placeholder="tu nombre"
          aria-label="tu nombre"
          v-model="datos.nombreSolicitante"
        />
      </div>
      <div class="input-group mb-3">
        <span class="input-group-text">📱 </span>
        <input
          class="form-control"
          placeholder="tu celular"
          aria-label="tu celular"
          v-model="datos.celularSolicitante"
        />
      </div>
      <div class="input-group mb-3">
        <span class="input-group-text">🗓 </span>
        <input
          class="form-control"
          type="date"
          aria-label="tu celular"
          v-model="datos.fechaTrasteo"
        />
      </div>
      <button :class="btnSolicitar" @click="solicitar">Solicitar</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import _ from "lodash";

export default {
  name: "Quote",
  props: {
    msg: String,
  },
  data() {
    return {
      requestCotizador: null,
      status: {
        dirorigen: true,
        dirdestino: true,
        button: true,
        paso: "paso1",
        pasos: {
          paso1: "nav-link active",
          paso2: "nav-link",
          paso3: "nav-link",
        },
        cotizacion: {
          precio: 0,
        },
      },
      datos: {
        nombreSolicitante: "",
        celularSolicitante: "",
        fechaTrasteo: "",
      },
      direccionorigenselect: {},
      direcciondestinoselect: {},
      direccionorigen: "",
      direcciondestino: "",
      direccionesorigen: [],
      direccionesdestino: [],
    };
  },
  methods: {
    getAddress: _.debounce(async function (flag) {
      let CancelToken = {}
      let source = {}

      if (this.requestCotizador) {
        this.requestCotizador.cancel("Operation canceled by the user.");
      } else {
        CancelToken = axios.CancelToken;
        source = CancelToken.source();
        this.requestCotizador = source;
      }

      const response = await axios.get(
        `${process.env.VUE_APP_REST}/api/getaddress?`,
        {
          params: {
            id: flag ? this.direccionorigen : this.direcciondestino,
          },
          cancelToken: source.token,
        }
      );

      if (flag) {
        this.direccionesorigen = response.data.data;
        this.status.dirorigen = true;
      } else {
        this.direccionesdestino = response.data.data;
        this.status.dirdestino = true;
      }
    }, 300),
    seleccionOrigen(selection) {
      this.direccionorigenselect = selection;
      this.direccionorigen = selection.name;
      this.status.dirorigen = false;
    },
    seleccionDestino(selection) {
      this.direcciondestinoselect = selection;
      this.direcciondestino = selection.name;
      this.status.dirdestino = false;
    },
    async cotizar() {
      const numberFormat = new Intl.NumberFormat("es-CO", {
        style: "currency",
        currency: "COP",
      });
      this.status.button = false;

      const response = await axios.get(
        `${process.env.VUE_APP_REST}/api/getquote?`,
        {
          params: {
            origin: this.direccionorigenselect.id,
            destination: this.direcciondestinoselect.id,
          },
        }
      );

      if (response.data.status) {
        this.status.button = true;
        this.status.paso = "paso2";
        this.status.pasos.paso1 = "nav-link";
        this.status.pasos.paso2 = "nav-link active";
        this.status.cotizacion.precio = numberFormat.format(
          response.data.data.price
        );
      }
    },
    continuar() {
      this.status.paso = "paso3";
      this.status.pasos.paso1 = "nav-link";
      this.status.pasos.paso2 = "nav-link";
      this.status.pasos.paso3 = "nav-link active";
    },
    async solicitar() {
      let celular = process.env.VUE_APP_CELULAR;
      let texto = `*Desde*: ${this.direccionorigenselect.name}%0A*Hasta*: ${this.direcciondestinoselect.name}%0A*Precio*: ${this.status.cotizacion.precio}%0A%0A*Nombre*: ${this.datos.nombreSolicitante}%0A*Celular*: ${this.datos.celularSolicitante}%0A*Fecha*: ${this.datos.fechaTrasteo}`;
      window.location.href = `https://api.whatsapp.com/send?phone=+57${celular}&text=${texto}`;
    },
  },
  computed: {
    btnCotizar: function () {
      if (this.direccionorigenselect.name && this.direcciondestinoselect.name) {
        return "btn btn-warning w-100";
      } else {
        return "btn btn-warning w-100 disabled";
      }
    },
    btnSolicitar: function () {
      if (
        this.datos.nombreSolicitante &&
        this.datos.celularSolicitante &&
        this.datos.fechaTrasteo
      ) {
        return "btn btn-warning w-100";
      } else {
        return "btn btn-warning w-100 disabled";
      }
    },
  },
};
</script>