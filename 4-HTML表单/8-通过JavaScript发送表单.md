HTML表单可以声明式发送[HTTP](1/en-US/docs/HTTP)请求。但是表单也可以准备HTTP请求以通过JavaScript发送，例如通过`XMLHttpRequest`。本文探讨了这种方法。

## 形式并不总是形式

对于渐进式Web应用程序，单页应用程序和基于框架的应用程序，通常在接收到响应数据时使用[HTML表单](1/en-US/docs/HTML/Forms)发送数据而不加载新文档。让我们首先讨论为什么这需要一种不同的方法。

### 获得对全局接口的控制



如上一篇文章中所述，标准HTML表单提交将加载发送数据的URL，这意味着浏览器窗口将以整页加载进行导航。避免整个页面加载可以通过避免网络延迟和可能的视觉问题（如闪烁）来提供更流畅的体验。

许多现代UI仅使用HTML表单来收集用户输入，而不使用数据提交。当用户尝试发送数据时，应用程序将控制并在后台异步传输数据，仅更新UI中需要更改的部分。

异步发送任意数据通常称为[AJAX](1/en-US/docs/AJAX)，它代表**“异步JavaScript和XML”**。

### 有什么不同？



[Ť](1/en-US/docs/AJAX)他[`XMLHttpRequest`](1/en-US/docs/Web/API/XMLHttpRequest)（XHR）DOM对象可以建立HTTP请求，向他们发送和检索其结果。从历史上看，它[`XMLHttpRequest`](1/en-US/docs/Web/API/XMLHttpRequest)被设计为以交换格式获取和发送[XML](1/en-US/docs/XML)，此后已被[JSON](1/en-US/docs/JSON)取代。但是XML和JSON都不适合表单数据请求编码。表单数据（`application/x-www-form-urlencoded`）由URL编码的键/值对列表组成。为了传输二进制数据，HTTP请求被重塑为`multipart/form-data`。

**注意**：[如今，Fetch API](1/en-US/docs/Web/API/Fetch_API)经常代替XHR使用-它是XHR的现代更新版本，其工作方式类似，但具有一些优点。您将在本文中看到的大多数XHR代码都可以换成Fetch。

如果您控制前端（在浏览器中执行的代码）和后端（在服务器上执行的代码），则可以发送JSON / XML并根据需要进行处理。

但是，如果要使用第三方服务，则需要以服务所需的格式发送数据。

那么我们应该如何发送此类数据呢？您需要使用的不同技术如下。

## 发送表格数据

从传统技术到较新的[`FormData`](1/en-US/docs/Web/API/XMLHttpRequest/FormData)对象，共有3种发送表单数据的方法。让我们详细看看它们。

### 手动构建XMLHttpRequest



[`XMLHttpRequest`](1/en-US/docs/Web/API/XMLHttpRequest)是发出HTTP请求的最安全，最可靠的方法。要使用发送表单数据[`XMLHttpRequest`](1/en-US/docs/Web/API/XMLHttpRequest)，请通过对URL进行编码来准备数据，并遵守表单数据请求的细节。

让我们看一个例子：

```html
<button>Click Me!</button>
```

现在是JavaScript：

```js
const btn = document.querySelector('button');

function sendData( data ) {
  console.log( 'Sending data' );

  const XHR = new XMLHttpRequest();

  let urlEncodedData = "",
      urlEncodedDataPairs = [],
      name;

  // Turn the data object into an array of URL-encoded key/value pairs.
  for( name in data ) {
    urlEncodedDataPairs.push( encodeURIComponent( name ) + '=' + encodeURIComponent( data[name] ) );
  }

  // Combine the pairs into a single string and replace all %-encoded spaces to 
  // the '+' character; matches the behaviour of browser form submissions.
  urlEncodedData = urlEncodedDataPairs.join( '&' ).replace( /%20/g, '+' );

  // Define what happens on successful data submission
  XHR.addEventListener( 'load', function(event) {
    alert( 'Yeah! Data sent and response loaded.' );
  } );

  // Define what happens in case of error
  XHR.addEventListener( 'error', function(event) {
    alert( 'Oops! Something went wrong.' );
  } );

  // Set up our request
  XHR.open( 'POST', 'https://example.com/cors.php' );

  // Add the required HTTP header for form data POST requests
  XHR.setRequestHeader( 'Content-Type', 'application/x-www-form-urlencoded' );

  // Finally, send our data.
  XHR.send( urlEncodedData );
}

btn.addEventListener( 'click', function() {
  sendData( {test:'ok'} );
} )
```



