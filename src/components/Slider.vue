<template>
  <div
    id="carouselExampleCaptions"
    class="carousel slide"
    data-bs-ride="carousel"
  >
    <div class="carousel-indicators">
      <button
        type="button"
        data-bs-target="#carouselExampleCaptions"
        data-bs-slide-to="0"
        class="active"
        aria-current="true"
        aria-label="Slide 1"
      ></button>
      <button
        type="button"
        data-bs-target="#carouselExampleCaptions"
        data-bs-slide-to="1"
        aria-label="Slide 2"
      ></button>
      <button
        type="button"
        data-bs-target="#carouselExampleCaptions"
        data-bs-slide-to="2"
        aria-label="Slide 3"
      ></button>
    </div>
    <div class="carousel-inner">
      <div class="carousel-item active">
        <img
          src="@/assets/carousel/camion1.jpeg"
          class="d-block w-100 img-carousel"
          alt="..."
        />
        <div class="carousel-caption d-none d-md-block">
          <h5>Excelencia</h5>
          <p>Brindamos una tranquilidad a los usuarios a la hora de transportar.</p>
        </div>
      </div>
      <div class="carousel-item">
        <img
          src="@/assets/carousel/camion2.jpeg"
          class="d-block w-100 img-carousel"
          alt="..."
        />
        <div class="carousel-caption d-none d-md-block">
          <h5>Puntualidad</h5>
          <p>La puntualidad define el sello de nuestro servicio.</p>
        </div>
      </div>
      <div class="carousel-item">
        <img
          src="@/assets/carousel/camion3.jpeg"
          class="d-block w-100 img-carousel"
          alt="..."
        />
        <div class="carousel-caption d-none d-md-block">
          <h5>Cobertura</h5>
          <p>Tenemos cobertura en gran parte de Antioquia.</p>
        </div>
      </div>
    </div>
    <button
      class="carousel-control-prev"
      type="button"
      data-bs-target="#carouselExampleCaptions"
      data-bs-slide="prev"
    >
      <span class="carousel-control-prev-icon" aria-hidden="true"></span>
      <span class="visually-hidden">Previous</span>
    </button>
    <button
      class="carousel-control-next"
      type="button"
      data-bs-target="#carouselExampleCaptions"
      data-bs-slide="next"
    >
      <span class="carousel-control-next-icon" aria-hidden="true"></span>
      <span class="visually-hidden">Next</span>
    </button>
  </div>
</template>
<style scoped>

</style>


<script>
export default {
  name: "Slider",
  props: {
    msg: String,
  },
};
</script>