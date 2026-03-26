/**

* Serves the HTML file as a Web App.

*/

function doGet(e) {

  return HtmlService.createHtmlOutputFromFile('index')

      .setTitle('Smart Version Analyzer')

      .addMetaTag('viewport', 'width=device-width, initial-scale=1.0')

      .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);

}

 

/**

* Optional: Include a function if you ever want to split CSS/JS into separate files

* and include them directly into index.html using <?!= include('filename'); ?>

*/

function include(filename) {

  return HtmlService.createHtmlOutputFromFile(filename).getContent();

}