**注意：**如果您想将数据发送到第三方网站，[`XMLHttpRequest`](1/en-US/docs/Web/API/XMLHttpRequest)则此使用受[同源策略的](1/en-US/docs/Glossary/same-origin_policy)约束。对于跨域请求，您将需要[CORS和HTTP访问控制](1/en-US/docs/HTTP/Access_control_CORS)。

### 使用XMLHttpRequest和FormData对象



手动建立HTTP请求可能会很麻烦。幸运的是，[XMLHttpRequest规范](http://www.w3.org/TR/XMLHttpRequest/)提供了一种更简单的方法来处理带有该[`FormData`](1/en-US/docs/Web/API/XMLHttpRequest/FormData)对象的表单数据请求。

该[`FormData`](1/en-US/docs/Web/API/XMLHttpRequest/FormData)对象可用于构建用于传输的表单数据，或用于获取表单元素中的数据以管理其发送方式。请注意，[`FormData`](1/en-US/docs/Web/API/XMLHttpRequest/FormData)对象是“仅写”的，这意味着您可以更改它们，但不能检索其内容。

“使用[FormData对象”中](1/en-US/docs/DOM/XMLHttpRequest/FormData/Using_FormData_Objects)详细介绍了使用此对象的方法，但这是两个示例：

#### 使用独立的FormData对象

```html
<button>Click Me!</button>
```

您应该熟悉该HTML示例。现在使用JavaScript：

```js
const btn = document.querySelector('button');

function sendData( data ) {
  const XHR = new XMLHttpRequest(),
        FD  = new FormData();

  // Push our data into our FormData object
  for( name in data ) {
    FD.append( name, data[ name ] );
  }

  // Define what happens on successful data submission
  XHR.addEventListener( 'load', function( event ) {
    alert( 'Yeah! Data sent and response loaded.' );
  } );

  // Define what happens in case of error
  XHR.addEventListener(' error', function( event ) {
    alert( 'Oops! Something went wrong.' );
  } );

  // Set up our request
  XHR.open( 'POST', 'https://example.com/cors.php' );

  // Send our FormData object; HTTP headers are set automatically
  XHR.send( FD );
}

btn.addEventListener( 'click', function() 
  { sendData( {test:'ok'} ); 
} )
```



#### 使用绑定到表单元素的FormData

您还可以将`FormData`对象绑定到`<form>`元素。这将创建一个`FormData`对象，该对象代表表单中包含的数据。

HTML是典型的：

```html
<form id="myForm">
  <label for="myName">Send me your name:</label>
  <input id="myName" name="name" value="John">
  <input type="submit" value="Send Me!">
</form>
```

但是JavaScript会采用以下形式：

```js
window.addEventListener( "load", function () {
  function sendData() {
    const XHR = new XMLHttpRequest();

    // Bind the FormData object and the form element
    const FD = new FormData( form );

    // Define what happens on successful data submission
    XHR.addEventListener( "load", function(event) {
      alert( event.target.responseText );
    } );

    // Define what happens in case of error
    XHR.addEventListener( "error", function( event ) {
      alert( 'Oops! Something went wrong.' );
    } );

    // Set up our request
    XHR.open( "POST", "https://example.com/cors.php" );

    // The data sent is what the user provided in the form
    XHR.send( FD );
  }
 
  // Access the form element...
  let form = document.getElementById( "myForm" );

  // ...and take over its submit event.
  form.addEventListener( "submit", function ( event ) {
    event.preventDefault();

    sendData();
  } );
} );

```

您甚至可以通过使用表单的[`elements`](1/en-US/docs/Web/API/HTMLFormElement/elements)属性来获取表单中所有数据元素的列表，然后一次手动管理它们，从而使流程更多地参与其中。

## 处理二进制数据

如果将[`FormData`](1/en-US/docs/Web/API/XMLHttpRequest/FormData)对象与包含`<input type="file">`小部件的表单一起使用，则数据将被自动处理。但是要手动发送二进制数据，还有很多工作要做。

有二进制数据，其中包括许多来源[`FileReader`](1/en-US/docs/Web/API/FileReader)，[`Canvas`](1/en-US/docs/Web/API/HTMLCanvasElement)和[的WebRTC](1/en-US/docs/WebRTC/navigator.getUserMedia)。不幸的是，某些旧版浏览器无法访问二进制数据或需要复杂的解决方法.

发送二进制数据[`FormData`](1/en-US/docs/Web/API/XMLHttpRequest/FormData)最简单的`append()`方法是使用上面演示的方法。如果必须手动操作，则比较棘手。

在以下示例中，我们使用[`FileReader`](1/en-US/docs/Web/API/FileReader)API访问二进制数据，然后手动构建多部分表单数据请求：

```html
<form id="theForm">
  <p>
    <label for="theText">text data:</label>
    <input id="theText" name="myText" value="Some text data" type="text">
  </p>
  <p>
    <label for="theFile">file data:</label>
    <input id="theFile" name="myFile" type="file">
  </p>
  <button>Send Me!</button>
</form>
```

如您所见，HTML是standard 。没有神奇的事情发生。“魔术”在JavaScript中：

```js
// Because we want to access DOM nodes,
// we initialize our script at page load.
window.addEventListener( 'load', function () {

  // These variables are used to store the form data
  const text = document.getElementById( "theText" );
  const file = {
        dom    : document.getElementById( "theFile" ),
        binary : null
      };
 
  // Use the FileReader API to access file content
  const reader = new FileReader();

  // Because FileReader is asynchronous, store its
  // result when it finishes to read the file
  reader.addEventListener( "load", function () {
    file.binary = reader.result;
  } );

  // At page load, if a file is already selected, read it.
  if( file.dom.files[0] ) {
    reader.readAsBinaryString( file.dom.files[0] );
  }

  // If not, read the file once the user selects it.
  file.dom.addEventListener( "change", function () {
    if( reader.readyState === FileReader.LOADING ) {
      reader.abort();
    }
    
    reader.readAsBinaryString( file.dom.files[0] );
  } );

  // sendData is our main function
  function sendData() {
    // If there is a selected file, wait it is read
    // If there is not, delay the execution of the function
    if( !file.binary && file.dom.files.length > 0 ) {
      setTimeout( sendData, 10 );
      return;
    }

    // To construct our multipart form data request,
    // We need an XMLHttpRequest instance
    const XHR = new XMLHttpRequest();

    // We need a separator to define each part of the request
    const boundary = "blob";

    // Store our body request in a string.
    let data = "";

    // So, if the user has selected a file
    if ( file.dom.files[0] ) {
      // Start a new part in our body's request
      data += "--" + boundary + "\r\n";

      // Describe it as form data
      data += 'content-disposition: form-data; '
      // Define the name of the form data
            + 'name="'         + file.dom.name          + '"; '
      // Provide the real name of the file
            + 'filename="'     + file.dom.files[0].name + '"\r\n';
      // And the MIME type of the file
      data += 'Content-Type: ' + file.dom.files[0].type + '\r\n';

      // There's a blank line between the metadata and the data
      data += '\r\n';
      
      // Append the binary data to our body's request
      data += file.binary + '\r\n';
    }

    // Text data is simpler
    // Start a new part in our body's request
    data += "--" + boundary + "\r\n";

    // Say it's form data, and name it
    data += 'content-disposition: form-data; name="' + text.name + '"\r\n';
    // There's a blank line between the metadata and the data
    data += '\r\n';

    // Append the text data to our body's request
    data += text.value + "\r\n";

    // Once we are done, "close" the body's request
    data += "--" + boundary + "--";

    // Define what happens on successful data submission
    XHR.addEventListener( 'load', function( event ) {
      alert( 'Yeah! Data sent and response loaded.' );
    } );

    // Define what happens in case of error
    XHR.addEventListener( 'error', function( event ) {
      alert( 'Oops! Something went wrong.' );
    } );

    // Set up our request
    XHR.open( 'POST', 'https://example.com/cors.php' );

    // Add the required HTTP header to handle a multipart form data POST request
    XHR.setRequestHeader( 'Content-Type','multipart/form-data; boundary=' + boundary );

    // And finally, send our data.
    XHR.send( data );
  }

  // Access our form...
  const form = document.getElementById( "theForm" );

  // ...to take over the submit event
  form.addEventListener( 'submit', function ( event ) {
    event.preventDefault();
    sendData();
  } );
} );
```