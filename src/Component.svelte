<script>
  import { getContext } from "svelte";
  import FormData from "form-data";
  import { extractStyle } from "style-extractor";

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  export let buttonText, filename;
  let PDFContent;

  async function createPDF() {
    const form = new FormData();
    let htmlContent = PDFContent.innerHTML;
    const { html, css } = extractStyle(htmlContent);
    htmlContent = "<style>" + css + "</style>" + html;
    const blob = new Blob([htmlContent], { type: "text/html" });
    
    form.append("files", blob, "index.html");
    form.append("paperWidth", "8.27");
    form.append("paperHeight", "11.7");
    form.append("marginTop", "0");
    form.append("marginBottom", "0");
    form.append("marginLeft", "0");
    form.append("marginRight", "0");
    form.append("preferCssPageSize", "false");
    form.append("printBackground", "true");
    form.append("landscape", "false");
    form.append("scale", "1.0");

    try {
      const response = await fetch("/pdf/forms/chromium/convert/html", {
        method: "POST",
        body: form,
      });

      if (response.ok) {
        const pdfBlob = await response.blob();
        const url = URL.createObjectURL(pdfBlob);
        const link = document.createElement("a");
        link.href = url;
        link.download = filename ?? "generated" + ".pdf";
        link.click();
        URL.revokeObjectURL(url);
      } else {
        // Handle the error response here
        console.log("Fetch error", response);
      }
    } catch (error) { 
      // Handle fetch or other error here
      console.log("Error while generating", error);
    }
  }
</script>

<div use:styleable={$component.styles}>
  <button on:click={createPDF}>{buttonText ?? "Generate PDF"}</button>
  <div bind:this={PDFContent} style="background-color:#FFF;width:210mm;">
    <slot />
  </div>
</div>
