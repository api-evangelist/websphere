---
title: "Model Context Protocol Server 1.0 updates and more in 26.0.0.2-beta"
url: "https://openliberty.io/blog/2026/02/10/26.0.0.2-beta.html"
date: "2026-02-10T00:00:00+00:00"
author: "Navaneeth S Nair"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>This beta release enhances the <code>mcpServer-1.0</code> feature with role-based authorization, the new <code>_meta</code> field, and key bug fixes. It also adds documentation and tests for <code>displayCustomizedExceptionText</code>, allowing users to replace default Liberty error messages with clearer, custom text.</p>
</div>
<div class="paragraph">
<p>The <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.2-beta includes the following beta features (along with <a href="https://openliberty.io/docs/latest/reference/feature/feature-overview.html">all GA features</a>):</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/02/10/26.0.0.2-beta.html#mcp">Model Context Protocol Server 1.0 updates</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/02/10/26.0.0.2-beta.html#displayCustomizedExceptionText">displayCustomizedExceptionText property</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>See also <a href="https://openliberty.io/blog/?search=beta&amp;key=tag">previous Open Liberty beta blog posts</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="mcp">Model Context Protocol Server 1.0 updates</h2>
<div class="sectionbody">
<div class="paragraph">
<p>The <a href="https://modelcontextprotocol.io/docs/getting-started/intro">Model Context Protocol (MCP)</a> is an open standard that enables AI applications to access real-time information from external sources. The Liberty MCP Server feature (<code>mcpServer-1.0</code>) allows developers to expose the business logic of their applications, allowing it to be integrated into agentic AI workflows.</p>
</div>
<div class="paragraph">
<p>This beta release of Open Liberty includes important updates to the <code>mcpServer-1.0</code> feature including role-based authorization, the <code>_meta</code> field, and a few bug fixes.</p>
</div>
<div class="sect2">
<h3 id="prerequisites">Prerequisites</h3>
<div class="paragraph">
<p>To use the <code>mcpServer-1.0</code> feature, Java 17 or later must be installed on the system.</p>
</div>
</div>
<div class="sect2">
<h3 id="implement-role-based-authorization-for-mcp-tools-via-annotations">Implement role-based authorization for MCP tools via annotations</h3>
<div class="paragraph">
<p>The following new annotations allow you to restrict tool usage through authorization policies:</p>
</div>
<div class="olist arabic">
<ol class="arabic">
<li>
<p><code>@DenyAll</code> - Resource is denied. This annotation is the strictest policy.</p>
</li>
<li>
<p><code>@RolesAllowed</code> - Resource is allowed for pre-authorised users in a role (the same as a group in Liberty).</p>
</li>
<li>
<p><code>@PermitAll</code> - Resource is allowed for anyone (even unauthenticated users).</p>
</li>
</ol>
</div>
<div class="paragraph">
<p>These security annotations are declared on two levels:</p>
</div>
<div class="olist arabic">
<ol class="arabic">
<li>
<p>Class level - Every method in the class inherits this class level annotation.</p>
</li>
<li>
<p>Method level - Overrides any class level annotations.</p>
</li>
</ol>
</div>
<div class="paragraph">
<p>For complete reference documentation, see the <a href="https://jakarta.ee/specifications/security/">Jakarta Security Annotations specification</a>.</p>
</div>
<div class="paragraph">
<p>The <code>mcpServer-1.0</code> feature does not currently implement all of the <a href="https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization#authorization-server-metadata-discovery">authorization-server-metadata-discovery requirements</a> specified in the MCP specification. However, you can use any of Liberty&#8217;s <a href="https://openliberty.io/docs/latest/authentication.html">authentication and authorization mechanisms</a> to authenticate users and assign roles. The <code>mcpServer-1.0</code> feature can then ensure that users can access only the tools that are permitted for their assigned roles.</p>
</div>
<div class="sect3">
<h4 id="basic-authorization-example">Basic authorization example</h4>
<div class="paragraph">
<p>The following example shows how to configure MCP tools to require authentication and to ensure that users have the appropriate roles to access specific tools. Users authenticate using Basic Authentication, and the list of users and their roles are configured in a basicRegistry in the <code>server.xml</code> file.</p>
</div>
<div class="paragraph">
<p>Consider an online bookshop that has different users each with different authorization levels:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>Users - can buy books and track prices</p>
</li>
<li>
<p>Moderators - can manage the books available</p>
</li>
<li>
<p>Admins - can manually ban users</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>Clients who connect without authenticating can only display the available books or register as a new user.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="comment">// common imports for all classes below</span>

