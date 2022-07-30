<script lang="ts">
  import { onMount } from "svelte";

  function getImageDimensions(image): Promise<{ width: number; height: number }> {
    return new Promise((resolve, reject) => {
      image.onload = function (e) {
        const width = this.width;
        const height = this.height;
        resolve({ height, width });
      };
    });
  }

  let imageInput = document.getElementById("image-input");

  onMount(() => {
    imageInput = document.getElementById("image-input");
    imageInput.addEventListener("change", async (ev) => {
      const uploadedImage = (imageInput as HTMLInputElement).files[0];
      if (!uploadedImage) {
        // if no file is uploaded, no need to do anything
        return;
      }

      // preview the inputted image
      const inputPreview = document.getElementById("input-preview");
      (inputPreview as HTMLImageElement).src = URL.createObjectURL(uploadedImage);

      //get the dimensions of the input image
      const { height, width } = await getImageDimensions(inputPreview);

      const MAX_WIDTH = 768; //if we resize by width, this is the max width of compressed image
      const MAX_HEIGHT = 768; //if we resize by height, this is the max height of the compressed image

      const widthRatioBlob = await compressImage(inputPreview, MAX_WIDTH / width, width, height);
      const heightRatioBlob = await compressImage(inputPreview, MAX_HEIGHT / height, width, height);

      //pick the smaller blob between both
      const compressedBlob = widthRatioBlob.size > heightRatioBlob.size ? heightRatioBlob : widthRatioBlob;

      // preview the compressed blob
      const outputPreview = document.getElementById("output-preview");
      (outputPreview as HTMLImageElement).src = URL.createObjectURL(compressedBlob);

      /*in some cases, the initial uploaded image maybe smaller than our compressed result. 
      if that is the case, reuse the uploaded image. */

      const optimalBlob = compressedBlob.size < uploadedImage.size ? compressedBlob : uploadedImage;
      console.log(`Inital Size: ${uploadedImage.size}. Compressed Size: ${optimalBlob.size}`);

      //   URL.revokeObjectURL(inputPreview);
      //   URL.revokeObjectURL(outputPreview);
    });
  });

  //   taken from: https://dev.to/ramko9999/client-side-image-compression-on-the-web-26j7

  function compressImage(image, scale, initalWidth, initalHeight): Promise<Blob> {
    return new Promise((resolve, reject) => {
      const canvas = document.createElement("canvas");

      canvas.width = scale * initalWidth;
      canvas.height = scale * initalHeight;

      const ctx = canvas.getContext("2d");
      ctx.drawImage(image, 0, 0, canvas.width, canvas.height);

      ctx.canvas.toBlob((blob) => {
        resolve(blob);
      }, "image/png");
    });
  }
</script>

<div>
  <h2>Crop image</h2>
  <div>
    <input id="image-input" type="file" accept=".jpg,.png,.jpeg,.jfif" />    
    <div>
      <img id="input-preview" alt="This is the preview of the img you are going to upload." />
    </div>
    <div>
      <img id="output-preview" alt="This is the compressed result of the img you will upload." />
    </div>
  </div>
</div>

<style>
  #input-preview {
    max-width: 50em;
  }

  #output-preview {
    max-width: 50em;
  }
</style>
