<script>
  import pdfjs from "pdfjs-dist";
  import pdfjsWorker from "pdfjs-dist/build/pdf.worker.entry";

  export let pageNum = 1;
  export let scale = 1.5;
  export let documentProxy = null;
  export let rotation = 0;

  pdfjs.GlobalWorkerOptions.workerSrc = pdfjsWorker;

  let canvas;
  let pdfDoc = null;

  documentProxy.promise.then(function(pdfDoc) {
    pdfDoc.getPage(pageNum).then(function(page) {
      let viewport = page.getViewport({ scale: scale, rotation: rotation });
      const canvasContext = canvas.getContext("2d");
      canvas.height = viewport.height;
      canvas.width = viewport.width;

      page.render({ canvasContext, viewport });
    });
  });
</script>

<style>
  .paper {
    margin: 10px;
    border: 1px solid #bfbfbf;
    box-shadow: 3px 3px 2px #aaaaaa;
  }
</style>

<canvas class="paper" bind:this={canvas} />