<span class="keyword">import</span> <span class="include">io.openliberty.mcp.annotations.Tool</span>;
<span class="keyword">import</span> <span class="include">io.openliberty.mcp.annotations.ToolArg</span>;
<span class="keyword">import</span> <span class="include">jakarta.annotation.security.PermitAll</span>;
<span class="keyword">import</span> <span class="include">jakarta.annotation.security.RolesAllowed</span>;
<span class="keyword">import</span> <span class="include">jakarta.enterprise.context.ApplicationScoped</span>;</code></pre>
</div>
</div>
</div>
<div class="sect3">
<h4 id="security-annotation-example-classes">Security Annotation example classes</h4>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@ApplicationScoped</span>
<span class="directive">public</span> <span class="type">class</span> <span class="class">BookShopTools</span> {

    <span class="comment">// Admin Tool</span>
    <span class="annotation">@RolesAllowed</span>(<span class="string"><span class="delimiter">&quot;</span><span class="content">Admins</span><span class="delimiter">&quot;</span></span>)
    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">banUser</span><span class="delimiter">&quot;</span></span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> banUser(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">userName</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Name of user to be banned</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> userName)  {...}

    <span class="comment">// Moderator Tools</span>
    <span class="annotation">@RolesAllowed</span>(<span class="string"><span class="delimiter">&quot;</span><span class="content">Moderators</span><span class="delimiter">&quot;</span></span>)
    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">addBook</span><span class="delimiter">&quot;</span></span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> addBook(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">bookCode</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Code to uniquely identify the book</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> bookCode)  {...}

    <span class="annotation">@RolesAllowed</span>(<span class="string"><span class="delimiter">&quot;</span><span class="content">Moderators</span><span class="delimiter">&quot;</span></span>)
    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">deleteBook</span><span class="delimiter">&quot;</span></span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> deleteBook(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">bookCode</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Code to uniquely identify the book</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> bookCode)  {...}

    <span class="comment">// User Tools</span>
    <span class="annotation">@RolesAllowed</span>(<span class="string"><span class="delimiter">&quot;</span><span class="content">Users</span><span class="delimiter">&quot;</span></span>)
    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">buyBook</span><span class="delimiter">&quot;</span></span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> buyBook(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">bookCode</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Code to uniquely identify the book</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> bookCode) {...}

    <span class="annotation">@RolesAllowed</span>(<span class="string"><span class="delimiter">&quot;</span><span class="content">Users</span><span class="delimiter">&quot;</span></span>)
    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">trackBookPrice</span><span class="delimiter">&quot;</span></span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> trackBookPrice(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">bookCode</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Code to uniquely identify the book</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> bookCode)  {...}
}</code></pre>
</div>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@ApplicationScoped</span>
<span class="annotation">@PermitAll</span> <span class="comment">// PermitAll Class level Annotation applies to all class tools, unless overwritten on method level</span>
<span class="directive">public</span> <span class="type">class</span> <span class="class">PublicTools</span> {

    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">displayBooks</span><span class="delimiter">&quot;</span></span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> displayBooks(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">bookCode</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Code to uniquely identify the book</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> book)  {...}

    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">registerNewUser</span><span class="delimiter">&quot;</span></span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> registerNewUser(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">userName</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">The name for the user to be registered</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> input,
                                  <span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">password</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">The password for the user to be registered</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> input)  {...}
}</code></pre>
</div>
</div>
</div>
<div class="sect3">
<h4 id="steps-required">Steps required</h4>
<div class="ulist">
<ul>
<li>
<p>Create an application with <code>@ApplicationScoped</code> and expose the tools with the required annotations.</p>
</li>
<li>
<p>Create a <code>server.xml</code> file with the users.</p>
</li>
<li>
<p>Ensure that the groups map to the roles created in the tool.</p>
</li>
<li>
<p>Add users to the groups in the <code>server.xml</code> file.</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>The following additional configuration is required in the <code>server.xml</code> (See the comments in the XML example for details.):</p>
</div>
<div class="ulist">
<ul>
<li>
<p>The <code>appSecurity</code> feature</p>
</li>
<li>
<p>The <code>transportSecurity</code> feature</p>
</li>
<li>
<p>Define a user registry</p>
</li>
<li>
<p>Enable TLS (Transport Layer Security) for security</p>
</li>
<li>
<p>Require TLS (Transport Layer Security) to ensure that credentials aren&#8217;t sent in cleartext</p>
</li>
</ul>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;server</span> <span class="attribute-name">description</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Liberty server config for Security Annotations</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>

  <span class="tag">&lt;featureManager&gt;</span>
    <span class="tag">&lt;feature&gt;</span>servlet-6.0<span class="tag">&lt;/feature&gt;</span>
    <span class="tag">&lt;feature&gt;</span>cdi-4.0<span class="tag">&lt;/feature&gt;</span>
    <span class="tag">&lt;feature&gt;</span>mcpServer-1.0<span class="tag">&lt;/feature&gt;</span>
    <span class="comment">&lt;!-- The following two features are required for application security to work --&gt;</span>
    <span class="tag">&lt;feature&gt;</span>appSecurity-5.0<span class="tag">&lt;/feature&gt;</span>
    <span class="tag">&lt;feature&gt;</span>transportSecurity-1.0<span class="tag">&lt;/feature&gt;</span>
  <span class="tag">&lt;/featureManager&gt;</span>

  <span class="tag">&lt;include</span> <span class="attribute-name">location</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">../fatTestPorts.xml</span><span class="delimiter">&quot;</span></span> <span class="tag">/&gt;</span>

  <span class="comment">&lt;!-- Required for passwords to be encrypted using HTTPS --&gt;</span>
  <span class="tag">&lt;keyStore</span> <span class="attribute-name">id</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">defaultKeyStore</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Liberty</span><span class="delimiter">&quot;</span></span> <span class="tag">/&gt;</span>

  <span class="comment">&lt;!-- Required to define https port for TLS (Transport Layer Security) --&gt;</span>
  <span class="tag">&lt;httpEndpoint</span> <span class="attribute-name">id</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">defaultHttpEndpoint</span><span class="delimiter">&quot;</span></span>
                <span class="attribute-name">httpPort</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">-1</span><span class="delimiter">&quot;</span></span>
                <span class="attribute-name">httpsPort</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">9443</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>
  <span class="tag">&lt;/httpEndpoint&gt;</span>

  <span class="tag">&lt;basicRegistry</span> <span class="attribute-name">id</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">basic</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">realm</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">BasicRealm</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>

       <span class="comment">&lt;!-- Basic user authentication setup --&gt;</span>

       <span class="comment">&lt;!-- Users --&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Sam</span><span class="delimiter">&quot;</span></span>   <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LDwoMC07aw==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Nile</span><span class="delimiter">&quot;</span></span>  <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LRwoMC07aw==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Cloe</span><span class="delimiter">&quot;</span></span>  <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LEwoMC07aw==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Vick</span><span class="delimiter">&quot;</span></span>  <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LFwoMC07aw==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>

       <span class="comment">&lt;!-- Moderators --&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Sally</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LCwoMC07bQ==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Harry</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LCwoMC07bQ==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>

       <span class="comment">&lt;!-- Admins --&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Bob</span><span class="delimiter">&quot;</span></span>   <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LCwoMC07bA==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;user</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Noah</span><span class="delimiter">&quot;</span></span>  <span class="attribute-name">password</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}KzosKy8+LCwoMC07aw==</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>


       <span class="comment">&lt;!-- Mapping users to groups for the RolesAllowed annotation (a group is equivalent to a role) --&gt;</span>

       <span class="tag">&lt;group</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Users</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Nile</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Sam</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Cloe</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Vick</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;/group&gt;</span>

       <span class="tag">&lt;group</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Moderators</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Sally</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Harry</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;/group&gt;</span>

       <span class="tag">&lt;group</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Admins</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Bob</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
           <span class="tag">&lt;member</span> <span class="attribute-name">name</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Noah</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>
       <span class="tag">&lt;/group&gt;</span>

  <span class="tag">&lt;/basicRegistry&gt;</span>
