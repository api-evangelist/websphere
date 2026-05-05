---
title: "Log Throttling and Model Context Protocol Server 1.0 updates in 26.0.0.1-beta"
url: "https://openliberty.io/blog/2026/01/13/26.0.0.1-beta.html"
date: "2026-01-13T00:00:00+00:00"
author: "Navaneeth S Nair"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>In this beta release, Open Liberty introduces log throttling to prevent excessive log output by limiting repeated high-volume messages within a short time span. It also includes updates to the <code>mcpServer-1.0</code> feature.</p>
</div>
<div class="paragraph">
<p>The <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.1-beta includes the following beta features (along with <a href="https://openliberty.io/docs/latest/reference/feature/feature-overview.html">all GA features</a>):</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/01/13/26.0.0.1-beta.html#logging">Log Throttling</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/01/13/26.0.0.1-beta.html#mcp">Model Context Protocol Server 1.0 updates</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>See also <a href="https://openliberty.io/blog/?search=beta&amp;key=tag">previous Open Liberty beta blog posts</a>.</p>
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
<p>Log throttling can be configured to throttle messages based on the message or messageID by using the <code>throttleType</code> logging attribute. The number of messages allowed before throttling can be configured by using the <code>throttleMaxMessagesPerWindow</code> attribute.</p>
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
<pre class="CodeRay highlight"><code><span class="tag">&lt;logging</span> <span class="attribute-name">throttleMaxMessagesPerWindow</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">5000</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">throttleType</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">message</span><span class="delimiter">&quot;</span></span> <span class="tag">/&gt;</span></code></pre>
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
<h2 id="mcp">Model Context Protocol Server 1.0 updates</h2>
<div class="sectionbody">
<div class="paragraph">
<p>The <a href="https://modelcontextprotocol.io/docs/getting-started/intro">Model Context Protocol (MCP)</a> is an open standard that enables AI applications to access real-time information from external sources. The Liberty MCP Server feature <code>mcpServer-1.0</code> allows developers to expose the business logic of their applications, allowing it to be integrated into agentic AI workflows.</p>
</div>
<div class="paragraph">
<p>This beta release of Open Liberty includes important updates to the <code>mcpServer-1.0</code> feature, including asynchronous tool support, support for stateless mode, and a few bug fixes.</p>
</div>
<div class="sect2">
<h3 id="prerequisites">Prerequisites</h3>
<div class="paragraph">
<p>To use the <code>mcpServer-1.0</code> feature, <code>Java 17</code> must be installed on the system.</p>
</div>
</div>
<div class="sect2">
<h3 id="update-on-the-mcp-jar-location">Update on the MCP JAR location</h3>
<div class="paragraph">
<p>The MCP server JAR is located in a new directory, <code>/dev/ibm/api</code>, replacing the location used in the initial beta. For complete instructions on building an MCP server in Liberty, see the <a href="https://openliberty.io/blog/2025/10/23/mcp-standalone-blog.html">set up guide</a>.</p>
</div>
</div>
<div class="sect2">
<h3 id="asynchronous-tool-execution-support">Asynchronous Tool Execution Support</h3>
<div class="paragraph">
<p>MCP tools can now run asynchronously by returning a <a href="https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/CompletionStage.html">CompletionStage</a>. If a tool needs to wait for something, such as for data to be returned from a remote system, running asynchronously allows it to wait without occupying a thread.</p>
</div>
<div class="paragraph">
<p>The tool method can return a <code>CompletionStage</code> quickly, but the response is not returned to the client until the <code>CompletionStage</code> completes and provides the response data.</p>
</div>
<div class="paragraph">
<p>The following example illustrates how this approach would look for a tool that calls an asynchronous Jakarta REST client:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="directive">private</span> Client client = ClientBuilder.newClient();

