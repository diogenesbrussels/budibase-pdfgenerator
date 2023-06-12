<script>
  import { getContext } from "svelte";
  import { jsPDF } from "jspdf";
  import { toPng } from "html-to-image";
  import html2PDF from 'jspdf-html2canvas';

  export let buttonText;

  let PDFContent;

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  async function createPDF() {

    html2PDF(PDFContent, {
    jsPDF: {
      format: 'a4',
      useCORS: true,
    },
    imageType: 'image/jpeg',
    output: './pdf/generate.pdf'
  });
    // const doc = new jsPDF({ orientation: "p", unit: "mm", format: "a4" });

    // domtoimage
    //   .toPng(PDFContent)
    //   .then(function (dataUrl) {
    //     var img = new Image();
    //     img.src = dataUrl;
    //     img.onload = () => {
    //       const imgProps = doc.getImageProperties(img);
    //       doc.addImage(img, "PNG", 0, 0, doc.internal.pageSize.getWidth(), doc.internal.pageSize.getHeight());
    //       doc.save("file.pdf");
    //     };

    //     document.body.appendChild(img);
    //   })
    //   .catch(function (error) {
    //     console.error("oops, somehhhthing went wrong!", error);
    //   });

    // toPng(PDFContent)
    //   .then((dataUrl) => {
    //     const img = new Image();
    //     img.src = dataUrl;

    //     img.onload = () => {
    //       const imgProps = doc.getImageProperties(img);
    //       const pdfWidth = doc.internal.pageSize.getWidth();
    //       const pdfHeight = doc.internal.pageSize.getHeight();
    //       const nPages = Math.ceil(imgProps.height / pdfHeight);

    //       const pageCanvas = document.createElement("canvas");
    //       const pageCtx = pageCanvas.getContext("2d");
    //       pageCanvas.width = pdfWidth;
    //       pageCanvas.height = pdfHeight;

    //       for (let page = 0; page < nPages; page++) {
    //         // Display the page.
    //         const w = pageCanvas.width;
    //         const h = pageCanvas.height;
    //         pageCtx.fillStyle = "white";
    //         pageCtx.fillRect(0, 0, w, h);
    //         pageCtx.drawImage(img, 0, page * pdfHeight, w, h, 0, 0, w, h);

    //         // Add the page to the PDF.
    //         if (page) doc.addPage();

    //         const imgData = pageCanvas.toDataURL(`image/PNG`, 1);
    //         doc.addImage(imgData, "PNG", 0, 0, pdfWidth, pdfHeight);
    //       }

    //       //doc.addImage(img, "PNG", 0, 0, pdfWidth, pdfHeight);
    //       doc.save("file.pdf");
    //     };
    //   })
    //   .catch(function (error) {
    //     console.error("oops, something went wrong!", error);
    //   });
  }
</script>

<div use:styleable={$component.styles}>
  <button on:click={createPDF}>{buttonText}</button>
  <div bind:this={PDFContent} style="background-color:#FFF;width: 210mm;">
    <slot />
  </div>
</div>

<style>
  * {
  } /* empty stylesheet0 solves the problem */
</style>
