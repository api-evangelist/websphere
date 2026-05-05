---
title: "UserRegistry Attribute Reader Enhancement, Jandex Index Format Support Update and more in 26.0.0.3"
url: "https://openliberty.io/blog/2026/03/24/26.0.0.3.html"
date: "2026-03-24T00:00:00+00:00"
author: "Navaneeth S Nair"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>This release introduces UserRegistry Attribute Reader APIs for retrieving user attributes from LDAP and custom registries, and adds support for Jandex index formats 11-13 to improve application startup performance. Other highlights include changes to AES encoding and fixes for security vulnerabilities and bugs.</p>
</div>
<div class="paragraph">
<p>In <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.3:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/03/24/26.0.0.3.html#userregistry">UserRegistry Attribute Reader Enhancement</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/03/24/26.0.0.3.html#jandex">Jandex Index Format Support Update</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/03/24/26.0.0.3.html#aes">AES encoding changes</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/03/24/26.0.0.3.html#CVEs">Security Vulnerability (CVE) Fixes</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/03/24/26.0.0.3.html#bugs">Notable bug fixes</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>View the list of fixed bugs in <a href="https://github.com/OpenLiberty/open-liberty/issues?q=label%3Arelease%3A26003+label%3A%22release+bug%22">26.0.0.3</a>.</p>
</div>
<div class="paragraph">
<p>Check out <a href="https://openliberty.io/blog/?search=release&amp;search!=beta">previous Open Liberty GA release blog posts</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="run">Develop and run your apps using 26.0.0.3</h2>
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
<h2 id="userregistry">UserRegistry Attribute Reader Enhancement</h2>
<div class="sectionbody">
<div class="paragraph">
<p>The UserRegistry API provides applications with the ability to retrieve specific user attributes and search for users based on attribute values directly from LDAP and custom user registries.</p>
</div>
<div class="sect2">
<h3 id="whats-new">What&#8217;s New?</h3>
<div class="paragraph">
<p>Previously, the UserRegistry API supported only basic user lookups such as searching by <code>userSecurityName</code> or by using wildcard pattern matching. However, it did not allow applications to retrieve specific user attributes or search for users based on attribute values. These capabilities were available in traditional WebSphere through the VMM API. The introduction of these two new APIs now enables applications to search for users and retrieve attributes from LDAP and custom user registries.</p>
</div>
<div class="paragraph">
<p>With this enhancement, you can now retrieve specific attributes for a user by using the <code>getAttributesForUser()</code> method. This method supports retrieval of individual attributes such as email or phoneNumber, or "*" to retrieve all available attributes for a user.</p>
</div>
<div class="paragraph">
<p>You can also search for users based on attribute values by using the <code>getUsersByAttribute()</code> method, enabling applications to locate users by matching specific attribute criteria. These methods are supported for LDAP user registries. Custom user registry implementations can also implement and support these methods.</p>
</div>
</div>
<div class="sect2">
<h3 id="how-to-use-it">How to Use It</h3>
<div class="paragraph">
<p>New API methods added to <code>UserRegistry.java</code>:</p>
</div>
<div class="olist arabic">
<ol class="arabic">
<li>
<p><code>default Map&lt;String, Object&gt; getAttributesForUser(String userSecurityName, Set&lt;String&gt; attributeNames)</code></p>
<div class="paragraph">
<p><strong>Description:</strong> Returns a Map&lt;String, Object&gt; containing the specified <code>attributesNames</code> and their corresponding values for the given <code>userSecurityName</code>, as configured in the LDAP user registry.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>UserRegistry ur = RegistryHelper.getUserRegistry(<span class="string"><span class="delimiter">&quot;</span><span class="content">realmName</span><span class="delimiter">&quot;</span></span>);