<span class="tag">&lt;/server&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>It is also possible to add multiple roles to tools: <code>@RolesAllowed("Admins, Moderators")</code>. Doing so grants access to users who have either role.</p>
</div>
<div class="paragraph">
<p>If a role that is used in <code>@RolesAllowed</code> annotation is not mentioned in the <code>server.xml</code> file, that just means that no user has that role. If it is the only role that is allowed to call the tool, then no users can call the tool.</p>
</div>
</div>
</div>
<div class="sect2">
<h3 id="mcp-tools-can-access-metadata-that-the-client-sends-in-the-_meta-field">MCP: Tools can access metadata that the client sends in the _meta field.</h3>
<div class="paragraph">
<p>The <a href="https://modelcontextprotocol.io/specification/2025-06-18/basic/index#meta">MCP specification</a> reserves the <code>_meta</code> property or parameter so clients and servers can attach additional metadata to their interactions.
Key name format: valid <code>_meta</code> key names have two segments:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>An optional prefix (e.g. "example.com/") and</p>
</li>
<li>
<p>A name ("myKey")</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>The whole <code>_meta</code> key is seen as "example.com/myKey"</p>
</div>
<div class="sect3">
<h4 id="adding-a-tool-meta-parameter">Adding a Tool Meta parameter</h4>
<div class="paragraph">
<p>The following code shows an example metaKey used to retrieve the value of the metadata. In the example, the metakey is "example.org/location"</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>    <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">noArgsRequest</span><span class="delimiter">&quot;</span></span>,
          title = <span class="string"><span class="delimiter">&quot;</span><span class="content">call tool without providing arguments in params</span><span class="delimiter">&quot;</span></span>,
          description = <span class="string"><span class="delimiter">&quot;</span><span class="content">return string made from args and metadata</span><span class="delimiter">&quot;</span></span>,
          structuredContent = <span class="predefined-constant">false</span>)
    <span class="directive">public</span> <span class="predefined-type">String</span> noArgsRequest(Meta meta) {
        Jsonb jsonb = JsonbBuilder.create();
        <span class="predefined-type">String</span> location = (<span class="predefined-type">String</span>) meta.getValue(MetaKey.from(<span class="string"><span class="delimiter">&quot;</span><span class="content">example.org/location</span><span class="delimiter">&quot;</span></span>));
        <span class="predefined-type">BigDecimal</span> timestamp = (<span class="predefined-type">BigDecimal</span>) meta.getValue(MetaKey.from(<span class="string"><span class="delimiter">&quot;</span><span class="content">timestamp</span><span class="delimiter">&quot;</span></span>));
        <span class="predefined-type">String</span> result = <span class="string"><span class="delimiter">&quot;</span><span class="content">You have called this tool from </span><span class="delimiter">&quot;</span></span> + location + <span class="string"><span class="delimiter">&quot;</span><span class="content"> at timestamp </span><span class="delimiter">&quot;</span></span> + timestamp.toString();
        <span class="keyword">return</span> result;
    }</code></pre>