<span class="annotation">@Tool</span>
<span class="directive">public</span> CompletionStage&lt;<span class="predefined-type">String</span>&gt; toolName(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">value</span><span class="delimiter">&quot;</span></span>) <span class="type">int</span> value) {
    <span class="keyword">return</span> client.target(<span class="string"><span class="delimiter">&quot;</span><span class="content">https://example.com/api</span><span class="delimiter">&quot;</span></span>)
            .queryParam(<span class="string"><span class="delimiter">&quot;</span><span class="content">value</span><span class="delimiter">&quot;</span></span>, value)
            .request()
            .rx()
            .get(<span class="predefined-type">String</span>.class);
}</code></pre>
</div>
</div>
</div>
<div class="sect2">
<h3 id="stateless-mode">Stateless Mode</h3>
<div class="paragraph">
<p>Parts of the Model Context Protocol require the server to store client information and reuse it for subsequent requests from the same client. This stateful behavior creates challenges when clustering MCP servers. When requests from the same client are handled by different servers, the stored information must be shared across all servers.</p>
</div>
<div class="paragraph">
<p>One way to address this problem is through session affinity. The ingress or load balancer in front of the MCP servers helps ensure that all requests from a single session are served by the same server. However, this approach requires special support for how MCP identifies sessions, and such support for MCP session affinity is not widely available yet.</p>
</div>
<div class="paragraph">
<p>This beta introduces another option, called <em>stateless mode</em>. This option disables MCP features that require state to be stored, allowing MCP servers to be clustered easily without any special logic for session affinity.</p>
</div>
<div class="paragraph">
<p>Currently, enabling stateless mode disables the client’s ability to cancel tool calls. As we add new features, we document which features are disabled or have limitations in stateless mode.</p>
</div>
<div class="paragraph">
<p>To enable stateless mode in your application, you need to add the following configuration in your <code>server.xml</code> file:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;mcpServer</span> <span class="attribute-name">stateless</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">true</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>One of the main differences in stateless mode is that the server does not assign a session ID to clients. As a result, subsequent client requests do not include the <code>Mcp-Session-Id</code> header.</p>
</div>
</div>
<div class="sect2">
<h3 id="working-with-structured-content-in-mcp-tools">Working with structured content in MCP Tools</h3>
<div class="paragraph">
<p>The <a href="https://modelcontextprotocol.io/specification/2025-06-18/server/tools#structured-content">MCP specification</a> supports returning structured content from tools.</p>
</div>
<div class="sect3">
<h4 id="returning-structured-content">Returning structured content</h4>
<div class="paragraph">
<p>It is now possible to return structured JSON from a tool method. To enable this capability, set <code>structuredContent = true</code> on the <code>@Tool</code> annotation and return a POJO from your tool method:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="directive">public</span> record Street(<span class="predefined-type">String</span> streetName, <span class="predefined-type">String</span> roadType) {}

<span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">getStreet</span><span class="delimiter">&quot;</span></span>,
      description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Look up a street by name</span><span class="delimiter">&quot;</span></span>
      structuredContent = <span class="predefined-constant">true</span>)
<span class="directive">public</span> Street getStreet(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">street</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">the street name</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> streetName){
    <span class="comment">//Get street information</span>
    <span class="keyword">return</span> <span class="keyword">new</span> Street(streetName, <span class="string"><span class="delimiter">&quot;</span><span class="content">One way Street</span><span class="delimiter">&quot;</span></span>);
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>Open Liberty uses <a href="https://jakarta.ee/specifications/jsonb/">Jakarta JSON Binding (JSON-B)</a> to encode the returned <code>Street</code> object into JSON and this is returned to the MCP client in the <code>structuredContent</code> field of the response. For compatibility with earlier versions and older clients that do not support <code>structuredContent</code>, the encoded JSON is also returned as <code>TextContent</code>.
In addition, Open Liberty generates a schema for the method&#8217;s return type to indicate the expected data structure to the MCP client.</p>
</div>
</div>
<div class="sect3">
<h4 id="customizing-serialization">Customizing Serialization</h4>
<div class="paragraph">
<p>Since Open Liberty uses JSON-B to encode the returned object, you can use the standard JSON-B annotations to customize how your objects are serialized.</p>
</div>
</div>
</div>
<div class="sect2">
<h3 id="customizing-schemas-with-schema">Customizing schemas with <code>@Schema</code></h3>
<div class="paragraph">
<p>MCP protocol requires tools to declare schemas for their inputs and outputs. These schemas help clients to understand what to send and what to expect in return. Open Liberty generates these schemas automatically by inspecting your Java types - although you can customize them by using the <code>@Schema</code> annotation.
The simplest use of <code>@Schema</code> is to add descriptions to your types. These descriptions help clients understand the purpose of each type or field.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Schema</span>(description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Represents Street information</span><span class="delimiter">&quot;</span></span>)
<span class="directive">public</span> record Street(
    <span class="annotation">@Schema</span>(description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Name of the street</span><span class="delimiter">&quot;</span></span>)
    <span class="predefined-type">String</span> streetName,

    <span class="annotation">@Schema</span>(description = <span class="string"><span class="delimiter">&quot;</span><span class="content">type of the road and its rules</span><span class="delimiter">&quot;</span></span>)
    <span class="predefined-type">String</span> roadType
) {}

<span class="annotation">@Tool</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">getStreet</span><span class="delimiter">&quot;</span></span>,
      description = <span class="string"><span class="delimiter">&quot;</span><span class="content">Look up a street by name</span><span class="delimiter">&quot;</span></span>
      structuredContent = <span class="predefined-constant">true</span>)