<span class="comment">// valid ldap attribute names</span>
<span class="predefined-type">Set</span>&lt;<span class="predefined-type">String</span>&gt; attributeNames = <span class="predefined-type">Set</span>.of(<span class="string"><span class="delimiter">&quot;</span><span class="content">telephoneNumber</span><span class="delimiter">&quot;</span></span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">uid</span><span class="delimiter">&quot;</span></span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">mail</span><span class="delimiter">&quot;</span></span>);  <span class="comment">// or &quot;*&quot; for retrieving all attributes</span>
<span class="predefined-type">Map</span>&lt;<span class="predefined-type">String</span>, <span class="predefined-type">Object</span>&gt; result = ur.getAttributesForUser(<span class="string"><span class="delimiter">&quot;</span><span class="content">testuser</span><span class="delimiter">&quot;</span></span>, attributeNames);

<span class="comment">// output in trace.log</span>
&gt; getAttributesForUser Entry
  testuser
  [telephoneNumber, uid, mail]

&lt; getAttributesForUser Exit
  {uid=testuser, mail=testuser<span class="annotation">@example</span>.com, telephoneNumber=&lt;telephone-number&gt;}</code></pre>
</div>
</div>
</li>
<li>
<p><code>default SearchResult getUsersByAttribute(String attributeName, String value, int limit)</code></p>
<div class="paragraph">
<p><strong>Description:</strong> Returns a <code>SearchResult</code> object that contains a list of users that match an <code>attributeName</code> with specified <code>value</code> that is configured in the LDAP user registry.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>UserRegistry ur = RegistryHelper.getUserRegistry(<span class="string"><span class="delimiter">&quot;</span><span class="content">realmName</span><span class="delimiter">&quot;</span></span>);

