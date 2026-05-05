---
title: "Model Context Protocol Server 1.0 updates in 26.0.0.3-beta"
url: "https://openliberty.io/blog/2026/03/10/26.0.0.3-beta.html"
date: "2026-03-10T00:00:00+00:00"
author: "Ismath Badsha"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>The 26.0.0.3-beta release updates the <code>mcpServer-1.0</code> feature with response encoders and provides request ID access to tools for logging and auditing.</p>
</div>
<div class="paragraph">
<p>The <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.3-beta includes the following beta features (along with <a href="https://openliberty.io/docs/latest/reference/feature/feature-overview.html">all GA features</a>):</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/03/10/26.0.0.3-beta.html#encoders">Encode responses with ContentEncoder and ToolResponseEncoder</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/03/10/26.0.0.3-beta.html#requestid">Access the request ID from a tool</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>The <a href="https://modelcontextprotocol.io/docs/getting-started/intro">Model Context Protocol (MCP)</a> is an open standard that enables AI applications to access real-time information from external sources. The Liberty MCP Server feature (<code>mcpServer-1.0</code>) allows developers to expose the business logic of their applications, allowing it to be integrated into agentic AI workflows.</p>
</div>
<div class="paragraph">
<p>See also <a href="https://openliberty.io/blog/?search=beta&amp;key=tag">previous Open Liberty beta blog posts</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="encoders">Encode responses with ContentEncoder and ToolResponseEncoder</h2>
<div class="sectionbody">
<div class="paragraph">
<p>When a client calls a tool, the object that is returned by the tool method is converted to a <code>ToolResponse</code>, which usually contains one or more <code>Content</code> objects. The <code>ToolResponse</code> maps directly to the result returned in the response.</p>
</div>
<div class="paragraph">
<p>If you need more control over the response from your tool, you can return a <code>ToolResponse</code> or <code>Content</code> object directly. Now, it is also possible to register encoders to control how other objects are converted into a response.</p>
</div>
<div class="ulist">
<ul>
<li>
<p>Use <code>ToolResponseEncoder</code> to convert an object into a <code>ToolResponse</code>, which gives you complete control over the whole response.</p>
</li>
<li>
<p>Use <code>ContentEncoder</code> when you only need to convert an object into a <code>Content</code> that is included in the response. You can also return a list of objects, and each object is individually converted into a <code>Content</code> and included in the response.</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>If a tool method returns an object for which no encoder is provided, JSON-B encodes the object as JSON, which is returned as text content.</p>
</div>
<div class="sect2">
<h3 id="example">Example</h3>
<div class="paragraph">
<p>Consider a tool that does a search over some datastore:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Tool</span>(description = <span class="string"><span class="delimiter">&quot;</span><span class="content">search the data store</span><span class="delimiter">&quot;</span></span>)
<span class="directive">public</span> <span class="predefined-type">SearchResult</span> search(<span class="annotation">@ToolArg</span>(name=<span class="string"><span class="delimiter">&quot;</span><span class="content">query</span><span class="delimiter">&quot;</span></span>, description=<span class="string"><span class="delimiter">&quot;</span><span class="content">the query to run</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> query) {
    <span class="predefined-type">SearchResult</span> result = datastore.runQuery(query);
    <span class="keyword">return</span> result;
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>In this scenario, the <code>SearchResult</code> object encapsulates a list of individual results. Each entry contains a summary, associated metadata, and a relevance score that indicates its relationship to the query.</p>
</div>
<div class="paragraph">
<p>By default, the <code>SearchResult</code> object that is returned is encoded as JSON by using JSON-B and placed into a <code>TextContent</code>. However, to return each search result summary as an individual <code>TextContent</code> and map the relevance score to a priority annotation, a <code>ToolResponseEncoder</code> is to be used.</p>
</div>
<div class="paragraph">
<p>Create a CDI bean that implements the <code>ToolResponseEncoder</code> interface and implement:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>the <code>encode</code> method to create a <code>ToolResponse</code> from a <code>SearchResult</code></p>
</li>
<li>
<p>the <code>supports</code> method to indicate that your encoder can be used for any <code>SearchResult</code></p>
</li>
</ul>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@ApplicationScoped</span>
<span class="directive">public</span> <span class="type">class</span> <span class="class">SearchResultEncoder</span> <span class="directive">implements</span> ToolResponseEncoder&lt;<span class="predefined-type">SearchResult</span>&gt; {

    <span class="directive">public</span> <span class="type">boolean</span> supports(<span class="predefined-type">Class</span>&lt;?&gt; runtimeType) {
        <span class="comment">// This encoder can encode SearchResult and any subtypes</span>
        <span class="keyword">return</span> <span class="predefined-type">SearchResult</span>.class.isAssignableFrom(runtimeType);
    }

    <span class="directive">public</span> ToolResponse encode(<span class="predefined-type">SearchResult</span> searchResult) {
        <span class="keyword">if</span> (searchResult.results().isEmpty()) {
            <span class="keyword">return</span> ToolResponse.error(<span class="string"><span class="delimiter">&quot;</span><span class="content">No results</span><span class="delimiter">&quot;</span></span>);
        }

        <span class="predefined-type">ArrayList</span>&lt;TextContent&gt; contents = <span class="keyword">new</span> <span class="predefined-type">ArrayList</span>&lt;&gt;();
        <span class="keyword">for</span> (<span class="type">var</span> result : searchResult.results()) {
            <span class="comment">// Set the priority annotation based on the relevance</span>
            Annotations annotations = <span class="keyword">new</span> Annotations(<span class="predefined-constant">null</span>, <span class="predefined-constant">null</span>, result.relevance());
            <span class="comment">// Create a TextContent from the summary, and add the annotations</span>
            contents.add(<span class="keyword">new</span> TextContent(result.summary(), <span class="predefined-constant">null</span>, annotations));
        }

        <span class="comment">// Create a successful response, using the created TextContents</span>
        <span class="keyword">return</span> ToolResponse.success(contents);
    }
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>This encoder is used for any tools in the application that return a <code>SearchResult</code></p>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="requestid">Access the request ID from a tool</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Each request from an MCP client includes a unique session-based Request ID, which is now accessible to tools and is useful for logging and auditing.</p>
</div>
<div class="paragraph">
<p>To access the ID, add a <code>RequestId</code> parameter to the tool method:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Tool</span>(description = <span class="string"><span class="delimiter">&quot;</span><span class="content">search the data store</span><span class="delimiter">&quot;</span></span>)
<span class="directive">public</span> <span class="predefined-type">SearchResult</span> search(<span class="annotation">@ToolArg</span>(name=<span class="string"><span class="delimiter">&quot;</span><span class="content">query</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> query, RequestId requestId) {
    logger.log(INFO, <span class="string"><span class="delimiter">&quot;</span><span class="content">Running search (</span><span class="delimiter">&quot;</span></span> + requestId.asString() + <span class="string"><span class="delimiter">&quot;</span><span class="content">) for query: </span><span class="delimiter">&quot;</span></span> + query);
    <span class="comment">// ....</span>
}</code></pre>
</div>
</div>
<div class="sect2">
<h3 id="notable-bug-fixes">Notable bug fixes</h3>
<div class="paragraph">
<p>The following bugs have been fixed:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>Output schemas were not generated correctly for asynchronous tool methods which have <code>structuredContent = true</code></p>
</li>
<li>
<p>Omitting the <code>arguments</code> object when calling a tool would result in an error. The <code>arguments</code> object is optional if the tool does not require any arguments</p>
</li>
</ul>
</div>
</div>
<div class="sect2">
<h3 id="run">Try it now</h3>
<div class="paragraph">
<p>To try out these features, update your build tools to pull the Open Liberty All Beta Features package instead of the main release. To enable the MCP server feature, follow the instructions from <a href="https://openliberty.io/blog/2025/10/23/mcp-standalone-blog.html">MCP standalone blog</a>. The beta works with Java SE 25, Java SE 21, Java SE 17, Java SE 11, and Java SE 8.</p>
</div>
<div class="paragraph">
<p>If you&#8217;re using <a href="https://openliberty.io/guides/maven-intro.html">Maven</a>, you can install the All Beta Features package using:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;plugin&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>io.openliberty.tools<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>liberty-maven-plugin<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>3.12.0<span class="tag">&lt;/version&gt;</span>
    <span class="tag">&lt;configuration&gt;</span>
        <span class="tag">&lt;runtimeArtifact&gt;</span>
          <span class="tag">&lt;groupId&gt;</span>io.openliberty.beta<span class="tag">&lt;/groupId&gt;</span>
          <span class="tag">&lt;artifactId&gt;</span>openliberty-runtime<span class="tag">&lt;/artifactId&gt;</span>
          <span class="tag">&lt;version&gt;</span>26.0.0.3-beta<span class="tag">&lt;/version&gt;</span>
          <span class="tag">&lt;type&gt;</span>zip<span class="tag">&lt;/type&gt;</span>
        <span class="tag">&lt;/runtimeArtifact&gt;</span>
    <span class="tag">&lt;/configuration&gt;</span>
<span class="tag">&lt;/plugin&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>You must also add dependencies to your <code>pom.xml</code> file for the beta version of the APIs that are associated with the beta features that you want to try. For example, the following block adds dependencies for two example beta APIs:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;dependency&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>org.example.spec<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>exampleApi<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>7.0<span class="tag">&lt;/version&gt;</span>
    <span class="tag">&lt;type&gt;</span>pom<span class="tag">&lt;/type&gt;</span>
    <span class="tag">&lt;scope&gt;</span>provided<span class="tag">&lt;/scope&gt;</span>
<span class="tag">&lt;/dependency&gt;</span>
<span class="tag">&lt;dependency&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>example.platform<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>example.example-api<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>11.0.0<span class="tag">&lt;/version&gt;</span>
    <span class="tag">&lt;scope&gt;</span>provided<span class="tag">&lt;/scope&gt;</span>
<span class="tag">&lt;/dependency&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>Or for <a href="https://openliberty.io/guides/gradle-intro.html">Gradle</a>:</p>
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
dependencies {
    libertyRuntime group: 'io.openliberty.beta', name: 'openliberty-runtime', version: '[26.0.0.3-beta,)'
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>Or if you&#8217;re using <a href="https://openliberty.io/docs/latest/container-images.html">container images</a>:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>FROM icr.io/appcafe/open-liberty:beta</code></pre>
</div>
</div>
<div class="paragraph">
<p>Or take a look at our <a href="https://openliberty.io/downloads/#runtime_betas">Downloads page</a>.</p>
</div>
<div class="paragraph">
<p>If you&#8217;re using <a href="https://plugins.jetbrains.com/plugin/14856-liberty-tools">IntelliJ IDEA</a>, <a href="https://marketplace.visualstudio.com/items?itemName=Open-Liberty.liberty-dev-vscode-ext">Visual Studio Code</a> or <a href="https://marketplace.eclipse.org/content/liberty-tools">Eclipse IDE</a>, you can also take advantage of our open source <a href="https://openliberty.io/docs/latest/develop-liberty-tools.html">Liberty developer tools</a> to enable effective development, testing, debugging and application management all from within your IDE.</p>
</div>
<div class="paragraph">
<p>For more information on using a beta release, refer to the <a href="https://openliberty.io/blog/2026/03/10/docs/latest/installing-open-liberty-betas.html">Installing Open Liberty beta releases</a> documentation.</p>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="feedback">We welcome your feedback</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Let us know what you think on <a href="https://groups.io/g/openliberty">our mailing list</a>. If you hit a problem, <a href="https://stackoverflow.com/questions/tagged/open-liberty">post a question on StackOverflow</a>. If you hit a bug, <a href="https://github.com/OpenLiberty/open-liberty/issues">please raise an issue</a>.</p>
</div>
</div>
</div>
