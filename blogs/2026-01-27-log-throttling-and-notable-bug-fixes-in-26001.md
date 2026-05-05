---
title: "Log throttling and notable bug fixes in 26.0.0.1"
url: "https://openliberty.io/blog/2026/01/27/26.0.0.1.html"
date: "2026-01-27T00:00:00+00:00"
author: "Navaneeth S Nair"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>In this release, Open Liberty introduces log throttling to automatically suppress excessive, repeated log messages, helping developers reduce noise and manage high-volume logging more effectively.</p>
</div>
<div class="paragraph">
<p>In <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.1:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/01/27/26.0.0.1.html#logging">Log Throttling</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/01/27/26.0.0.1.html#CVEs">Security Vulnerability (CVE) Fixes</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/01/27/26.0.0.1.html#bugs">Notable bug fixes</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>Along with the new features and functions added to the runtime, we’ve also made <a href="https://openliberty.io/blog/2026/01/27/26.0.0.1.html#guides">updates to our guides</a>.</p>
</div>
<div class="paragraph">
<p>View the list of fixed bugs in <a href="https://github.com/OpenLiberty/open-liberty/issues?q=label%3Arelease%3A26001+label%3A%22release+bug%22">26.0.0.1</a>.</p>
</div>
<div class="paragraph">
<p>Check out <a href="https://openliberty.io/blog/?search=release&amp;search!=beta">previous Open Liberty GA release blog posts</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="run">Develop and run your apps using 26.0.0.1</h2>
<div class="sectionbody">
<div class="paragraph">
<p>If you&#8217;re using <a href="https://openliberty.io/guides/maven-intro.html">Maven</a>, include the following in your <code>pom.xml</code> file:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;plugin&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>io.openliberty.tools<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>liberty-maven-plugin<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>3.11.5<span class="tag">&lt;/version&gt;</span>
<span class="tag">&lt;/plugin&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>Or for <a href="https://openliberty.io/guides/gradle-intro.html">Gradle</a>, include the following in your <code>build.gradle</code> file:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'io.openliberty.tools:liberty-gradle-plugin:3.9.6'
    }
}
apply plugin: 'liberty'</code></pre>
</div>
</div>
<div class="paragraph">
<p>Or if you&#8217;re using <a href="https://openliberty.io/docs/latest/container-images.html">container images</a>:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>FROM icr.io/appcafe/open-liberty</code></pre>
</div>
</div>
<div class="paragraph">
<p>Or take a look at our <a href="https://openliberty.io/start/">Downloads page</a>.</p>
</div>
<div class="paragraph">
<p>If you&#8217;re using <a href="https://plugins.jetbrains.com/plugin/14856-liberty-tools">IntelliJ IDEA</a>, <a href="https://marketplace.visualstudio.com/items?itemName=Open-Liberty.liberty-dev-vscode-ext">Visual Studio Code</a> or <a href="https://marketplace.eclipse.org/content/liberty-tools">Eclipse IDE</a>, you can also take advantage of our open source <a href="https://openliberty.io/docs/latest/develop-liberty-tools.html">Liberty developer tools</a> to enable effective development, testing, debugging and application management all from within your IDE.</p>
</div>
<div class="imageblock text-center">
<div class="content">
<a class="image" href="https://stackoverflow.com/tags/open-liberty"><img alt="Ask a question on Stack Overflow" src="https://openliberty.io/img/blog/blog_btn_stack.svg" /></a>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="logging">Log Throttling</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Open Liberty Logging is introducing log throttling. Developers previously had no way to throttle or suppress high-volume messages. This new feature helps to prevent excessive log output when the same log events occur repeatedly within a short span of time.</p>
</div>
<div class="paragraph">
<p>Throttling is enabled by default. While enabled, Liberty tracks each messageID using a sliding window. By default, any messageID that is repeated more than 1,000 times within a five-minute interval is suppressed. A throttle warning is logged when throttling begins.</p>
</div>
<div class="paragraph">
<p>Log throttling can be configured to throttle messages based on the message or messageID by using the <code>throttleType</code> logging attribute. The number of messages allowed before throttling begins can be configured by using the <code>throttleMaxMessagesPerWindow</code> attribute.</p>
</div>
<div class="paragraph">
<p>Log Throttling is disabled by setting <code>throttleMaxMessagesPerWindow</code> to <code>0</code>.</p>
</div>
<div class="paragraph">
<p>Currently, these attributes can be configured as follows:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>In <code>server.xml</code>:</p>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;logging</span> <span class="attribute-name">throttleMaxMessagesPerWindow</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">5000</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">throttleType</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">messageID</span><span class="delimiter">&quot;</span></span> <span class="tag">/&gt;</span></code></pre>
</div>
</div>
</li>
<li>
<p>In <code>bootstrap.properties</code>:</p>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>com.ibm.ws.logging.throttle.max.messages.per.window=5000
com.ibm.ws.logging.throttle.type=messageID</code></pre>
</div>
</div>
</li>
<li>
<p>In <code>server.env</code>:</p>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>WLP_LOGGING_THROTTLE_MAX_MESSAGES_PER_WINDOW=5000
WLP_LOGGING_THROTTLE_TYPE=messageID</code></pre>
</div>
</div>
</li>
</ul>
</div>
<div class="sect2">
<h3 id="difference-between-message-and-messageid">Difference between message and messageID:</h3>
<div class="paragraph">
<p>Consider the following example of a log event: <code>TEST0111I: Hello World!</code>.</p>
</div>
<div class="paragraph">
<p>When <code>throttleType</code> is set to <code>messageID</code>, throttling is exclusively applied based on the number of occurrences of the messageID. In this example, <code>TEST0111I</code> is used for throttling.</p>
</div>
<div class="paragraph">
<p>When <code>throttleType</code> is set to <code>message</code>, throttling is applied to the entire message. Any variation in the message content is tracked separately. In this example, <code>TEST0111I: Hello World!</code> is used for throttling.</p>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="CVEs">Security vulnerability (CVE) fixes in this release</h2>
<div class="sectionbody">
<table class="tableblock frame-all grid-all stretch">
<colgroup>
<col style="width: 20%;" />
<col style="width: 20%;" />
<col style="width: 20%;" />
<col style="width: 20%;" />
<col style="width: 20%;" />
</colgroup>
<thead>
<tr>
<th class="tableblock halign-left valign-top">CVE</th>
<th class="tableblock halign-left valign-top">CVSS Score</th>
<th class="tableblock halign-left valign-top">Vulnerability Assessment</th>
<th class="tableblock halign-left valign-top">Versions Affected</th>
<th class="tableblock halign-left valign-top">Notes</th>
</tr>
</thead>
<tbody>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2025-12635">CVE-2025-12635</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">5.4</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Cross-site scripting</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">17.0.0.3-25.0.0.12</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Affects the <code>servlet-3.1</code>, <code>servlet-4.0</code>, <code>servlet-5.0</code>, and <code>servlet-6.0</code> features</p></td>
</tr>
</tbody>
</table>
<div class="paragraph">
<p>For a list of past security vulnerability fixes, reference the <a href="https://openliberty.io/docs/latest/security-vulnerabilities.html">Security vulnerability (CVE) list</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="bugs">Notable bugs fixed in this release</h2>
<div class="sectionbody">
<div class="paragraph">
<p>We’ve spent some time fixing bugs. The following sections describe just some of the issues resolved in this release. If you’re interested, here’s the  <a href="https://github.com/OpenLiberty/open-liberty/issues?q=label%3Arelease%3A26001+label%3A%22release+bug%22">full list of bugs fixed in 26.0.0.1</a>.</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/33686">NullPointerException occurs in SocketRWChannelSelector</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/33617">IBM WebSphere Application Server Liberty is affected by cross-site scripting (CVE-2025-12635 CVSS 5.4)</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/33609">wlp passwords keys may fail to decode</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/33571">mpOpenAPI does not correctly merge x-ibm-zcon-roles-allowed</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/33561">Server package &lt;server&gt; --include=minify does not include Liberty&#8217;s FIPS properties file</a></p>
</li>
</ul>
</div>
</div>
</div>
<div class="sect1">
<h2 id="guides">New and updated guides since the previous release</h2>
<div class="sectionbody">
<div class="paragraph">
<p>As Open Liberty features and functionality continue to grow, we continue to add <a href="https://openliberty.io/guides/?search=new&amp;key=tag">new guides to openliberty.io</a> on those topics to make their adoption as easy as possible.</p>
</div>
<div class="paragraph">
<p>Two new guides have been published under the <a href="https://openliberty.io/guides/#observability">Observability</a> category:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/guides/microprofile-telemetry-grafana-automatic.html">Enabling observability in microservices with traces, metrics, and logs using OpenTelemetry and Grafana</a></p>
</li>
<li>
<p><a href="https://openliberty.io/guides/microprofile-telemetry-grafana-custom.html">Adding custom tracing and metrics for microservice observability using OpenTelemetry and Grafana</a></p>
</li>
</ul>
</div>
</div>
</div>
<div class="sect1">
<h2 id="get-open-liberty-26-0-0-1-now">Get Open Liberty 26.0.0.1 now</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Available through <a href="https://openliberty.io/blog/2026/01/27/26.0.0.1.html#run">Maven, Gradle, Docker, and as a downloadable archive</a>.</p>
</div>
</div>
</div>