</div>
</div>
</div>
<div class="sect3">
<h4 id="returning-metadata-in-a-tool-response">Returning metadata in a tool response</h4>
<div class="paragraph">
<p>A method can return any metadata by using the <code>ToolResponse</code> object:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>   <span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">addPersonToListToolResponse</span><span class="delimiter">&quot;</span></span>,
         title = <span class="string"><span class="delimiter">&quot;</span><span class="content">adds person to people list</span><span class="delimiter">&quot;</span></span>,
         description = <span class="string"><span class="delimiter">&quot;</span><span class="content">adds person to people list</span><span class="delimiter">&quot;</span></span>,
         structuredContent = <span class="predefined-constant">true</span>)
    <span class="directive">public</span> <span class="annotation">@Schema</span>(value = <span class="string"><span class="delimiter">&quot;</span><span class="content">{ ... }</span><span class="delimiter">&quot;</span></span>,description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Returns list of person object</span><span class="delimiter">&quot;</span></span>)
    ToolResponse addPersonToListToolResponse(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">employeeList</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">List of people</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">List</span>&lt;Person&gt; employeeList,
                                             <span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">person</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Person object</span><span class="delimiter">&quot;</span></span>) Optional&lt;Person&gt; person) {
        Person personInstance = person.get();
        employeeList.add(personInstance);
        Jsonb jsonb = JsonbBuilder.create();
        <span class="predefined-type">Map</span>&lt;MetaKey, <span class="predefined-type">Object</span>&gt; _meta = <span class="keyword">new</span> <span class="predefined-type">HashMap</span>&lt;&gt;();
        _meta.put(MetaKey.from(<span class="string"><span class="delimiter">&quot;</span><span class="content">timestamp</span><span class="delimiter">&quot;</span></span>), <span class="integer">1762860699</span>);
        _meta.put(MetaKey.from(<span class="string"><span class="delimiter">&quot;</span><span class="content">example.org/location</span><span class="delimiter">&quot;</span></span>), <span class="string"><span class="delimiter">&quot;</span><span class="content">Hursley</span><span class="delimiter">&quot;</span></span>);
        _meta.put(MetaKey.from(<span class="string"><span class="delimiter">&quot;</span><span class="content">example.org/person</span><span class="delimiter">&quot;</span></span>), personInstance);
        <span class="keyword">return</span> <span class="keyword">new</span> ToolResponse(<span class="predefined-constant">false</span>, <span class="predefined-type">List</span>.of(<span class="keyword">new</span> TextContent(jsonb.toJson(employeeList))), employeeList, _meta);
    }</code></pre>
