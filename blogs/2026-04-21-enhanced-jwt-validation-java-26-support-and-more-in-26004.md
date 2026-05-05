---
title: "Enhanced JWT validation, Java 26 support, and more in 26.0.0.4"
url: "https://openliberty.io/blog/2026/04/21/26.0.0.4.html"
date: "2026-04-21T00:00:00+00:00"
author: "Navaneeth S Nair"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>This release introduces support for selecting JWT signature algorithms from JOSE headers and adds Java 26 support. It also removes the default LTPA keys password for enhanced security, and includes file transfer restrictions and security vulnerability fixes.</p>
</div>
<div class="paragraph">
<p>In <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.4:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/04/21/26.0.0.4.html#file_transfer">File Transfer changes for 26.0.0.4</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/21/26.0.0.4.html#ltpa">Default LTPA keys password removal</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/21/26.0.0.4.html#jwt">Support selecting JWT signature and decryption algorithms from JOSE header</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/21/26.0.0.4.html#java_26">Support for Java 26</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/21/26.0.0.4.html#displayCustomizedExceptionText">displayCustomizedExceptionText property</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/21/26.0.0.4.html#CVEs">Security Vulnerability (CVE) Fixes</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>View the list of fixed bugs in <a href="https://github.com/OpenLiberty/open-liberty/issues?q=label%3Arelease%3A26004+label%3A%22release+bug%22">26.0.0.4</a>.</p>
</div>
<div class="paragraph">
<p>Check out <a href="https://openliberty.io/blog/?search=release&amp;search!=beta">previous Open Liberty GA release blog posts</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="run">Develop and run your apps using 26.0.0.4</h2>
<div class="sectionbody">
<div class="paragraph">
<p>If you&#8217;re using <a href="https://openliberty.io/guides/maven-intro.html">Maven</a>, include the following in your <code>pom.xml</code> file:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;plugin&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>io.openliberty.tools<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>liberty-maven-plugin<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>3.12.0<span class="tag">&lt;/version&gt;</span>
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
        classpath 'io.openliberty.tools:liberty-gradle-plugin:4.0.0'
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
<p>If you&#8217;re using <a href="https://plugins.jetbrains.com/plugin/14856-liberty-tools">IntelliJ IDEA</a>, <a href="https://marketplace.visualstudio.com/items?itemName=Open-Liberty.liberty-dev-vscode-ext">Visual Studio Code</a>, or <a href="https://marketplace.eclipse.org/content/liberty-tools">Eclipse IDE</a>, you can also take advantage of our open source <a href="https://openliberty.io/docs/latest/develop-liberty-tools.html">Liberty developer tools</a> to enable effective development, testing, debugging, and application management all from within your IDE.</p>
</div>
<div class="imageblock text-center">
<div class="content">
<a class="image" href="https://stackoverflow.com/tags/open-liberty"><img alt="Ask a question on Stack Overflow" src="https://openliberty.io/img/blog/blog_btn_stack.svg" /></a>
</div>
</div>
<div class="sect2">
<h3 id="file_transfer">File Transfer changes for 26.0.0.4</h3>
<div class="paragraph">
<p>Liberty&#8217;s FileService MBean provided by the <code>restConnector-2.0</code> feature now includes an extra <code>blocklist</code> attribute. This attribute is configured by the <code>&lt;blockDir&gt;</code> config element in the <code>server.xml</code> file. The default value of this attribute is <code>${server.output.dir}/resources/security</code>. This behavior change resolves the security vulnerability <a href="https://github.com/advisories/GHSA-c39w-6qgm-5cp7">CVE-2025-14915</a>, by restricting default FileTransfer access to <code>${server.output.dir}/resources/security</code>.</p>
</div>
<div class="paragraph">
<p>If FileTransfer access to <code>${server.output.dir}/resources/security</code> is required, the original behavior can be restored by setting an empty blocklist.</p>
</div>
<div class="paragraph">
<p>For more information, see the <a href="https://www.ibm.com/docs/en/was-liberty/nd?topic=manually-file-transfer">documentation</a>.</p>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="ltpa">Default LTPA keys password removal</h2>
<div class="sectionbody">
<div class="paragraph">
<p>The default LTPA keys password is removed to resolve the security vulnerability <a href="https://www.ibm.com/support/pages/node/7266845">CVE-2025-14917</a>.</p>
</div>
<div class="paragraph">
<p>Previously, a default password for the LTPA keys was used when the <code>keysPassword</code> attribute was not defined in the <code>&lt;ltpa /&gt;</code> element. With this change, the default password is no longer supported.</p>
</div>
<div class="paragraph">
<p>For existing servers, if the LTPA keys password is not configured in the <code>server.xml</code> file, the <code>keystore_password</code> in the <code>server.env</code> file is used. This value re-encrypts the LTPA keys in the <code>ltpa.keys</code> file. The LTPA keys themselves are not impacted. The <code>keystore_password</code> is configured in the <code>server.env</code> file during server creation unless the <code>--no-password</code> option is used with the <code>server create</code> command.</p>
</div>
<div class="paragraph">
<p>If a <code>keysPassword</code> is not defined in the <code>&lt;ltpa /&gt;</code> element in the <code>server.xml</code> file and a <code>keystore_password</code> is not defined in the <code>server.env</code> file, the LTPA service fails.
The following error message is displayed:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>CWWKS4118E: LTPA configuration error. A keysPassword attribute is not configured on the &lt;ltpa /&gt; element, the 'ltpa_keys_password' environment variable is not set, and the 'keystore_password' environment variable is not set.</code></pre>
</div>
</div>
<div class="paragraph">
<p>Confirm that an LTPA keys password is set up by doing the following steps:</p>
</div>
<div class="olist arabic">
<ol class="arabic">
<li>
<p>Check to see whether a <code>keysPassword</code> attribute is provided for the <code>&lt;ltpa /&gt;</code> element in the <code>server.xml</code> file (for example, <code>&lt;ltpa keysPassword="myKeysPassword" /&gt;</code>).</p>
<div class="ulist">
<ul>
<li>
<p>If it is provided, this update does not affect you and no further action is needed.</p>
</li>
<li>
<p>If it is not provided, do <strong>not</strong> add it and proceed to the next step.</p>
</li>
</ul>
</div>
</li>
<li>
<p>Check to see whether the <code>keystore_password</code> environment variable exists in the <code>server.env</code> file (for example, <code>keystore_password=myKeystorePassword</code>).</p>
<div class="ulist">
<ul>
<li>
<p>If it exists, then the <code>keystore_password</code> is used to reencrypt the LTPA keys that were previously encrypted with the default <code>keysPassword</code> when the server starts.</p>
</li>
<li>
<p>If it does not exist, proceed to the next step.</p>
</li>
</ul>
</div>
</li>
<li>
<p>Add the following environment variable to the <code>server.env</code> file (ensure you use the <code>keystore_password</code> here and <strong>not</strong> the <code>ltpa_keys_password</code> described in the next section for new servers):</p>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>keystore_password=your-desired-password</code></pre>
</div>
</div>
<div class="ulist">
<ul>
<li>
<p>The <code>keystore_password</code> is used to reencrypt the LTPA keys that were previously encrypted with the default <code>keysPassword</code> when the server starts.</p>
</li>
</ul>
</div>
</li>
</ol>
</div>
<div class="paragraph">
<p>For new servers, a new <code>ltpa_keys_password</code> is randomly generated during server creation. It is stored in the <code>server.env</code> file unless the <code>--no-password</code> option is specified with the <code>server create</code> command. The randomly generated <code>ltpa_keys_password</code> is used if the <code>keysPassword</code> attribute is not defined for the <code>&lt;ltpa /&gt;</code> element.</p>
</div>
<div class="paragraph">
<p>For more information, see the <a href="https://openliberty.io/docs/latest/reference/config/ltpa.html">LTPA</a> configuration element.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jwt">Support selecting JWT signature and decryption algorithms from JOSE header</h2>
<div class="sectionbody">
<div class="paragraph">
<p>JSON Web Tokens (JWTs) can be signed by using various cryptographic signature algorithms. With this release, the JWT Consumer, MicroProfile JWT, OpenID Connect Client, and Social Media Login features support selecting the JWT signature algorithm from the JOSE header. This support allows different signature algorithms to be used based on the token header.</p>
</div>
<div class="paragraph">
<p>Previously, only one single signature algorithm (for example, <code>RS256</code>) was able to be configured for each configuration in the <code>server.xml</code> file. If the incoming JWT was signed with a different algorithm, validation would fail. This update allows the signature algorithm from the JWT header to be used for validation. It provides the flexibility of using different signature algorithms within a single configuration.</p>
</div>
<div class="sect2">
<h3 id="how-to-use">How to use</h3>
<div class="paragraph">
<p>To enable signature algorithm selection from the header, set the <code>signatureAlgorithm</code> attribute to <code>FROM_HEADER</code> and optionally configure the <code>allowedSignatureAlgorithms</code> attribute to specify which algorithms are permitted.</p>
</div>
<div class="paragraph">
<p>If <code>allowedSignatureAlgorithms</code> is not configured, the default list contains all Open Liberty-supported signature algorithms: <code>RS256, RS384, RS512, HS256, HS384, HS512, ES256, ES384</code>, and <code>ES512</code>.</p>
</div>
<div class="paragraph">
<p>When using <code>FROM_HEADER</code> with asymmetric algorithms and a trust store setup, the public keys must be prefixed with their corresponding algorithm (e.g., <code>RS256_keyalias</code>) for automatic selection. During validation, the server searches the trust store for an alias that begins with the algorithm specified in the JWT&#8217;s header. If no algorithm-prefixed alias is found, the client falls back to using the alias specified by the <code>trustedAlias</code> attribute (for <code>jwtConsumer</code>) or <code>trustAliasName</code> attribute (for <code>openidConnectClient</code>, <code>oidcLogin</code> and <code>mpJwt</code>), if configured.</p>
</div>
<div class="paragraph">
<p>See the following <code>server.xml</code> file configurations for examples on how to apply these settings to the supported elements:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;jwtConsumer</span>
    <span class="attribute-name">signatureAlgorithm</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">FROM_HEADER</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">allowedSignatureAlgorithms</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">RS256, ES384, HS512</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">...</span>
