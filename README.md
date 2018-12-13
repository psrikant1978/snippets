# snippets

I have a use case where I need to download data as an excel file. I am using Angular 7 (UI) and Java Spring REST API service. The REST API will generate the Excel file and send it to Angular where I use FileSaver.js module to save the file to disk. 
I see the blob being created and the size printed on the console, but the popup to save the file to disk does not show.
This used to work when I had Angular 4. Any help is greatly appreciated. Also, if anybody can suggest an alternative node module to achieve the same result, I am all ears. 

Below is the code snippet in Angular
this.myService.getDataAsXLSX(this.dataList).subscribe(byteArray => {
 this.excelFileAsByteArray = byteArray;
  },
  error => console.log("There was an error."),
  () => {
 const blob = new Blob([this.excelFileAsByteArray.body], {type:'application/application/vnd.openxmlformats-officedocument.spreadsheetml.sheet; ; charset=UTF-8'});
 console.log('Blob size in filtered = ' + blob.size);
    FileSaver.saveAs(blob, "MyData-" + this.datePipe.transform(this.currentDate, 'MM-dd-yyyy') + ".xlsx");
  }
);
Here is the code from the REST API

ByteArrayOutputStream outputStream = new ExcelWriter().createExcelWorkbook(dataList, fullName);
byte[] byteArray = outputStream.toByteArray();

HttpHeaders responseHeaders = new HttpHeaders();
responseHeaders.set("Content-Type","application/vnd.openxmlformats-officedocument.spreadsheetml.sheet; ; charset=UTF-8");
responseHeaders.set("Content-Disposition", "attachment; filename=MyData.xlsx");
responseHeaders.set("Expires", "0");

return new ResponseEntity<Object>(byteArray, responseHeaders, HttpStatus.OK);
