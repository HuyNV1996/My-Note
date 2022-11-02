## Điều chỉnh generate file css từ file scss
Go to VScode menu- file>preferences>settings>extension>live sass copile config>edit in settings.json

and paste it code >>

  "liveSassCompile.settings.formats": [
    {
      "format": "expanded",
      "extensionName": ".css",
      "savePath": "~/../css/"
    }
  ]