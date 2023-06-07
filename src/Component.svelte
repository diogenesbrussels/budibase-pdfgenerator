<script>
  import { getContext } from "svelte";
  import { jsPDF } from "jspdf";
  import { toPng } from "html-to-image";

  export let buttonText;

  let PDFContent;

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  function createPDF() {
    const doc = new jsPDF();

    toPng(PDFContent).then((dataUrl) => {
      const img = new Image({ orientation: "p", unit: "mm", format: "a4" });
      img.src = dataUrl;

      img.onload = () => {
        doc.internal.scaleFactor = 1;

        const imgProps = doc.getImageProperties(img);
        const pdfWidth = doc.internal.pageSize.getWidth();
        const pdfHeight = doc.internal.pageSize.getHeight();
        const imgFullHeight = imgProps.height;
        const nPages = Math.ceil(imgFullHeight / 1402);

        const pageCanvas = document.createElement("canvas");
        const pageCtx = pageCanvas.getContext("2d");
        pageCanvas.width = imgProps.width;
        pageCanvas.height = imgFullHeight;

        for (let page = 0; page < nPages; page++) {
          // Display the page.
          const w = pageCanvas.width;
          const h = pageCanvas.height;
          pageCtx.fillStyle = "white";
          pageCtx.fillRect(0, 0, w, h);
          pageCtx.drawImage(img, 0, page * 1402, w, h, 0, 0, w, h);

          // Add the page to the PDF.
          if (page) doc.addPage();

          const imgData = pageCanvas.toDataURL(`image/PNG`, 1);
          doc.addImage(imgData, 'PNG', 0, 0, 210, 297);
        }

        //doc.addImage(img, "PNG", 0, 0, pdfWidth, pdfHeight);
        doc.save("file.pdf");
      };
    });
  }
  $: console.log(PDFContent);
</script>

<div use:styleable={$component.styles}>
  <button on:click={createPDF}>{buttonText}</button>
  <div bind:this={PDFContent} style="background-color:#FFF;width: 992px;">
    <slot />
  </div>
</div>

<style>
  * {
  } /* empty stylesheet0 solves the problem */
</style>