<span class="tag">/&gt;</span>

<span class="tag">&lt;mpJwt</span>
    <span class="attribute-name">signatureAlgorithm</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">FROM_HEADER</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">allowedSignatureAlgorithms</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">RS256, ES384, HS512</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">...</span>
<span class="tag">/&gt;</span>

<span class="tag">&lt;openidConnectClient</span>
    <span class="attribute-name">signatureAlgorithm</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">FROM_HEADER</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">allowedSignatureAlgorithms</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">RS256, ES384, HS512</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">...</span>
<span class="tag">/&gt;</span>

<span class="tag">&lt;oidcLogin</span>
    <span class="attribute-name">signatureAlgorithm</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">FROM_HEADER</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">allowedSignatureAlgorithms</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">RS256, ES384, HS512</span><span class="delimiter">&quot;</span></span>
    <span class="attribute-name">...</span>
<span class="tag">/&gt;</span></code></pre>
</div>
</div>
</div>
<div class="sect2">
<h3 id="learn-more">Learn more</h3>
<div class="paragraph">
<p><strong>Server configurations:</strong></p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/config/openidConnectClient.html">openidConnectClient</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/config/jwtConsumer.html">jwtConsumer</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/config/mpJwt.html">mpJwt</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/config/oidcLogin.html">oidcLogin</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p><strong>Documentation:</strong></p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/feature/openidConnectClient-1.0.html">OpenID Connect Client 1.0</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/feature/jwt-1.0.html">JSON Web Token 1.0</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/feature/mpJwt-2.1.html">MicroProfile JWT 2.1</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/feature/socialLogin-1.0.html">Social Media Login 1.0</a></p>
</li>
</ul>
</div>
</div>
<div class="sect2">
<h3 id="java_26">Support for Java 26</h3>
<div class="paragraph">
<p>Java 26 is a recent Java release that introduces new features and enhancements over earlier versions that can be useful to review. This release is not a long-term support (LTS) release.</p>
</div>
<div class="paragraph">
<p>There are 10 new features (JEPs) in <a href="https://openjdk.org/projects/jdk/26/">Java 26</a>. Five are test features and five are fully delivered.</p>
</div>
<div class="paragraph">
<p><strong>Test Features:</strong></p>
</div>
<div class="ulist">
<ul>
<li>
<p>524: <a href="https://openjdk.org/jeps/524">PEM Encodings of Cryptographic Objects (Second Preview)</a></p>
</li>
<li>
<p>525: <a href="https://openjdk.org/jeps/525">Structured Concurrency (Sixth Preview)</a></p>
</li>
<li>
<p>526: <a href="https://openjdk.org/jeps/526">Lazy Constants (Second Preview)</a></p>
</li>
<li>
<p>529: <a href="https://openjdk.org/jeps/529">Vector API (Eleventh Incubator)</a></p>
</li>
<li>
<p>530: <a href="https://openjdk.org/jeps/530">Primitive Types in Patterns, instanceof, and switch (Fourth Preview)</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p><strong>Delivered Features:</strong></p>
</div>
<div class="ulist">
<ul>
<li>
<p>500: <a href="https://openjdk.org/jeps/500">Prepare to Make Final Mean Final</a></p>
</li>
<li>
<p>504: <a href="https://openjdk.org/jeps/504">Remove the Applet API</a></p>
</li>
<li>
<p>516: <a href="https://openjdk.org/jeps/516">Ahead-of-Time Object Caching with Any GC</a></p>
</li>
<li>
<p>517: <a href="https://openjdk.org/jeps/517">HTTP/3 for the HTTP Client API</a></p>
</li>
<li>
<p>522: <a href="https://openjdk.org/jeps/522">G1 GC: Improve Throughput by Reducing Synchronization</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>A new change JEP 500 ("Prepare to Make Final Mean Final") in Java 26 starts enforcing true immutability of final fields by restricting their mutation when using deep reflection.
In Java 26, such mutations still work but trigger runtime warnings by default, preparing developers for stricter enforcement.
Future releases would likely throw exceptions instead, making the final truly nonmutable.</p>
</div>
<div class="paragraph">
<p>Developers can opt in early to this stricter behavior by using a JVM flag (for example, <code>--illegal-final-field-mutation=deny</code>) to detect issues sooner.
This change improves program correctness, security, and JVM optimizations.</p>
</div>
<div class="paragraph">
<p>Take advantage of these changes now to gain more time to evaluate how your applications
and microservices behave on Java 26.</p>
</div>
<div class="paragraph">
<p>Get started today by downloading the latest release of  <a href="https://developer.ibm.com/languages/java/semeru-runtimes/downloads/">IBM Semeru Runtime 26</a> or <a href="https://adoptium.net/temurin/releases/?version=26">Temurin 26</a>, then download and install the Open Liberty <a href="https://openliberty.io/start/#runtime_releases">26.0.0.4</a>. Update your Liberty server&#8217;s <a href="https://openliberty.io/docs/latest/reference/config/server-configuration-overview.html#server-env">server.env</a> file with <code>JAVA_HOME</code> set to your Java 26 installation directory and start testing.</p>
</div>
<div class="paragraph">
<p>For more information on Java 26, see the Java 26 <a href="https://jdk.java.net/26/release-notes">release notes page</a> and <a href="https://docs.oracle.com/en/java/javase/26/docs/api/index.html">API Javadoc page</a>.</p>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="displayCustomizedExceptionText">displayCustomizedExceptionText property</h2>
<div class="sectionbody">
<div class="paragraph">
<p>This release adds documentation and tests for the <code>displayCustomizedExceptionText</code> configuration, which allows users to override Liberty’s default error messages (such as SRVE0218E: Forbidden and SRVE0232E: An exception occurred) with clearer, user-defined messages.</p>
</div>
<div class="paragraph">
<p>The feature is enabled through simple <code>server.xml</code> file configuration, where custom messages can be mapped to specific HTTP status codes (<code>403</code> and <code>500</code>).</p>
</div>
<div class="paragraph">
<p>Testing ensures that these custom messages correctly replace Liberty’s defaults across all supported platforms, confirming that the configured text is returned consistently in all scenarios.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;webContainer</span> <span class="attribute-name">displaycustomizedexceptiontext</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Custom error message</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span></code></pre>
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
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2025-14915">CVE-2025-14915</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">6.5</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Privilege escalation</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">17.0.0.3-26.0.0.3</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Affects the <code>restConnector-2.0</code> feature</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2025-14917">CVE-2025-14917</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">6.7</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Weaker security</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">17.0.0.3-26.0.0.3</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Affects the <code>appSecurity-1.0</code>, <code>appSecurity-2.0</code>, <code>appSecurity-3.0</code>, <code>appSecurity-4.0</code>, and <code>appSecurity-5.0</code> features</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2026-1561">CVE-2026-1561</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">5.4</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Server-side request forgery</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">17.0.0.3-26.0.0.3</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Affects the <code>samlWeb-2.0</code> feature</p></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2026-29063">CVE-2026-29063</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">8.7</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Prototype pollution</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">17.0.0.3-26.0.0.3</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Affects the <code>openapi-3.1</code>, <code>mpOpenAPI-1.0</code>, <code>mpOpenAPI-1.1</code>, <code>mpOpenAPI-2.0</code>, <code>mpOpenAPI-3.0</code>, <code>mpOpenAPI-3.1</code>, <code>mpOpenAPI-4.0</code> and <code>mpOpenAPI-4.1</code> features</p></td>
</tr>
</tbody>
</table>
<div class="paragraph">
<p>For a list of past security vulnerability fixes, reference the <a href="https://openliberty.io/docs/latest/security-vulnerabilities.html">Security vulnerability (CVE) list</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="get-open-liberty-26-0-0-4-now">Get Open Liberty 26.0.0.4 now</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Available through <a href="https://openliberty.io/blog/2026/04/21/26.0.0.4.html#run">Maven, Gradle, Docker, and as a downloadable archive</a>.</p>
</div>
</div>
</div>
