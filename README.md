# Budibase-pdfgenerator
This is a proof of concept for an initial integration of Gotenberg (Docker-powered stateless API for PDF files) into Budibase. This is not functional or ready for production use.
The html generated does not yet include the css style to create the PDF correctly.
Any ideas or comments to help me make progress are welcome.

# Description
Generate a PDF file from HTML content.

Find out more about [Budibase](https://github.com/Budibase/budibase).

## Instructions

You need to setup a Gotenberg container.
In your docker compose services add:
```
gotenberg:
    image: gotenberg/gotenberg:7
    environment:
      DEFAULT_LISTEN_PORT: '3000'
    ports:
      - 3000:3000
```

And add a proxy to your container in nginx:
```
location /pdf/ {
        proxy_pass      http://gotenberg:3000/;
    }
```

To build your new  plugin run the following in your Budibase CLI:
```
budi plugins --build
```

You can also re-build everytime you make a change to your plugin with the command:
```
budi plugins --watch
```

Please note that this integration is still in the proof of concept stage and should not be used in a production environment.