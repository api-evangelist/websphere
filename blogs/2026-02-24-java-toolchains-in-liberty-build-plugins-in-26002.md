---
title: "Java Toolchains in Liberty Build Plugins in 26.0.0.2"
url: "https://openliberty.io/blog/2026/02/24/26.0.0.2.html"
date: "2026-02-24T00:00:00+00:00"
author: "Navaneeth S Nair"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>This release introduces Java Toolchains support, enabling developers to decouple the JDK used to run build tools from the JDK used to run the Liberty server.</p>
</div>
<div class="paragraph">
<p>In <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.2:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/02/24/26.0.0.2.html#java_toolchains">Java Toolchains in Liberty Build Plugins</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/02/24/26.0.0.2.html#CVEs">Security Vulnerability (CVE) Fixes</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/02/24/26.0.0.2.html#bugs">Notable bug fixes</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>View the list of fixed bugs in <a href="https://github.com/OpenLiberty/open-liberty/issues?q=label%3Arelease%3A26002+label%3A%22release+bug%22">26.0.0.2</a>.</p>
</div>
<div class="paragraph">
<p>Check out <a href="https://openliberty.io/blog/?search=release&amp;search!=beta">previous Open Liberty GA release blog posts</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="run">Develop and run your apps using 26.0.0.2</h2>
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
        classpath 'io.openliberty.tools:liberty-gradle-plugin:3.10.0'
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
<h2 id="java_toolchains">Java Toolchains in Liberty Build Plugins</h2>
<div class="sectionbody">
<div class="paragraph">
<p>In the latest release of the Liberty build plugins, support has been added for Java Toolchains. This enhancement enables developers to decouple the JDK used to run their build tools (Maven or Gradle) from the JDK used to run the Liberty server and their applications. This provides greater flexibility and environmental consistency.</p>
</div>
<div class="sect2">
<h3 id="java-toolchains-support">Java Toolchains support</h3>
<div class="paragraph">
<p>The Liberty build plugins now support the standard Java Toolchain mechanism.
Previously, Liberty plugins were restricted to using the same Java version that was running Maven or Gradle.
This prevented developers from using more recent JDK versions for their build process if their applications required a specific older JDK version.</p>
</div>
<div class="paragraph">
<p>With Java Toolchains, you can now run your build tool on a modern JDK (for example, Java 25). At the same time, Liberty servers and all server operations can execute using a different, configured JDK (for example, Java 8).</p>
</div>
</div>
<div class="sect2">
<h3 id="maven-plugin-integration">Maven Plugin integration</h3>
<div class="paragraph">
<p>The Liberty Maven Plugin now integrates seamlessly with the maven-toolchain-plugin as of version 3.12.0.
To use this feature, define your available JDKs in your <code>~/.m2/toolchains.xml</code> file and then configure <code>&lt;jdkToolchain&gt;</code> tag in <code>&lt;configuration&gt;</code>.
The plugin automatically detects and uses the toolchain specified in your project’s <code>pom.xml</code> file.</p>
</div>
<div class="paragraph">
<p>For detailed configuration steps and parameters, see the <a href="https://github.com/OpenLiberty/ci.maven/blob/main/docs/toolchain.md">Liberty Maven Plugin Toolchain documentation</a>.</p>
</div>
<div class="paragraph">
<p>The plugin acknowledges the JDK vendor and version constraints that are defined in your Maven profiles, helping to ensure that your server environment remains consistent across different developer machines and CI/CD pipelines.</p>
</div>
</div>
<div class="sect2">
<h3 id="gradle-plugin-integration">Gradle Plugin integration</h3>
<div class="paragraph">
<p>The Liberty Gradle plugin now recognizes the native <code>java { toolchain { &#8230;&#8203; } }</code> configuration block as of version 3.10.0. This configuration provides a unified way to manage Java versions across multi-project builds, where different services might have different runtime requirements.</p>
</div>
<div class="paragraph">
<p>For detailed configuration steps and parameters, see the <a href="https://github.com/OpenLiberty/ci.gradle/blob/main/docs/toolchain.md">Liberty Gradle Plugin Toolchain documentation</a>.</p>
</div>
<div class="paragraph">
<p>When a toolchain is configured in your <code>build.gradle</code>, the Liberty plugin uses that specific Java runtime for all server-related tasks (for example, <code>libertyDev</code> and <code>libertyStart</code>).</p>
</div>
</div>
<div class="sect2">
<h3 id="try-it-now">Try it now</h3>
<div class="paragraph">
<p>To start using Java Toolchains, update your build tools to the latest versions of the Liberty Maven or Gradle plugins.</p>
</div>
<div class="paragraph">
<p><strong>For Maven:</strong></p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;plugin&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>io.openliberty.tools<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>liberty-maven-plugin<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>3.12.0<span class="tag">&lt;/version&gt;</span>
    <span class="tag">&lt;configuration&gt;</span>
        <span class="tag">&lt;jdkToolchain&gt;</span>
            <span class="tag">&lt;version&gt;</span>11<span class="tag">&lt;/version&gt;</span>
            <span class="comment">&lt;!-- Optional: include vendor if you need to distinguish between toolchains --&gt;</span>
             <span class="comment">&lt;!-- &lt;vendor&gt;ibm&lt;/vendor&gt; --&gt;</span>
        <span class="tag">&lt;/jdkToolchain&gt;</span>
    <span class="tag">&lt;/configuration&gt;</span>
<span class="tag">&lt;/plugin&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p><strong>For Gradle:</strong></p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath 'io.openliberty.tools:liberty-gradle-plugin:3.10.0'
    }
}
apply plugin: 'liberty'


java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
        // Optional: specify vendor
        // vendor = JvmVendorSpec.ADOPTIUM
    }
}</code></pre>
</div>
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
<td class="tableblock halign-left valign-top"><p class="tableblock"><a href="https://www.cve.org/CVERecord?id=CVE-2025-14914">CVE-2025-14914</a></p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">7.6</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Remote code execution</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">17.0.0.3-26.0.0.1</p></td>
<td class="tableblock halign-left valign-top"><p class="tableblock">Affects the <code>restConnector-2.0</code> feature</p></td>
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
<p>We’ve spent some time fixing bugs. The following sections describe the issues resolved in this release. If you’re interested, here’s the <a href="https://github.com/OpenLiberty/open-liberty/issues?q=label%3Arelease%3A26002+label%3A%22release+bug%22">full list of bugs fixed in 26.0.0.2</a>.</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/33927">IBM WebSphere Application Server Liberty is affected by a remote code execution vulnerability (CVE-2025-14914 CVSS 7.6)</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/32996">"WARNING: package sun.security.action not in java.base" shows up in console.log starting in Java 24</a></p>
</li>
</ul>
</div>
</div>
</div>
<div class="sect1">
<h2 id="get-open-liberty-26-0-0-2-now">Get Open Liberty 26.0.0.2 now</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Available through <a href="https://openliberty.io/blog/2026/02/24/26.0.0.2.html#run">Maven, Gradle, Docker, and as a downloadable archive</a>.</p>
</div>
</div>
</div>