<span class="directive">public</span> Street getStreet(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">street</span><span class="delimiter">&quot;</span></span>, description = <span class="string"><span class="delimiter">&quot;</span><span class="content">the street name</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> streetName) {
    <span class="comment">// Get street information</span>
    <span class="keyword">return</span> <span class="keyword">new</span> Street(streetName, <span class="string"><span class="delimiter">&quot;</span><span class="content">One way Street</span><span class="delimiter">&quot;</span></span>);
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>If the default schemas do not meet your needs, you can provide a complete schema override. You might need to do an override if you use JSON-B annotations to customize the serialization of the object causing the generated schema to no longer match. A schema override is performed by providing a value to the <code>@Schema</code> annotation.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Schema</span>(<span class="string"><span class="delimiter">&quot;</span><span class="delimiter">&quot;</span></span><span class="string"><span class="delimiter">&quot;</span><span class="content">
{
    </span><span class="delimiter">&quot;</span></span>type<span class="string"><span class="delimiter">&quot;</span><span class="content">: </span><span class="delimiter">&quot;</span></span>object<span class="string"><span class="delimiter">&quot;</span><span class="content">,
    </span><span class="delimiter">&quot;</span></span>required<span class="string"><span class="delimiter">&quot;</span><span class="content">: [</span><span class="delimiter">&quot;</span></span>streetName<span class="string"><span class="delimiter">&quot;</span><span class="content">],
    </span><span class="delimiter">&quot;</span></span>properties<span class="string"><span class="delimiter">&quot;</span><span class="content">: {
        </span><span class="delimiter">&quot;</span></span>streetName<span class="string"><span class="delimiter">&quot;</span><span class="content">: {
            </span><span class="delimiter">&quot;</span></span>type<span class="string"><span class="delimiter">&quot;</span><span class="content">: </span><span class="delimiter">&quot;</span></span>string<span class="string"><span class="delimiter">&quot;</span><span class="content">
        },
        </span><span class="delimiter">&quot;</span></span>roadType<span class="string"><span class="delimiter">&quot;</span><span class="content">: {
            </span><span class="delimiter">&quot;</span></span>type<span class="string"><span class="delimiter">&quot;</span><span class="content">: </span><span class="delimiter">&quot;</span></span>string<span class="string"><span class="delimiter">&quot;</span><span class="content">,
            </span><span class="delimiter">&quot;</span></span><span class="type">enum</span><span class="string"><span class="delimiter">&quot;</span><span class="content">: [
                </span><span class="delimiter">&quot;</span></span>One Way Street<span class="string"><span class="delimiter">&quot;</span><span class="content">,
                </span><span class="delimiter">&quot;</span></span>Dual Carriageway<span class="string"><span class="delimiter">&quot;</span><span class="content">,
                </span><span class="delimiter">&quot;</span></span>Motorway<span class="string"><span class="delimiter">&quot;</span><span class="content">
            ]
        }
    }
}
</span><span class="delimiter">&quot;</span></span><span class="string"><span class="delimiter">&quot;</span><span class="delimiter">&quot;</span></span>)
<span class="directive">public</span> record Street(<span class="predefined-type">String</span> streetName, <span class="predefined-type">String</span> roadType) {}</code></pre>
</div>
</div>
<div class="paragraph">
<p>You usually also need to provide an output schema for a tool method that returns a <code>ToolResponse</code>. In this case, Open Liberty does not know what type your tool is going to return, so it cannot generate a detailed schema for it.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Schema</span>(<span class="string"><span class="delimiter">&quot;</span><span class="delimiter">&quot;</span></span><span class="string"><span class="delimiter">&quot;</span><span class="content">
{
    </span><span class="delimiter">&quot;</span></span>type<span class="string"><span class="delimiter">&quot;</span><span class="content">: </span><span class="delimiter">&quot;</span></span>object<span class="string"><span class="delimiter">&quot;</span><span class="content">,
    </span><span class="delimiter">&quot;</span></span>required<span class="string"><span class="delimiter">&quot;</span><span class="content">: [</span><span class="delimiter">&quot;</span></span>streetName<span class="string"><span class="delimiter">&quot;</span><span class="content">],
    </span><span class="delimiter">&quot;</span></span>properties<span class="string"><span class="delimiter">&quot;</span><span class="content">: {
        </span><span class="delimiter">&quot;</span></span>streetName<span class="string"><span class="delimiter">&quot;</span><span class="content">: {
            </span><span class="delimiter">&quot;</span></span>type<span class="string"><span class="delimiter">&quot;</span><span class="content">: </span><span class="delimiter">&quot;</span></span>string<span class="string"><span class="delimiter">&quot;</span><span class="content">
        },
        </span><span class="delimiter">&quot;</span></span>roadType<span class="string"><span class="delimiter">&quot;</span><span class="content">: {
            </span><span class="delimiter">&quot;</span></span>type<span class="string"><span class="delimiter">&quot;</span><span class="content">: </span><span class="delimiter">&quot;</span></span>string<span class="string"><span class="delimiter">&quot;</span><span class="content">,
            </span><span class="delimiter">&quot;</span></span><span class="type">enum</span><span class="string"><span class="delimiter">&quot;</span><span class="content">: [
                </span><span class="delimiter">&quot;</span></span>One Way Street<span class="string"><span class="delimiter">&quot;</span><span class="content">,
                </span><span class="delimiter">&quot;</span></span>Dual Carriageway<span class="string"><span class="delimiter">&quot;</span><span class="content">,
                </span><span class="delimiter">&quot;</span></span>Motorway<span class="string"><span class="delimiter">&quot;</span><span class="content">
            ]
        }
    }
}
</span><span class="delimiter">&quot;</span></span><span class="string"><span class="delimiter">&quot;</span><span class="delimiter">&quot;</span></span>)
<span class="annotation">@Tool</span>(structuredContent = <span class="predefined-constant">true</span>)
<span class="directive">public</span> ToolResponse toolName() {
    Street street = yourService.getStreetInfo();
    <span class="keyword">return</span> ToolResponse.structuredSuccess(street)
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>Use <code>@Schema(description = "&#8230;&#8203;")</code> when you want to add helpful context for clients, and <code>@Schema(value = "{&#8230;&#8203;}")</code> when you have more complex types and the auto-generated schema does not reflect your actual JSON output.</p>
</div>
</div>
<div class="sect2">
<h3 id="allowing-annotations-in-mcp-content-objects">Allowing annotations in MCP Content objects</h3>
<div class="paragraph">
<p>As specified in the <a href="https://modelcontextprotocol.io/specification/2025-06-18/server/resources#annotations">MCP specification</a>, MCP annotations can be added to Content objects returned from tool methods. These annotations are not Java annotations; instead, they are additional fields that provide hints to clients about how to use and display the content.</p>
</div>
<div class="paragraph">
<p>The annotation includes the following fields:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><strong><code>audience</code> field</strong> - an enum of type <code>Role</code> that indicates the intended audience for the content.</p>
</li>
<li>
<p><strong><code>lastModified</code> field</strong> - a string containing an ISO 8601-formatted timestamp that indicates when the content object was last modified.</p>
</li>
<li>
<p><strong><code>priority</code> field</strong> - a double value between 0.0 to 1.0 that indicates the importance of the content object.</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>The following example shows how annotation fields can be added to tools.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Tool</span>
<span class="directive">public</span> TextContent toolName(<span class="annotation">@ToolArg</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">input</span><span class="delimiter">&quot;</span></span>) <span class="predefined-type">String</span> input) {
    Content.Annotations annotations = <span class="keyword">new</span> Content.Annotations(<span class="predefined-type">Role</span>.USER,
                                                              Instant.now().toString(),
                                                              <span class="float">1.0</span>);
    <span class="keyword">return</span> <span class="keyword">new</span> TextContent(<span class="string"><span class="delimiter">&quot;</span><span class="content">My Generated Text</span><span class="delimiter">&quot;</span></span>, <span class="predefined-constant">null</span>, annotations);
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>Result:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>{
    <span class="key"><span class="delimiter">&quot;</span><span class="content">id</span><span class="delimiter">&quot;</span></span>: <span class="integer">1</span>,
    <span class="key"><span class="delimiter">&quot;</span><span class="content">jsonrpc</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">2.0</span><span class="delimiter">&quot;</span></span>,
    <span class="key"><span class="delimiter">&quot;</span><span class="content">result</span><span class="delimiter">&quot;</span></span>: {
        <span class="key"><span class="delimiter">&quot;</span><span class="content">content</span><span class="delimiter">&quot;</span></span>: [
            {
                <span class="key"><span class="delimiter">&quot;</span><span class="content">annotations</span><span class="delimiter">&quot;</span></span>: {
                    <span class="key"><span class="delimiter">&quot;</span><span class="content">audience</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">user</span><span class="delimiter">&quot;</span></span>,
                    <span class="key"><span class="delimiter">&quot;</span><span class="content">lastModified</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">2025-12-11T12:31:04</span><span class="delimiter">&quot;</span></span>,
                    <span class="key"><span class="delimiter">&quot;</span><span class="content">priority</span><span class="delimiter">&quot;</span></span>: <span class="float">1.0</span>
                },
                <span class="key"><span class="delimiter">&quot;</span><span class="content">text</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">My Generated Text</span><span class="delimiter">&quot;</span></span>,
                <span class="key"><span class="delimiter">&quot;</span><span class="content">type</span><span class="delimiter">&quot;</span></span>: <span class="string"><span class="delimiter">&quot;</span><span class="content">text</span><span class="delimiter">&quot;</span></span>
            }
        ],
        <span class="key"><span class="delimiter">&quot;</span><span class="content">isError</span><span class="delimiter">&quot;</span></span>: <span class="value">false</span>
    }
}</code></pre>
</div>
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
    <span class="tag">&lt;version&gt;</span>3.11.5<span class="tag">&lt;/version&gt;</span>
    <span class="tag">&lt;configuration&gt;</span>
        <span class="tag">&lt;runtimeArtifact&gt;</span>
          <span class="tag">&lt;groupId&gt;</span>io.openliberty.beta<span class="tag">&lt;/groupId&gt;</span>
          <span class="tag">&lt;artifactId&gt;</span>openliberty-runtime<span class="tag">&lt;/artifactId&gt;</span>
          <span class="tag">&lt;version&gt;</span>26.0.0.1-beta<span class="tag">&lt;/version&gt;</span>
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
        classpath 'io.openliberty.tools:liberty-gradle-plugin:3.9.6'
    }
}
apply plugin: 'liberty'
dependencies {
    libertyRuntime group: 'io.openliberty.beta', name: 'openliberty-runtime', version: '[26.0.0.1-beta,)'
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
<p>For more information on using a beta release, refer to the <a href="https://openliberty.io/blog/2026/01/13/docs/latest/installing-open-liberty-betas.html">Installing Open Liberty beta releases</a> documentation.</p>
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
