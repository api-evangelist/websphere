---
title: "Support for Java Toolchains in Liberty Build Plugins"
url: "https://openliberty.io/blog/2026/03/02/java-toolchains-in-liberty-build-plugins.html"
date: "2026-03-02T00:00:00+00:00"
author: "Arun Venmany"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>Liberty Gradle and Maven plugins now support <strong>Java Toolchains</strong>. This feature allows you to decouple the JDK used to run your build tool (Gradle or Maven) from the JDK used to compile your code and run your Liberty server and applications.</p>
</div>
<div class="paragraph">
<p>When a toolchain is configured, the Liberty build plugins automatically detect the specified JDK and use it to start the Liberty server, ensuring environment consistency across your development team.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="why-use-java-toolchains">Why use Java Toolchains?</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Using Java toolchains provides several advantages for enterprise development and CI/CD pipelines:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><strong>Environment Consistency</strong>: Ensure every developer and build server uses the exact same JDK version for the runtime, regardless of what is installed as the system default.</p>
</li>
<li>
<p><strong>Decoupling</strong>: You can run your build tool (e.g., Maven or Gradle) on a newer, high-performance JVM while targeting an older JDK (like Java 8 or 11) for the actual Liberty server.</p>
</li>
<li>
<p><strong>Simplified Configuration</strong>: Avoid manually setting and maintaining <code>JAVA_HOME</code> environment variables across different machines. The build tool handles the discovery and provisioning of the required JDK.</p>
</li>
<li>
<p><strong>Early Detection</strong>: If a developer tries to build a project but lacks the required JDK version, the build tool will provide a clear error or automatically download the correct version (depending on your configuration).</p>
</li>
</ul>
</div>
</div>
</div>
<div class="sect1">
<h2 id="minimum-version-requirements">Minimum Version Requirements</h2>
<div class="sectionbody">
<div class="paragraph">
<p>To use toolchain support, ensure you are using the following plugin versions or newer:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><strong>Liberty Maven Plugin</strong>: <a href="https://central.sonatype.com/artifact/io.openliberty.tools/liberty-maven-plugin/3.12.0">3.12.0</a></p>
</li>
<li>
<p><strong>Liberty Gradle Plugin</strong>: <a href="https://central.sonatype.com/artifact/io.openliberty.tools/liberty-gradle-plugin/3.10.0">3.10.0</a></p>
</li>
</ul>
</div>
</div>
</div>
<div class="sect1">
<h2 id="liberty-gradle-plugin-support">Liberty Gradle Plugin Support</h2>
<div class="sectionbody">
<div class="paragraph">
<p>The Liberty Gradle plugin integrates with the standard <a href="https://docs.gradle.org/current/userguide/toolchains.html">Gradle Java Toolchain</a> feature. It does not require a Liberty-specific configuration block; it automatically honors the project&#8217;s <code>java { toolchain { &#8230;&#8203; } }</code> settings.</p>
</div>
<div class="sect2">
<h3 id="how-to-use">How to use</h3>
<div class="paragraph">
<p>Apply the <code>java</code> plugin and define the desired Java version in your <code>build.gradle</code> file:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>plugins {
    id 'java'
    id 'io.openliberty.tools.gradle.Liberty' version '3.10.0'
}