<span class="predefined-type">SearchResult</span> searchResult = ur.getUsersByAttribute(<span class="string"><span class="delimiter">&quot;</span><span class="content">email</span><span class="delimiter">&quot;</span></span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">testuser@example.com</span><span class="delimiter">&quot;</span></span>, <span class="integer">1</span>);

<span class="comment">// output in trace.log</span>
&gt; getUsersByAttribute Entry
  email
  testuser<span class="annotation">@example</span>.com
  <span class="integer">1</span>

&lt; getUsersByAttribute Exit
  <span class="predefined-type">SearchResult</span> hasMore=<span class="predefined-constant">false</span> [testuser]</code></pre>
</div>
</div>
</li>
</ol>
</div>
<div class="paragraph">
<p>To learn more about the user registry, see the following resources:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://www.ibm.com/docs/en/was-liberty/base?topic=infrastructure-customizing-user-registries-repositories-liberty">Open Liberty Custom User Registry documentation</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/modules/reference/24.0.0.3/com.ibm.websphere.appserver.api.securityClient_1.1-javadoc/com/ibm/websphere/security/UserRegistry.html">UserRegistry API Javadoc</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/user-registries-application-security.html#_federated_user_registries">Federated User Registry configuration</a></p>
</li>
</ul>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jandex">Jandex Index Format Support Update</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Open Liberty now supports the latest Jandex index formats, allowing the use of Jandex indexes that are created with Jandex version 3.1.0 and later. Jandex is a space‑efficient Java annotation indexer and offline reflection library that improves application startup performance by pre‑indexing class metadata.</p>
</div>
<div class="paragraph">
<p>This feature targets application developers and DevOps engineers who work with Jakarta EE and MicroProfile applications. It is designed to optimize application startup times and reduce runtime costs.</p>
</div>
<div class="sect2">
<h3 id="whats-new-2">What&#8217;s New?</h3>
<div class="paragraph">
<p>With this update, Open Liberty supports Jandex index formats <strong>11, 12, and 13</strong>. Jandex versions 3.1.0 through the current latest version, 3.5.3, generate indexes that use these formats.</p>
</div>
<div class="paragraph">
<p>Previously, Open Liberty supported Jandex index format versions only up to and including version 10. This index format limitation meant that applications packaged with Jandex indexes could not use index formats beyond version 10 while retaining the expected performance improvements. To preserve these performance benefits, build tooling had to continue using a Jandex version prior to 3.1.0, or explicitly configure Jandex index generation to produce indexes that use index format up to and including version 10.</p>
</div>
</div>
<div class="sect2">
<h3 id="benefits">Benefits</h3>
<div class="paragraph">
<p>The added support for Jandex index formats has several benefits:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><strong>Improved Compatibility:</strong> Works seamlessly with the latest versions of build tools and frameworks that use newer Jandex versions</p>
</li>
<li>
<p><strong>Faster Startup Times:</strong> Continue to benefit from Jandex&#8217;s efficient annotation indexing without version constraints</p>
</li>
<li>
<p><strong>Reduced Maintenance:</strong> No need to maintain multiple Jandex versions or regenerate indexes for compatibility</p>
</li>
<li>
<p><strong>Future-Proof:</strong> Stay current with the Jandex ecosystem as it evolves</p>
</li>
</ul>
</div>
</div>
<div class="sect2">
<h3 id="limitations">Limitations</h3>
<div class="paragraph">
<p><strong>When using Jandex indexes, ensure that the index files are kept up to date with the application classes.</strong> If a Jandex index is not synchronized with the application classes, it can contain incorrect annotation data, which can cause the application to function incorrectly. Out‑of‑date Jandex indexes cannot be reliably detected.</p>
</div>
<div class="paragraph">
<p>Open Liberty does not read index formats higher than index format 13. If a new version of Jandex adds a higher index format version, Open Liberty requires an update before it can read Jandex indexes that use the higher index format version.</p>
</div>
<div class="paragraph">
<p>The Open Liberty feature <code>mpGraphQL</code>, which is obtained from an external source, is limited to reading Jandex index formats no higher than index format 10. If the feature <code>mpGraphQL</code> is used, Jandex indexes should be generated by using index format 10. This limitation applies to all current versions of <code>mpGraphQL</code>, including the current highest version, <code>mpGraphQL-2.0</code>.</p>
</div>
</div>
<div class="sect2">
<h3 id="lifting-restriction">Lifting Restriction</h3>
<div class="paragraph">
<p>Previously, when Jandex usage was enabled and an application included Jandex indexes that used formats 11 through 13, Open Liberty displayed an error message and ignored those indexes. In such cases, annotation scanning was performed by using Liberty’s internal annotation scanning mechanism.</p>
</div>
<div class="paragraph">
<p>With the addition of support for Jandex index formats 11 through 13, Open Liberty now uses indexes that are generated in these formats. These indexes must be kept up to date with the application classes. Ensure that the Jandex indexes packaged with your application accurately reflect the current application classes.</p>
</div>
</div>
<div class="sect2">
<h3 id="how-to-use">How to Use</h3>
<div class="paragraph">
<p>No configuration changes are required in the <code>server.xml</code> file to enable support for the new Jandex index formats. Open Liberty automatically detects the Jandex index format and selects the appropriate index reader for that format. It reads indexes that use formats 11, 12, and 13, and continues to support indexes that use format up to version 10.</p>
</div>
<div class="paragraph">
<p>By default, Jandex usage is disabled. To enable Jandex, set the <code>useJandex</code> property to <code>true</code> in either an <code>application</code> element or the <code>applicationManager</code> element.</p>
</div>
<div class="paragraph">
<p>If you are generating Jandex indexes as part of your build process, you can now use the latest Jandex Maven plugin or Gradle plugin versions:</p>
</div>
<div class="paragraph">
<p><strong>Maven Example:</strong></p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;plugin&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>io.smallrye<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>jandex-maven-plugin<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>3.5.3<span class="tag">&lt;/version&gt;</span>
    <span class="tag">&lt;executions&gt;</span>
        <span class="tag">&lt;execution&gt;</span>
            <span class="tag">&lt;id&gt;</span>make-index<span class="tag">&lt;/id&gt;</span>
            <span class="tag">&lt;goals&gt;</span>
                <span class="tag">&lt;goal&gt;</span>jandex<span class="tag">&lt;/goal&gt;</span>
            <span class="tag">&lt;/goals&gt;</span>
        <span class="tag">&lt;/execution&gt;</span>
    <span class="tag">&lt;/executions&gt;</span>
<span class="tag">&lt;/plugin&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p><strong>Gradle Example:</strong></p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>plugins {
    id 'org.kordamp.gradle.jandex' version '2.3.0'
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>Open Liberty automatically detects and recognizes the generated <code>META-INF/jandex.idx</code> file for any supported index format version.</p>
</div>
<div class="paragraph">
<p>For more information, see the <a href="https://smallrye.io/jandex/jandex/3.5.3/index.html">Jandex Documentation</a></p>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="aes">AES encoding changes</h2>
<div class="sectionbody">
<div class="paragraph">
<p>The <code>securityUtility encode</code> command now requires a key to be specified for AES encodings. Specify a key by including one of the following options: <code>--key</code>, <code>--base64Key</code>, <code>--aesConfigFile</code>, or <code>--keyring</code>. This update resolves <a href="https://www.ibm.com/support/pages/node/7261638">CVE-2025-14923</a>.</p>
</div>
<div class="paragraph">
<p>The following command now results in an error because a key is not specified in the command.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>securityUtility encode --encoding=aes &quot;password&quot;

When using AES encoding, one of the following arguments must be specified: --key, --base64Key, --aesConfigFile, or --keyring.</code></pre>
</div>
</div>
<div class="paragraph">
<p>The following is an example of a valid usage with the new requirement.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>securityUtility encode --encoding=aes --key=mycustomkey &quot;password&quot;
{aes}ARArmkfkgGyE1eiN8mKKjnkUDrFUFuuq2FlpDqrJKBgTAEYeN+PLP45wChLvIhlnWEDHnSA4zJI9KA3k5R9e/QDC+O2tzr3EwD3IMMAfvfOYxxqNPmoXqIeRVPD8TPZWnIIPmCnvyROw5A8=</code></pre>
</div>
</div>
<div class="paragraph">
<p>For more information, see the full documentation for <a href="https://openliberty.io/docs/latest/reference/command/securityUtility-encode.html">securityUtility encode</a> and <a href="https://openliberty.io/docs/latest/password-encryption.html">password encryption</a>.</p>
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
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2025-14923">CVE-2025-14923</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">4.7</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Weaker security</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">17.0.0.3-26.0.0.2</p></td>
<td class="tableblock halign-left valign-top"></td>
</tr>
<tr>
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2024-29371">CVE-2024-29371</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">7.5</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Denial of service</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">21.0.0.3-26.0.0.2</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Affects the <code>openidConnectClient-1.0</code>, <code>socialLogin-1.0</code>, <code>mpJwt-1.2</code>, <code>mpJwt-2.0</code>, <code>mpJwt-2.1</code>, and <code>jwt-1.0</code> features</p></td>
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
<p>We’ve spent some time fixing bugs. The following sections describe some of the issues resolved in this release. If you’re interested, here’s the <a href="https://github.com/OpenLiberty/open-liberty/issues?q=label%3Arelease%3A26003+label%3A%22release+bug%22">full list of bugs fixed in 26.0.0.3</a>.</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/34017">Missing error message in JMX REST client</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/34052">NullPointerException resurfaced in Open Liberty due to removal of EclipseLink 2.7.16 fix</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/34171">IBM WebSphere Application Server Liberty is affected by a denial of service due to jose4j (CVE-2024-29371 CVSS 7.5)</a></p>
</li>
</ul>
</div>
</div>
</div>
<div class="sect1">
<h2 id="get-open-liberty-26-0-0-3-now">Get Open Liberty 26.0.0.3 now</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Available through <a href="https://openliberty.io/blog/2026/03/24/26.0.0.3.html#run">Maven, Gradle, Docker, and as a downloadable archive</a>.</p>
</div>
</div>
</div>