</div>
</div>
<div class="paragraph">
<p>The <code>_meta</code> field enables protocol extensions without breaking compatibility. The following examples illustrate common use cases:</p>
</div>
</div>
<div class="sect3">
<h4 id="1-cost-tracking-extension">1) Cost Tracking Extension</h4>
<div class="paragraph">
<p>Track API costs for expensive operations.</p>
</div>
<div class="paragraph">
<p><strong>How it works:</strong></p>
</div>
<div class="ulist">
<ul>
<li>
<p>Server reports the cost of executing a tool in the result metadata.</p>
</li>
<li>
<p>Client tracks total spending across operations.</p>
</li>
</ul>
</div>
</div>
<div class="sect3">
<h4 id="2-rate-limiting-extension">2) Rate Limiting Extension</h4>
<div class="paragraph">
<p>Prevent resource exhaustion and ensure fair access.</p>
</div>
<div class="paragraph">
<p><strong>How it works:</strong></p>
</div>
<div class="ulist">
<ul>
<li>
<p>Metadata shows the remaining quota before each call for time window</p>
</li>
<li>
<p>If limit is exceeded, the error includes retry time in <code>_meta</code></p>
</li>
</ul>
</div>
</div>
<div class="sect3">
<h4 id="3-caching-hints-extension">3) Caching Hints Extension</h4>
<div class="paragraph">
<p>Optimize performance through intelligent caching.</p>
</div>
<div class="paragraph">
<p><strong>How it works:</strong></p>
</div>
<div class="ulist">
<ul>
<li>
<p>Server includes caching information in the tool call result</p>
</li>
<li>
<p>Client can implement client-side caching</p>
</li>
</ul>
</div>
</div>
<div class="sect3">
<h4 id="4-bringing-it-all-together">4) Bringing it all together</h4>
<div class="paragraph">
<p>All three of the preceding extensions might be built into your MCP server as extensions:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>{
  <span class="key"><span class="delimiter">&quot;</span><span class="content">name</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">ToolForMetaDataExample</span><span class="delimiter">&quot;</span></span>,
  <span class="key"><span class="delimiter">&quot;</span><span class="content">description</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">ToolDescription</span><span class="delimiter">&quot;</span></span>,
  <span class="key"><span class="delimiter">&quot;</span><span class="content">inputSchema</span><span class="delimiter">&quot;</span></span>: { <span class="error">.</span><span class="error">.</span><span class="error">.</span> },
  <span class="key"><span class="delimiter">&quot;</span><span class="content">_meta</span><span class="delimiter">&quot;</span></span>: {
    <span class="error">/</span><span class="error">/</span> <span class="error">R</span><span class="error">a</span><span class="error">t</span><span class="error">e</span> <span class="error">L</span><span class="error">i</span><span class="error">m</span><span class="error">i</span><span class="error">t</span><span class="error">i</span><span class="error">n</span><span class="error">g</span> <span class="error">E</span><span class="error">x</span><span class="error">t</span><span class="error">e</span><span class="error">n</span><span class="error">s</span><span class="error">i</span><span class="error">o</span><span class="error">n</span> <span class="error">-</span> <span class="error">c</span><span class="error">o</span><span class="error">m</span><span class="error">.</span><span class="error">e</span><span class="error">x</span><span class="error">a</span><span class="error">m</span><span class="error">p</span><span class="error">l</span><span class="error">e</span><span class="error">.</span><span class="error">r</span><span class="error">a</span><span class="error">t</span><span class="error">e</span><span class="error">L</span><span class="error">i</span><span class="error">m</span><span class="error">i</span><span class="error">t</span> <span class="error">n</span><span class="error">a</span><span class="error">m</span><span class="error">e</span><span class="error">s</span><span class="error">p</span><span class="error">a</span><span class="error">c</span><span class="error">e</span>
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.rateLimit/requests</span><span class="delimiter">&quot;</span></span>: <span class="integer">100</span>,
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.rateLimit/period</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">hour</span><span class="delimiter">&quot;</span></span>,
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.rateLimit/remaining</span><span class="delimiter">&quot;</span></span>: <span class="integer">47</span>,
    <span class="error">/</span><span class="error">/</span> <span class="error">C</span><span class="error">o</span><span class="error">s</span><span class="error">t</span> <span class="error">T</span><span class="error">r</span><span class="error">a</span><span class="error">c</span><span class="error">k</span><span class="error">i</span><span class="error">n</span><span class="error">g</span> <span class="error">E</span><span class="error">x</span><span class="error">t</span><span class="error">e</span><span class="error">n</span><span class="error">s</span><span class="error">i</span><span class="error">o</span><span class="error">n</span> <span class="error">-</span> <span class="error">c</span><span class="error">o</span><span class="error">m</span><span class="error">.</span><span class="error">e</span><span class="error">x</span><span class="error">a</span><span class="error">m</span><span class="error">p</span><span class="error">l</span><span class="error">e</span><span class="error">.</span><span class="error">c</span><span class="error">o</span><span class="error">s</span><span class="error">t</span> <span class="error">n</span><span class="error">a</span><span class="error">m</span><span class="error">e</span><span class="error">s</span><span class="error">p</span><span class="error">a</span><span class="error">c</span><span class="error">e</span>
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.cost/estimatedCost</span><span class="delimiter">&quot;</span></span>: <span class="float">0.02</span>,
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.cost/currency</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">USD</span><span class="delimiter">&quot;</span></span>,
    <span class="error">/</span><span class="error">/</span> <span class="error">C</span><span class="error">a</span><span class="error">c</span><span class="error">h</span><span class="error">i</span><span class="error">n</span><span class="error">g</span> <span class="error">H</span><span class="error">i</span><span class="error">n</span><span class="error">t</span><span class="error">s</span> <span class="error">E</span><span class="error">x</span><span class="error">t</span><span class="error">e</span><span class="error">n</span><span class="error">s</span><span class="error">i</span><span class="error">o</span><span class="error">n</span> <span class="error">-</span> <span class="error">c</span><span class="error">o</span><span class="error">m</span><span class="error">.</span><span class="error">e</span><span class="error">x</span><span class="error">a</span><span class="error">m</span><span class="error">p</span><span class="error">l</span><span class="error">e</span><span class="error">.</span><span class="error">c</span><span class="error">a</span><span class="error">c</span><span class="error">h</span><span class="error">e</span> <span class="error">n</span><span class="error">a</span><span class="error">m</span><span class="error">e</span><span class="error">s</span><span class="error">p</span><span class="error">a</span><span class="error">c</span><span class="error">e</span>
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.cache/cacheable</span><span class="delimiter">&quot;</span></span>: <span class="value">true</span>,
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.cache/cacheKey</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">LondonWeather</span><span class="delimiter">&quot;</span></span>,
    <span class="key"><span class="delimiter">&quot;</span><span class="content">com.example.cache/ttl</span><span class="delimiter">&quot;</span></span>: <span class="integer">300</span>
  }
}</code></pre>
</div>
</div>
</div>
</div>
<div class="sect2">
<h3 id="notable-bug-fixes-in-mcp-1-0">Notable bug fixes in MCP 1.0</h3>
<div class="sect3">
<h4 id="1-mcp-server-feature-used-iso-8859-1-and-did-not-handle-non-latin-characters">1) MCP Server feature used ISO-8859-1 and did not handle non-Latin characters</h4>
<div class="ulist">
<ul>
<li>
<p>MCP requests and responses are now encoded correctly using UTF-8.</p>
</li>
</ul>
</div>
</div>
<div class="sect3">
<h4 id="2-fix-asynchronous-mcp-tool-error-handling">2) Fix asynchronous MCP Tool error handling</h4>
<div class="ulist">
<ul>
<li>
<p>Previously, when an asynchronous tool returned a failed completion stage, the system did not verify whether the exception was a business or technical error. This resulted in all failures being reported as internal errors.</p>
</li>
<li>
<p>The system now differentiates between exception types. This categorization ensures that business errors are returned to the client, while technical errors are reported in the Liberty server log.</p>
</li>
</ul>
</div>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="displayCustomizedExceptionText">displayCustomizedExceptionText property</h2>
<div class="sectionbody">
<div class="paragraph">
<p>This beta release adds documentation and tests for the <code>displayCustomizedExceptionText</code> configuration, which allows users to override Liberty’s default error messages (such as SRVE0218E: Forbidden and SRVE0232E: An exception occurred) with clearer, user-defined messages.</p>
</div>
<div class="paragraph">
<p>The feature is enabled through simple <code>server.xml</code> file configuration, where custom messages can be mapped to specific HTTP status codes (403 and 500).</p>
</div>
<div class="paragraph">
<p>Testing ensures that these custom messages correctly replace Liberty’s defaults across all supported platforms, confirming that the configured text is returned consistently in all scenarios.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;webContainer</span> <span class="attribute-name">displaycustomizedexceptiontext</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">Custom error message</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span></code></pre>
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
          <span class="tag">&lt;version&gt;</span>26.0.0.2-beta<span class="tag">&lt;/version&gt;</span>
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
    libertyRuntime group: 'io.openliberty.beta', name: 'openliberty-runtime', version: '[26.0.0.2-beta,)'
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
<p>For more information on using a beta release, refer to the <a href="https://openliberty.io/blog/2026/02/10/docs/latest/installing-open-liberty-betas.html">Installing Open Liberty beta releases</a> documentation.</p>
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
