<template>
  <div class="results">
    <div class="results__wrapper" v-if="image">
      <div class="results__coordinates">
        <p class="results__coordinates-title">Results:</p>
        <p class="results__coordinates-value">Width: {{ coordinates.width }}</p>
        <p class="results__coordinates-value">
          Height: {{ coordinates.height }}
        </p>
        <p class="results__coordinates-value">Left: {{ coordinates.left }}</p>
        <p class="results__coordinates-value">Top: {{ coordinates.top }}</p>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
// Extra small screen / phone
$screen-xs: 480px !default;
$screen-xs-min: $screen-xs !default;

// Small screen / tablet
$screen-sm: 721px !default;
$screen-sm-min: $screen-sm !default;

// Medium screen / desktop
$screen-md: 944px !default;
$screen-md-min: $screen-md !default;

// Large screen / wide desktop
$screen-lg: 1200px !default;
$screen-lg-min: $screen-lg !default;

$vue-color: #3fb37f;
$gray-color: #585858;
$transition-time: 0.5s;

$header-height: 75px;
$footer-height: 150px;
$footer-margin: 50px;

$boundaries-color: #5B74BC;
$cropper-color: #3fb37f;
$area-color: #bcbcbc;
$stretcher-color: #4966c4;

@mixin container {
  width: 100%;
  max-width: $screen-md;
  margin: 0 auto;
  padding: 0 22px;
}

.results {
  &__wrapper {
    display: flex;
    position: absolute;
    right: 15px;
    bottom: 15px;
    border: dashed 1px rgba(white, 0.5);
  }
  &__coordinates {
    background: rgba(black, 0.8);
    padding: 5px;
    font-size: 10px;
    color: white;
    pointer-events: none;
    @media (max-width: $screen-sm) {
      display: none;
    }
  }
  &__coordinates-title {
    font-weight: bold;
  }
  &__coordinates-title,
  &__coordinates-value {
    margin-top: 2px;
    margin-bottom: 2px;
  }
}
</style>

<script>
export default {
  props: {
    coordinates: {
      type: Object,
      default() {
        return {};
      },
    },
    image: {
      type: String,
    },
  },
};
</script>
