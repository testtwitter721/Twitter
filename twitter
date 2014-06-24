bplist00�_WebMainResource�	
_WebResourceData_WebResourceMIMEType_WebResourceTextEncodingName_WebResourceFrameName^WebResourceURLOA�<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <meta http-equiv="Content-Style-Type" content="text/css">
  <title></title>
  <meta name="Generator" content="Cocoa HTML Writer">
  <meta name="CocoaVersion" content="1265.19">
  <style type="text/css">
    p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 17.0px; font: 14.0px Menlo; color: #2b91af; background-color: #eeeeee}
    p.p2 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 17.0px; font: 14.0px Menlo; background-color: #eeeeee}
    p.p3 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 17.0px; font: 14.0px Menlo; background-color: #eeeeee; min-height: 16.0px}
    p.p4 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 17.0px; font: 14.0px Menlo; color: #808080; background-color: #eeeeee}
    p.p5 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 17.0px; font: 14.0px Menlo; color: #800000; background-color: #eeeeee}
    span.s1 {color: #00008b}
    span.s2 {color: #000000}
    span.s3 {color: #2b91af}
    span.s4 {color: #808080}
    span.s5 {color: #800000}
  </style>
</head>
<body>
<p class="p1"><span class="s1">import</span><span class="s2"> java.io.</span>BufferedReader<span class="s2">;</span></p>
<p class="p2"><span class="s1">import</span> java.io.<span class="s3">File</span>;</p>
<p class="p2"><span class="s1">import</span> java.io.<span class="s3">FileWriter</span>;</p>
<p class="p1"><span class="s1">import</span><span class="s2"> java.io.</span>IOException<span class="s2">;</span></p>
<p class="p1"><span class="s1">import</span><span class="s2"> java.io.</span>InputStreamReader<span class="s2">;</span></p>
<p class="p3"><br></p>
<p class="p2"><span class="s1">import</span> org.apache.http.<span class="s3">HttpResponse</span>;</p>
<p class="p2"><span class="s1">import</span> org.apache.http.auth.<span class="s3">AuthScope</span>;</p>
<p class="p1"><span class="s1">import</span><span class="s2"> org.apache.http.auth.</span>UsernamePasswordCredentials<span class="s2">;</span></p>
<p class="p2"><span class="s1">import</span> org.apache.http.client.methods.<span class="s3">HttpGet</span>;</p>
<p class="p2"><span class="s1">import</span> org.apache.http.impl.client.<span class="s3">DefaultHttpClient</span>;</p>
<p class="p3"><br></p>
<p class="p2"><span class="s1">import</span> oauth.signpost.<span class="s3">OAuthConsumer</span>;</p>
<p class="p2"><span class="s1">import</span> oauth.signpost.commonshttp.<span class="s3">CommonsHttpOAuthConsumer</span>;</p>
<p class="p3"><br></p>
<p class="p4">/**</p>
<p class="p4"><span class="Apple-converted-space"> </span>* A hacky little class illustrating how to receive and store Twitter streams</p>
<p class="p4"><span class="Apple-converted-space"> </span>* for later analysis, requires Apache Commons HTTP Client 4+. Stores the data</p>
<p class="p4"><span class="Apple-converted-space"> </span>* in 64MB long JSON files.</p>
<p class="p4"><span class="Apple-converted-space"> </span>*<span class="Apple-converted-space"> </span></p>
<p class="p4"><span class="Apple-converted-space"> </span>* Usage:</p>
<p class="p4"><span class="Apple-converted-space"> </span>*<span class="Apple-converted-space"> </span></p>
<p class="p4"><span class="Apple-converted-space"> </span>* TwitterConsumer t = new TwitterConsumer("username", "password",</p>
<p class="p4"><span class="Apple-converted-space"> </span>*<span class="Apple-converted-space">      </span>"http://stream.twitter.com/1/statuses/sample.json", "sample");</p>
<p class="p4"><span class="Apple-converted-space"> </span>* t.start();</p>
<p class="p4"><span class="Apple-converted-space"> </span>*/</p>
<p class="p1"><span class="s1">public</span><span class="s2"> </span><span class="s1">class</span><span class="s2"> </span>TwitterConsumer<span class="s2"> </span><span class="s1">extends</span><span class="s2"> </span>Thread<span class="s2"> {</span></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s4">//</span></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">static</span> <span class="s3">String</span> STORAGE_DIR = <span class="s5">"/tmp"</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">static</span> <span class="s1">long</span> BYTES_PER_FILE = <span class="s5">64</span> * <span class="s5">1024</span> * <span class="s5">1024</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s4">//</span></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">public</span> <span class="s1">long</span> <span class="s3">Messages</span> = <span class="s5">0</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">public</span> <span class="s1">long</span> <span class="s3">Bytes</span> = <span class="s5">0</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">public</span> <span class="s1">long</span> <span class="s3">Timestamp</span> = <span class="s5">0</span>;</p>
<p class="p3"><br></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">private</span> <span class="s3">String</span> accessToken = <span class="s5">""</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">private</span> <span class="s3">String</span> accessSecret = <span class="s5">""</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">private</span> <span class="s3">String</span> consumerKey = <span class="s5">""</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">private</span> <span class="s3">String</span> consumerSecret = <span class="s5">""</span>;<span class="Apple-converted-space"> </span></p>
<p class="p3"><br></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">private</span> <span class="s3">String</span> feedUrl;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">private</span> <span class="s3">String</span> filePrefix;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">boolean</span> isRunning = <span class="s1">true</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s3">File</span> file = <span class="s1">null</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s3">FileWriter</span> fw = <span class="s1">null</span>;</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">long</span> bytesWritten = <span class="s5">0</span>;</p>
<p class="p3"><br></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">public</span> <span class="s1">static</span> <span class="s1">void</span> main(<span class="s3">String</span>[] args) {</p>
<p class="p1"><span class="s2"><span class="Apple-converted-space">        </span></span>TwitterConsumer<span class="s2"> t = </span><span class="s1">new</span><span class="s2"> </span>TwitterConsumer<span class="s2">(</span></p>
<p class="p2"><span class="Apple-converted-space">            </span><span class="s5">"XXX"</span>,<span class="Apple-converted-space"> </span></p>
<p class="p2"><span class="Apple-converted-space">            </span><span class="s5">"XXX"</span>,</p>
<p class="p2"><span class="Apple-converted-space">            </span><span class="s5">"XXX"</span>,</p>
<p class="p2"><span class="Apple-converted-space">            </span><span class="s5">"XXX"</span>,</p>
<p class="p5"><span class="s2"><span class="Apple-converted-space">            </span></span>"http://stream.twitter.com/1/statuses/sample.json"<span class="s2">, </span>"sample"<span class="s2">);</span></p>
<p class="p2"><span class="Apple-converted-space">        </span>t.start();</p>
<p class="p2"><span class="Apple-converted-space">    </span>}</p>
<p class="p3"><br></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">public</span> <span class="s3">TwitterConsumer</span>(<span class="s3">String</span> accessToken, <span class="s3">String</span> accessSecret, <span class="s3">String</span> consumerKey, <span class="s3">String</span> consumerSecret, <span class="s3">String</span> url, <span class="s3">String</span> prefix) {</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s1">this</span>.accessToken = accessToken;</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s1">this</span>.accessSecret = accessSecret;</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s1">this</span>.consumerKey = consumerKey;</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s1">this</span>.consumerSecret = consumerSecret;</p>
<p class="p2"><span class="Apple-converted-space">        </span>feedUrl = url;</p>
<p class="p2"><span class="Apple-converted-space">        </span>filePrefix = prefix;</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s3">Timestamp</span> = <span class="s3">System</span>.currentTimeMillis();</p>
<p class="p2"><span class="Apple-converted-space">    </span>}</p>
<p class="p3"><br></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s4">/**</span></p>
<p class="p4"><span class="Apple-converted-space">     </span>* @throws IOException</p>
<p class="p4"><span class="Apple-converted-space">     </span>*/</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">private</span> <span class="s1">void</span> rotateFile() <span class="s1">throws</span> <span class="s3">IOException</span> {</p>
<p class="p4"><span class="s2"><span class="Apple-converted-space">        </span></span>// Handle the existing file</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s1">if</span> (fw != <span class="s1">null</span>)</p>
<p class="p2"><span class="Apple-converted-space">            </span>fw.close();</p>
<p class="p4"><span class="s2"><span class="Apple-converted-space">        </span></span>// Create the next file</p>
<p class="p2"><span class="Apple-converted-space">        </span>file = <span class="s1">new</span> <span class="s3">File</span>(STORAGE_DIR, filePrefix + <span class="s5">"-"</span></p>
<p class="p2"><span class="Apple-converted-space">                </span>+ <span class="s3">System</span>.currentTimeMillis() + <span class="s5">".json"</span>);</p>
<p class="p2"><span class="Apple-converted-space">        </span>bytesWritten = <span class="s5">0</span>;</p>
<p class="p2"><span class="Apple-converted-space">        </span>fw = <span class="s1">new</span> <span class="s3">FileWriter</span>(file);</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s3">System</span>.out.println(<span class="s5">"Writing to "</span> + file.getAbsolutePath());</p>
<p class="p2"><span class="Apple-converted-space">    </span>}</p>
<p class="p3"><br></p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s4">/**</span></p>
<p class="p4"><span class="Apple-converted-space">     </span>* @see java.lang.Thread#run()</p>
<p class="p4"><span class="Apple-converted-space">     </span>*/</p>
<p class="p2"><span class="Apple-converted-space">    </span><span class="s1">public</span> <span class="s1">void</span> run() {</p>
<p class="p4"><span class="s2"><span class="Apple-converted-space">        </span></span>// Open the initial file</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s1">try</span> { rotateFile(); } <span class="s1">catch</span> (<span class="s3">IOException</span> e) { e.printStackTrace(); <span class="s1">return</span>; }</p>
<p class="p4"><span class="s2"><span class="Apple-converted-space">        </span></span>// Run loop</p>
<p class="p2"><span class="Apple-converted-space">        </span><span class="s1">while</span> (isRunning) {</p>
<p class="p2"><span class="Apple-converted-space">            </span><span class="s1">try</span> {</p>
<p class="p3"><br></p>
<p class="p2"><span class="Apple-converted-space">                </span><span class="s3">OAuthConsumer</span> consumer = <span class="s1">new</span> <span class="s3">CommonsHttpOAuthConsumer</span>(consumerKey, consumerSecret);</p>
<p class="p2"><span class="Apple-converted-space">                </span>consumer.setTokenWithSecret(accessToken, accessSecret);</p>
<p class="p2"><span class="Apple-converted-space">                </span><span class="s3">HttpGet</span> request = <span class="s1">new</span> <span class="s3">HttpGet</span>(feedUrl);</p>
<p class="p2"><span class="Apple-converted-space">                </span>consumer.sign(request);</p>
<p class="p3"><br></p>
<p class="p1"><span class="s2"><span class="Apple-converted-space">                </span></span>DefaultHttpClient<span class="s2"> client = </span><span class="s1">new</span><span class="s2"> </span>DefaultHttpClient<span class="s2">();</span></p>
<p class="p2"><span class="Apple-converted-space">                </span><span class="s3">HttpResponse</span> response = client.execute(request);</p>
<p class="p2"><span class="Apple-converted-space">                </span><span class="s3">BufferedReader</span> reader = <span class="s1">new</span> <span class="s3">BufferedReader</span>(</p>
<p class="p2"><span class="Apple-converted-space">                        </span><span class="s1">new</span> <span class="s3">InputStreamReader</span>(response.getEntity().getContent()));</p>
<p class="p2"><span class="Apple-converted-space">                </span><span class="s1">while</span> (<span class="s1">true</span>) {</p>
<p class="p2"><span class="Apple-converted-space">                    </span><span class="s3">String</span> line = reader.readLine();</p>
<p class="p2"><span class="Apple-converted-space">                    </span><span class="s1">if</span> (line == <span class="s1">null</span>)</p>
<p class="p2"><span class="Apple-converted-space">                        </span><span class="s1">break</span>;</p>
<p class="p2"><span class="Apple-converted-space">                    </span><span class="s1">if</span> (line.length() &gt; <span class="s5">0</span>) {</p>
<p class="p2"><span class="Apple-converted-space">                        </span><span class="s1">if</span> (bytesWritten + line.length() + <span class="s5">1</span> &gt; BYTES_PER_FILE)</p>
<p class="p2"><span class="Apple-converted-space">                            </span>rotateFile();</p>
<p class="p2"><span class="Apple-converted-space">                        </span>fw.write(line + <span class="s5">"\n"</span>);</p>
<p class="p2"><span class="Apple-converted-space">                        </span>bytesWritten += line.length() + <span class="s5">1</span>;</p>
<p class="p2"><span class="Apple-converted-space">                        </span><span class="s3">Messages</span>++;</p>
<p class="p2"><span class="Apple-converted-space">                        </span><span class="s3">Bytes</span> += line.length() + <span class="s5">1</span>;</p>
<p class="p2"><span class="Apple-converted-space">                    </span>}</p>
<p class="p2"><span class="Apple-converted-space">                </span>}</p>
<p class="p2"><span class="Apple-converted-space">            </span>} <span class="s1">catch</span> (<span class="s3">Exception</span> e) {</p>
<p class="p2"><span class="Apple-converted-space">                </span>e.printStackTrace();</p>
<p class="p2"><span class="Apple-converted-space">            </span>}</p>
<p class="p5"><span class="s2"><span class="Apple-converted-space">            </span></span><span class="s3">System</span><span class="s2">.out.println(</span>"Sleeping before reconnect..."<span class="s2">);</span></p>
<p class="p2"><span class="Apple-converted-space">            </span><span class="s1">try</span> { <span class="s3">Thread</span>.sleep(<span class="s5">15000</span>); } <span class="s1">catch</span> (<span class="s3">Exception</span> e) { }</p>
<p class="p2"><span class="Apple-converted-space">        </span>}</p>
<p class="p2"><span class="Apple-converted-space">    </span>}</p>
<p class="p2">}</p>
<p class="p2">}</p>
</body>
</html>
Ytext/htmlUutf-8P_file:///index.html    ( : P n � �BWBaBgBh                           B}