java {
    toolchain {
        // Required Java version for your project
        languageVersion = JavaLanguageVersion.of(17)
        // Optional: include vendor if you need to distinguish between toolchains
        // vendor.set(JvmVendorSpec.ADOPTIUM)
    }
}</code></pre>
</div>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="liberty-maven-plugin-support">Liberty Maven Plugin Support</h2>
<div class="sectionbody">
<div class="paragraph">
<p>By leveraging Maven Toolchains, the Liberty Maven plugin can decouple the JDK used to run the plugin from the JDK used to run your Liberty server. It retrieves these definitions from your local <code>toolchains.xml</code>.</p>
</div>
<div class="sect2">
<h3 id="how-to-use-2">How to use</h3>
<div class="olist arabic">
<ol class="arabic">
<li>
<p><strong>Configure toolchains.xml</strong>: Ensure your <code>~/.m2/toolchains.xml</code> defines the JDKs available on your system.</p>
</li>
<li>
<p><strong>Update pom.xml</strong>: Configure the plugin versions in your <code>liberty-maven-plugin</code> configurations and add a <code>&lt;jdkToolchain&gt;</code> configuration.</p>
</li>
</ol>
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
</div>
</div>
</div>
<div class="sect1">
<h2 id="dev-mode-behavior">Dev mode behavior</h2>
<div class="sectionbody">
<div class="paragraph">
<p>When you run <code>gradle libertyDev</code> or <code>mvn liberty:dev</code>, the plugin ensures environment consistency by applying the following logic:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><strong>Server JVM</strong>: Sets the Liberty server process&#8217;s <code>JAVA_HOME</code> to the configured toolchain JDK.</p>
</li>
<li>
<p><strong>Recompilation</strong>: Automatically uses the toolchain JDK for compilation (e.g., <code>compileJava</code> and <code>compileTestJava</code> tasks in Gradle, or <code>compiler:compile</code> and <code>compiler:testCompile</code> goals in Maven).</p>
</li>
<li>
<p><strong>Logs</strong>: Look for confirmation in the terminal output to verify the toolchain is active:</p>
</li>
</ul>
</div>
<div class="hdlist">
<table>
<tr>
<td class="hdlist1">
Gradle
</td>
<td class="hdlist2">
<p><code>CWWKM4101I: The :libertyDev task is using the configured toolchain JDK located at /path/to/jdk-17</code></p>
</td>
</tr>
<tr>
<td class="hdlist1">
Maven
</td>
<td class="hdlist2">
<p><code>CWWKM4101I: The liberty:dev goal is using the configured toolchain JDK located at /path/to/jdk-17</code></p>
</td>
</tr>
</table>
</div>
</div>
</div>
<div class="sect1">
<h2 id="rules-and-precedence">Rules and precedence</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Both plugins follow a specific order of operations:</p>
</div>
<div class="olist arabic">
<ol class="arabic">
<li>
<p><strong>Manual Overrides</strong>: If <code>JAVA_HOME</code> is set in <code>server.env</code> or <code>jvm.options</code>, <strong>that value takes precedence</strong> over the Java toolchain.</p>
</li>
<li>
<p><strong>Toolchain Resolution</strong>: If no manual override exists, the plugin uses the resolved toolchain JDK.</p>
</li>
<li>
<p><strong>Fallback</strong>: If no toolchain is found, the plugin falls back to the JVM currently running the build.</p>
</li>
</ol>
</div>
</div>
</div>
<div class="sect1">
<h2 id="verification">Verification</h2>
<div class="sectionbody">
<div class="paragraph">
<p>To confirm which JDK the Liberty server started with, check the <code>messages.log</code> file (located in <code>${server.output.dir}/logs/</code>). You should see entries similar to the following:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>********************************************************************************
product = Open Liberty 25.0.0.12 (wlp-1.0.108.cl251220251117-0302)
wlp.install.dir = /path/to/project/build/wlp/
java.home = /path/to/java/semeru-11.0.28/Contents/Home
java.version = 11.0.28
java.runtime = IBM Semeru Runtime Open Edition (11.0.28+6)
os = Mac OS X (26.1; aarch64) (en_IN)
process = 62955@Device-Name.local
Classpath = /path/to/project/build/wlp/bin/tools/ws-server.jar
Java Library path = /path/to/java/semeru-11.0.28/Contents/Home/lib/default:/path/to/java/semeru-11.0.28/Contents/Home/lib:/usr/lib
********************************************************************************</code></pre>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="conclusion">Conclusion</h2>
<div class="sectionbody">
<div class="paragraph">
<p>With Java toolchain support in the Liberty Gradle and Maven plugins, managing your development environment becomes significantly more robust. By defining your Java version at the project level, you eliminate the "it works on my machine" class of bugs related to mismatched JDKs. Whether you are using Gradle&#8217;s automatic provisioning or Maven&#8217;s toolchain manager, your Liberty server will now stay in perfect sync with your source code&#8217;s requirements.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="further-documentation">Further Documentation</h2>
<div class="sectionbody">
<div class="paragraph">
<p>For detailed technical specifications, refer to the <code>toolchains.md</code> documentation in the plugin repositories:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>Liberty Gradle Plugin: <a href="https://github.com/OpenLiberty/ci.gradle/blob/main/docs/toolchain.md">toolchain.md</a></p>
</li>
<li>
<p>Liberty Maven Plugin: <a href="https://github.com/OpenLiberty/ci.maven/blob/main/docs/toolchain.md">toolchain.md</a></p>
</li>
</ul>
</div>
</div>
</div>
<div class="sect1">
<h2 id="reporting-issues">Reporting Issues</h2>
<div class="sectionbody">
<div class="paragraph">
<p>We welcome feedback and contributions! If you encounter any bugs or have feature requests, please report them in our open-source repositories:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>For Maven issues, visit the <a href="https://github.com/OpenLiberty/ci.maven/issues">Liberty Maven Plugin GitHub Issues</a>.</p>
</li>
<li>
<p>For Gradle issues, visit the <a href="https://github.com/OpenLiberty/ci.gradle/issues">Liberty Gradle Plugin GitHub Issues</a>.</p>
</li>
</ul>
</div>
</div>
</div>
