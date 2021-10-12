<template>
  <div id="app">
    <b-container fluid class="p-4 bg-dark">
      <b-row>
        <b-col>
          <cropper
            ref="cropper"
            :src="img.src"
            :debounce="false"
            @change="onChange"
            :stencil-props="{
              aspectRatio: aspectRatio.value,
            }"
          />
          <results :coordinates="coordinates" :image="image" />
        </b-col>
      </b-row>
    </b-container>
    <div
      class="btn-group btn-group-toggle m-2"
      v-for="item in aspectRatios"
      :key="item.name"
    >
      <button
        type="button"
        :value="item.ratio"
        v-on:click="aspect_ratio"
        class="btn btn-primary"
      >
        {{ item.title }}
      </button>
    </div>
    <button type="button" v-on:click="cropped" class="btn btn-primary">
      Cropped
    </button>
  </div>
</template>


<script>
import { Cropper } from "vue-advanced-cropper";
import results from "./results";

function getMimeType(file, fallback = null) {
  const byteArray = new Uint8Array(file).subarray(0, 4);
  let header = "";
  for (let i = 0; i < byteArray.length; i++) {
    header += byteArray[i].toString(16);
  }
  switch (header) {
    case "89504e47":
      return "image/png";
    case "47494638":
      return "image/gif";
    case "ffd8ffe0":
    case "ffd8ffe1":
    case "ffd8ffe2":
    case "ffd8ffe3":
    case "ffd8ffe8":
      return "image/jpeg";
    default:
      return fallback;
  }
}

function get_form_field() {
  return {
    x: document.getElementById("id_focal_point_x"),
    y: document.getElementById("id_focal_point_y"),
    w: document.getElementById("id_focal_point_width"),
    h: document.getElementById("id_focal_point_height"),
  };
}

export default {
  components: {
    Cropper,
    results,
  },
  data() {
    return {
      focus_point: get_form_field(),
      img: {
        src:
          "https://images.unsplash.com/photo-1586083718719-019f9dc6ca94?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1024&q=80",
      },
      coordinates: {
        width: 0,
        height: 0,
        left: 0,
        top: 0,
      },
      aspectRatio: { value: 1, data: "1:1" },
      aspectRatios: [
        { title: "1:1", ratio: 1 },
        { title: "1:2", ratio: 1 / 2 },
        { title: "2:1", ratio: 2 / 1 },
        { title: "3:4", ratio: 3 / 4 },
      ],

      image: null,
    };
  },
  mounted() {
    var el = document.getElementById("id_file");
    el.addEventListener("change", this.loadImage);

    console.log("Mounted");
  },

  methods: {
    onChange({ coordinates, canvas }) {
      this.coordinates = coordinates;
    },
    aspect_ratio(el) {
      this.aspectRatio.value = el.target.value;
    },
    cropped() {
      this.focus_point.x.value = this.coordinates.left;
      this.focus_point.y.value = this.coordinates.top;
      this.focus_point.w.value = this.coordinates.width;
      this.focus_point.h.value = this.coordinates.height;
    },
    loadImage(event) {
      // Reference to the DOM input element
      const { files } = event.target;
      // Ensure that you have a file before attempting to read it
      if (files && files[0]) {
        // 1. Revoke the object URL, to allow the garbage collector to destroy the uploaded before file
        if (this.img.src) {
          URL.revokeObjectURL(this.img.src);
        }
        // 2. Create the blob link to the file to optimize performance:
        const blob = URL.createObjectURL(files[0]);

        // Create a new FileReader to read this image binary data
        const reader = new FileReader();
        // Define a callback function to run, when FileReader finishes its job
        reader.onload = (e) => {
          // Note: arrow function used here, so that "this.image" refers to the image of Vue component
          this.img = {
            // Set the image source (it will look like blob:http://example.com/2c5270a5-18b5-406e-a4fb-07427f5e7b94)
            src: blob,
            // Determine the image type to preserve it during the extracting the image from canvas:
            type: getMimeType(e.target.result, files[0].type),
          };
        };
        // Start the reader job - read file as a data url (base64 format)
        reader.readAsArrayBuffer(files[0]);
      }
    },
  },
};
</script>

