# Design Resources
* [SVG Icons - https://thenounproject.com/](https://thenounproject.com/)
* [SVG Workflow - http://www.grunticon.com/](http://www.grunticon.com/)
* [SVG - Covert SVG to chunks of code](http://www.grumpicon.com/)
* [SVG - Downloads](http://www.vecteezy.com)
* http://creativedroplets.com/export-svg-for-the-web-with-illustrator-cc
* https://css-tricks.com/flat-icons-icon-fonts/

# HTML5
* [http://html5hub.com/](http://html5hub.com)

# Tools
* [Run Marked2 from the Command Line](http://jblevins.org/log/marked-2-command)

# Dev Tool Tips

### In code
* Insert 'debugger;' into your code.

### Elements tab
* Right-click on element, then "Scroll into View"
* Select element, and toggle with 'H'
* HTML - DOM Break Point
  * removes node
  * changes node
  * or anything under the node.
* Shift-click color to change color format.

### Network tab

* Disable cache - so that cache cannot be saved
* Shift mouseover. Red are the files that are called.  Green are the files that it was called by.
* Screenshots (camera icon) shows what the user sees.
* Deeper review:
  * Queuing:
    *  Lower priority than critical resources (e.g., images vs js/css)
    *  Unavailable TCP socks - about to free up
    *  HTTP1 allows only 6 connections per origin
    *  Time spent on disk cache entries (typically very fast)
  * Stalled/Blocking
    * Similiar to Queuing
    * Usually the proxy server for negotiation
  * Proxy negotiation
    * Timestamp of how long proxy negotiations took
  * DNS Lookup
    * Every new domain requires a full round trip to do the DNS lookup
  * Initial Connection / Connecting
    * Time it took to establish a connection, including TCP handshake/retries and negotiating a SSL.
  * Request Sent / Sending
    * Time spent issuing the network request. Typically a fraction of a millisecond.
  * Waiting (TTFB)
    * Time spent waiting on initial response, also known as Time To First Byte.  Captures roundtrip and spend time waiting.
  * Content Download / Downloading
    * Time spent receiving the response data.
* Try Network Throttling (3G) to get user experience   

### Sources tab
* 'Workspace' sources tab.  Just drag a folder into 'Sources'.
  * Run app as localhost, then create workspace and map files to save. 
* In the 'call stack', can right-click on any script that are 3rd party, and can 'Blackbox Script'.  That script will not be part of debugging, and won't step into it.

### Performance
* github.com/wilsonpage/fastdom
  * Window.requestAnimationFrame() - saves writing of DOM in batch.
* Check out paint flashing => Hit 'esc' for the console drawer and click on 'Rendering'
* And FPS meter
* While looking at pain flashing, just guess and deleting elements might help.
* CSS: 'will-change: transform' -> Will send to GPU.  Works best for 'fixed position' elements.

### Profiles
* Heap Snapshot
  * Take 2 snapshots then do a comparison.  This will be clear if there are any memory leaks.

### Resources
* @chromedevtools
* @firefoxdevtools
* @edgedevtools
* @paul_irish
* @addyosmani
* @jkup
* http://jankfree.org
* https://developers.google.com/web/tools/chrome-devtools/

