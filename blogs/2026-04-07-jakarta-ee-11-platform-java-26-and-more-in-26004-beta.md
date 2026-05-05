---
title: "Jakarta EE 11 Platform, Java 26, and more in 26.0.0.4-beta"
url: "https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html"
date: "2026-04-07T00:00:00+00:00"
author: "Navaneeth S Nair"
feed_url: "https://openliberty.io/feed.xml"
---
<div id="preamble">
<div class="sectionbody">
<div class="paragraph">
<p>This beta introduces Jakarta EE 11 Platform and Web Profile, and delivers enhancements across Jakarta Authentication 3.1, Jakarta Authorization 3.0, and Jakarta Security 4.0. In addition, it provides a preview of Jakarta Data 1.1 M2, support for Java 26, MCP Server updates, and Jandex index improvements.</p>
</div>
<div class="paragraph">
<p>The <a href="https://openliberty.io/">Open Liberty</a> 26.0.0.4-beta includes the following beta features (along with <a href="https://openliberty.io/docs/latest/reference/feature/feature-overview.html">all GA features</a>):</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#jakarta_ee">Jakarta EE 11 Platform and Web Profile</a></p>
<div class="ulist">
<ul>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#jakarta_auth">Application Authentication 3.1 (Jakarta Authentication 3.1)</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#jakarta_authz">Application Authorization 3.0 (Jakarta Authorization 3.0)</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#jakarta_security">Application Security 6.0 (Jakarta Security 4.0)</a></p>
</li>
</ul>
</div>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#java_26">Beta support for Java 26</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#mcp">Updates to <code>mcpServer-1.0</code></a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#jakarta_data">Preview of some Jakarta Data 1.1 M2 capability</a></p>
</li>
<li>
<p><a href="https://openliberty.io/blog/2026/04/07/26.0.0.4-beta.html#jandex_index">Support for Reading Jandex Indexes from WEB-INF/classes in Web Modules</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>See also <a href="https://openliberty.io/blog/?search=beta&amp;key=tag">previous Open Liberty beta blog posts</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jakarta_ee">Jakarta EE 11 Platform and Web Profile</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Open Liberty 24.0.0.11-beta was one of the ratifying implementations for Jakarta EE 11 Core Profile. Building on that foundation, the 26.0.0.4-beta release completes Jakarta EE 11 beta support, including Jakarta EE Application Client 11.0, Jakarta EE Web Profile 11.0, and Jakarta EE Platform 11.0.</p>
</div>
<div class="paragraph">
<p>Liberty provides convenience features that bundle all component specifications in the Jakarta EE Web Profile and Jakarta EE Platform. The <code>webProfile-11.0</code> Liberty feature includes all Jakarta EE Web Profile functions, including the new Jakarta Data 1.0 component. The <code>jakartaee-11.0</code> Liberty feature provides the Jakarta EE Platform version 11 implementation. For Jakarta EE 11 features in the application client, use the <code>jakartaeeClient-11.0</code> Liberty feature.</p>
</div>
<div class="paragraph">
<p>These convenience features enable developers to rapidly develop applications using all APIs in the Web Profile and Platform specifications. Unlike the <code>jakartaee-10.0</code> Liberty feature, the <code>jakartaee-11.0</code> Liberty feature does not enable the Managed Beans specification function any longer, as this specification was removed from the platform which includes removal of the <code>jakarta.annotation.ManagedBean</code> annotation API. Applications relying on the <code>@ManagedBean</code> annotation must migrate to use CDI annotations.</p>
</div>
<div class="paragraph">
<p>The Jakarta EE 11 Platform specification removes all optional specifications from the platform, meaning Jakarta SOAP with Attachments, XML Binding, XML Web Services, and Enterprise Beans 2.x APIs functions are not included with the <code>jakartaee-11.0</code> Liberty feature. To use these capabilities, explicitly add Liberty features to your <code>server.xml</code> feature list: <code>xmlBinding-4.0</code> for XML Binding, <code>xmlWS-4.0</code> for SOAP with Attachments and XML Web Services, and <code>enterpriseBeansHome-4.0</code> for Jakarta Enterprise Beans 2.x APIs. Alternatively, use the equivalent versionless features with the <code>jakartaee-11.0</code> platform.</p>
</div>
<div class="paragraph">
<p>When using the Liberty application client with the <code>jakartaeeClient-11.0</code> feature, Jakarta SOAP with Attachments, XML Binding, and XML Web Services client functions are not available. To continue using these functions in your Liberty application client, enable the <code>xmlBinding-4.0</code> and the new <code>xmlWSClient-4.0</code> features in your <code>client.xml</code> file.</p>
</div>
<div class="paragraph">
<p>To enable the Jakarta EE 11 beta features in your Liberty server&#8217;s <code>server.xml</code>:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>  <span class="tag">&lt;featureManager&gt;</span>
    <span class="tag">&lt;feature&gt;</span>jakartaee-11.0<span class="tag">&lt;/feature&gt;</span>
  <span class="tag">&lt;/featureManager&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>Or you can add the Web Profile convenience feature to enable all of the Jakarta EE 11 Web Profile beta features at once:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>  <span class="tag">&lt;featureManager&gt;</span>
    <span class="tag">&lt;feature&gt;</span>webProfile-11.0<span class="tag">&lt;/feature&gt;</span>
  <span class="tag">&lt;/featureManager&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>You can enable the Jakarta EE 11 features on the Application Client Container in the <code>client.xml</code>:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code> <span class="tag">&lt;featureManager&gt;</span>
       <span class="tag">&lt;feature&gt;</span>jakartaeeClient-11.0<span class="tag">&lt;/feature&gt;</span>
 <span class="tag">&lt;/featureManager&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>For more information, see the <a href="https://jakarta.ee/specifications/platform/11/jakarta-platform-spec-11.0">Jakarta EE Platform 11 Specification</a> and the <a href="https://jakarta.ee/specifications/webprofile/11/jakarta-webprofile-spec-11.0">Jakarta EE Web Profile 11 Specification</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jakarta_auth">Application Authentication 3.1 (Jakarta Authentication 3.1)</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Jakarta Authentication defines a general SPI for authentication mechanisms, which are controllers that interact with a caller and the container&#8217;s environment to obtain and validate the caller&#8217;s credentials. It then passes an authenticated identity (such as name and groups) to the container.</p>
</div>
<div class="paragraph">
<p>The 3.1 version of the Jakarta Authentication API removes the deprecated Permission-related fields in the <code>jakarta.security.auth.message.config.AuthConfigFactory</code> class. The methods in the class no longer do permission checking also. These changes are part of Jakarta EE 11&#8217;s removal of SecurityManager support.</p>
</div>
<div class="paragraph">
<p>You can enable the Jakarta Authentication 3.1 feature by adding the <code>appAuthentication-3.1</code> Liberty feature in the <code>server.xml</code> file:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>    <span class="tag">&lt;featureManager&gt;</span>
        <span class="tag">&lt;feature&gt;</span>appAuthentication-3.1<span class="tag">&lt;/feature&gt;</span>
    <span class="tag">&lt;/featureManager&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>For more information, see the Jakarta Authentication 3.1 <a href="https://jakarta.ee/specifications/authentication/3.1/jakarta-authentication-spec-3.1">specification</a> and <a href="https://jakarta.ee/specifications/authentication/3.1/apidocs/jakarta.security.auth.message/module-summary.html">javadoc</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jakarta_authz">Application Authorization 3.0 (Jakarta Authorization 3.0)</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Jakarta Authorization defines an SPI for authorization modules, which are repositories of permissions that facilitate subject-based security by determining whether a subject has a specific permission. It also defines algorithms that transform security constraints for specific containers, such as Jakarta Servlet or Jakarta Enterprise Beans, into these permissions.</p>
</div>
<div class="paragraph">
<p>The 3.0 API introduces the new <code>jakarta.security.jacc.PolicyFactory</code> and <code>jakarta.security.jacc.Policy</code> classes for doing authorization decisions. These classes are added to remove the dependency on the <code>java.security.Policy</code> class, which is deprecated in newer versions of Java. With the new <code>PolicyFactory</code> API, now you can have a <code>Policy</code> per policy context instead of having a global policy. This design allows separate policies to be maintained for each application.</p>
</div>
<div class="paragraph">
<p>Additionally, the 3.0 specification defines a mechanism to define <code>PolicyConfigurationFactory</code> and <code>PolicyFactory</code> classes in your application by using a <code>web.xml</code> file. This design allows for an application to have a different configuration than the server-scoped one, but still allow for it to delegate to a server scoped factory for any other applications. Authorization modules can do this delegation by using decorator constructors for both <code>PolicyConfigurationFactory</code> and <code>PolicyFactory</code> classes.</p>
</div>
<div class="paragraph">
<p>To configure your authorization modules in your application&#8217;s <code>web.xml</code> file, add specification defined context parameters:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="preprocessor">&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;</span>
<span class="tag">&lt;web-app</span> <span class="attribute-name">xmlns</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">https://jakarta.ee/xml/ns/jakartaee</span><span class="delimiter">&quot;</span></span>
  <span class="attribute-name">xmlns:xsi</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">http://www.w3.org/2001/XMLSchema-instance</span><span class="delimiter">&quot;</span></span>
  <span class="attribute-name">xsi:schemaLocation</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_1.xsd</span><span class="delimiter">&quot;</span></span>
  <span class="attribute-name">version</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">6.1</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>

  <span class="tag">&lt;context-param&gt;</span>
    <span class="tag">&lt;param-name&gt;</span>jakarta.security.jacc.PolicyConfigurationFactory.provider<span class="tag">&lt;/param-name&gt;</span>
    <span class="tag">&lt;param-value&gt;</span>com.example.MyPolicyConfigurationFactory<span class="tag">&lt;/param-value&gt;</span>
  <span class="tag">&lt;/context-param&gt;</span>

  <span class="tag">&lt;context-param&gt;</span>
    <span class="tag">&lt;param-name&gt;</span>jakarta.security.jacc.PolicyFactory.provider<span class="tag">&lt;/param-name&gt;</span>
    <span class="tag">&lt;param-value&gt;</span>com.example.MyPolicyFactory<span class="tag">&lt;/param-value&gt;</span>
  <span class="tag">&lt;/context-param&gt;</span>

<span class="tag">&lt;/web-app&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>Due to Jakarta Authorization 3.0 no longer using the <code>java.security.Policy</code> class and introducing a new configuration mechanism for authorization modules, the <code>com.ibm.wsspi.security.authorization.jacc.ProviderService</code> Liberty API is no longer available with the appAuthorization-3.0 feature. If a Liberty user feature configures authorization modules, the OSGi service that provided a <code>ProviderService</code> implementation must be updated to use the <code>PolicyConfigurationFactory</code> and <code>PolicyFactory</code> <code>set</code> methods. These methods configure the modules in the OSGi service. Alternatively you can use a Web Application Bundle (WAB) in your user feature to specify your security modules in a <code>web.xml</code> file.</p>
</div>
<div class="paragraph">
<p>Finally, the 3.0 API adds a new <code>jakarta.security.jacc.PrincipalMapper</code> class that you can obtain from the <code>PolicyContext</code> class when authorization processing is done in your <code>Policy</code> implementation. From this class, you can obtain the roles that are associated with a specific Subject to be able to determine whether the Subject is in the required role.</p>
</div>
<div class="paragraph">
<p>You can use the <code>PrincipalMapper</code> class in your <code>Policy</code> implementation&#8217;s <code>impliesByRole</code> (or <code>implies</code>) method, as shown in the following example:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>    <span class="directive">public</span> <span class="type">boolean</span> impliesByRole(<span class="predefined-type">Permission</span> p, <span class="predefined-type">Subject</span> subject) {
        <span class="predefined-type">Map</span>&lt;<span class="predefined-type">String</span>, <span class="predefined-type">PermissionCollection</span>&gt; perRolePermissions =
            PolicyConfigurationFactory.get().getPolicyConfiguration(contextID).getPerRolePermissions();
        PrincipalMapper principalMapper = PolicyContext.get(PolicyContext.PRINCIPAL_MAPPER);

        <span class="comment">// Check to see if the Permission is in the all authenticated users role (**)</span>
        <span class="keyword">if</span> (!principalMapper.isAnyAuthenticatedUserRoleMapped() &amp;&amp; !subject.getPrincipals().isEmpty()) {
            <span class="predefined-type">PermissionCollection</span> rolePermissions = perRolePermissions.get(<span class="string"><span class="delimiter">&quot;</span><span class="content">**</span><span class="delimiter">&quot;</span></span>);
            <span class="keyword">if</span> (rolePermissions != <span class="predefined-constant">null</span> &amp;&amp; rolePermissions.implies(p)) {
                <span class="keyword">return</span> <span class="predefined-constant">true</span>;
            }
        }

        <span class="comment">// Check to see if the roles for the Subject provided imply the permission</span>
        <span class="predefined-type">Set</span>&lt;<span class="predefined-type">String</span>&gt; mappedRoles = principalMapper.getMappedRoles(subject);
        <span class="keyword">for</span> (<span class="predefined-type">String</span> mappedRole : mappedRoles) {
            <span class="predefined-type">PermissionCollection</span> rolePermissions = perRolePermissions.get(mappedRole);
            <span class="keyword">if</span> (rolePermissions != <span class="predefined-constant">null</span> &amp;&amp; rolePermissions.implies(p)) {
                <span class="keyword">return</span> <span class="predefined-constant">true</span>;
            }
        }
        <span class="keyword">return</span> <span class="predefined-constant">false</span>;
    }</code></pre>
</div>
</div>
<div class="paragraph">
<p>You can enable the Jakarta Authorization 3.0 feature by adding the <code>appAuthorization-3.0</code> Liberty feature in the <code>server.xml</code> file:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>    <span class="tag">&lt;featureManager&gt;</span>
        <span class="tag">&lt;feature&gt;</span>appAuthorization-3.0<span class="tag">&lt;/feature&gt;</span>
    <span class="tag">&lt;/featureManager&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>For more information, see the Jakarta Authorization 3.0 <a href="https://jakarta.ee/specifications/authorization/3.0/jakarta-authorization-spec-3.0">specification</a> and <a href="https://jakarta.ee/specifications/authorization/3.0/apidocs/jakarta.security.jacc/module-summary.html">javadoc</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jakarta_security">Application Security 6.0 (Jakarta Security 4.0)</h2>
<div class="sectionbody">
<div class="paragraph">
<p>An In-memory Identity Store is a developer-defined store of credential information that is used during the Open Liberty authentication and authorization work flow. It provides a quick, simple, and convenient authentication mechanism for Liberty application testing, debugging, demos, and more.</p>
</div>
<div class="ulist">
<ul>
<li>
<p>Multiple HTTP Authentication Mechanisms (HAMs) can now be defined within the same application. These mechanisms can be specified through built-in Jakarta annotations such as <code>@FormAuthenticationMechanismDefinition</code> or through custom implementations of the <code>HttpAuthenticationMechanism</code> interface.</p>
</li>
<li>
<p>A new method is added to the <code>SecurityContext</code> interface called <code>getAllDeclaredCallerRoles()</code>, which returns a list of all static (declared) application roles that the authenticated caller is in.</p>
</li>
<li>
<p>References to the <code>IdentityStorePermission</code> class are removed as it was previously deprecated.</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>All features are available as part of <code>appSecurity-6.0</code>.</p>
</div>
<div class="paragraph">
<p>All feature use cases are targeted towards Jakarta EE web application developers who want to move to a modern Jakarta version and use implementations of the Jakarta Security 4.0 specification.</p>
</div>
<div class="sect2">
<h3 id="in-memory-identity-store">In-memory identity store</h3>
<div class="paragraph">
<p>Before the introduction of the new identity store specification, Jakarta Security natively supported only two types of identity stores: <strong>database</strong> and <strong>LDAP</strong>, both of which are used for credential validation. While effective for production environments, these options were considered heavyweight for testing, debugging, and demonstration scenarios.</p>
</div>
<div class="paragraph">
<p>Developers can also implement custom identity stores, but doing so requires bespoke code to collect, validate, and manage credential information. This effort can divert attention from core application development. A built-in, in-memory identity store reduces this burden for non-production use cases such as testing, debugging, and demonstrations.</p>
</div>
</div>
<div class="sect2">
<h3 id="multiple-hams">Multiple HAMs</h3>
<div class="paragraph">
<p>It is now possible for multiple authentication mechanisms to logically act as a single HTTP Authentication Mechanism (HAM), providing a more flexible, dynamic, and configurable approach to the authentication workflow.</p>
</div>
<div class="paragraph">
<p>Access to the <code>HttpAuthenticationMechanism</code> is now abstracted by an internal implementation of the new Jakarta <code>HttpAuthenticationMechanismHandler</code> interface. This implementation prioritizes custom‑defined HAMs first and then falls back to the built‑in (annotation‑defined) HAMs, in the following order: OpenID, custom form, form, and basic authentication mechanisms.</p>
</div>
<div class="paragraph">
<p>Developers are free to provide their own implementation of the <code>HttpAuthenticationMechanismHandler</code>, allowing them to define a custom prioritization strategy for selecting HAMs. They <strong>must</strong> supply such an implementation if any built‑in HAMs are defined with the optional <code>qualifiers</code> attribute set.</p>
</div>
</div>
<div class="sect2">
<h3 id="getalldeclaredcallerroles">getAllDeclaredCallerRoles()</h3>
<div class="paragraph">
<p>The <code>SecurityContext</code> interface implementation is updated to include a new method, <code>getAllDeclaredCallerRoles()</code>, which returns a list of the declared application roles for an authenticated caller. If the caller is not authenticated or has no declared roles, the method returns an empty list.</p>
</div>
</div>
<div class="sect2">
<h3 id="how-to-use">How to use</h3>
<div class="paragraph">
<p>Enable Jakarta Security 4.0 features within the <code>server.xml</code> file by adding the <code>appSecurity-6.0</code> feature as shown in the following example:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;server</span> <span class="attribute-name">description</span> = <span class="string"><span class="delimiter">&quot;</span><span class="content">Liberty server</span><span class="delimiter">&quot;</span></span><span class="tag">&gt;</span>

    . . .

    <span class="tag">&lt;featureManager&gt;</span>
        <span class="tag">&lt;feature&gt;</span>appSecurity-6.0<span class="tag">&lt;/feature&gt;</span>
    <span class="tag">&lt;/featureManager&gt;</span>

    <span class="tag">&lt;webAppSecurity</span> <span class="attribute-name">allowInMemoryIdentityStores</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">true</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span>

    . . .</code></pre>
</div>
</div>
<div class="admonitionblock note">
<table>
<tr>
<td class="icon">
<i class="fa icon-note" title="Note"></i>
</td>
<td class="content">
As shown in the preceding example, when an application defines an in‑memory identity store in code, its use must also be explicitly enabled in the <code>server.xml</code> configuration. By default, the <code>allowInMemoryIdentityStores</code> attribute is set to false, which instructs the Liberty authentication workflows not to use in‑memory identity stores, even when a custom identity store handler is present. For applications that rely on multiple HAMs, the <code>allowInMemoryIdentityStores</code> attribute does not need to be set.
</td>
</tr>
</table>
</div>
<div class="sect3">
<h4 id="in-memory-identity-store-2">In-Memory Identity Store</h4>
<div class="paragraph">
<p>The <a href="https://jakarta.ee/specifications/security/4.0/jakarta-security-spec-4.0#in-memory-annotation">Jakarta Security Specification 4.0</a> provides details on how to specify credential information to be used during the authentication workflow through the new <code>@InMemoryIdentityStoreDefinition</code> annotation, as shown in the following example:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>. . .

<span class="annotation">@InMemoryIdentityStoreDefinition</span> (
    priority = <span class="integer">10</span>,
    priorityExpression = <span class="string"><span class="delimiter">&quot;</span><span class="content">${80/20}</span><span class="delimiter">&quot;</span></span>,
    useFor = {VALIDATE, PROVIDE_GROUPS},
    useForExpression = <span class="string"><span class="delimiter">&quot;</span><span class="content">#{'VALIDATE'}</span><span class="delimiter">&quot;</span></span>,
    value = {
        <span class="annotation">@Credentials</span>(callerName = <span class="string"><span class="delimiter">&quot;</span><span class="content">jasmine</span><span class="delimiter">&quot;</span></span>, password = <span class="string"><span class="delimiter">&quot;</span><span class="content">secret1</span><span class="delimiter">&quot;</span></span>, groups = { <span class="string"><span class="delimiter">&quot;</span><span class="content">caller</span><span class="delimiter">&quot;</span></span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">user</span><span class="delimiter">&quot;</span></span> } )
    }
)</code></pre>
</div>
</div>
<div class="paragraph">
<p>All attributes for the <code>@InMemoryIdentityStoreDefinition</code> annotation are shown in the example. The <code>priority</code>, <code>priorityExpression</code>, <code>useFor</code>, and <code>useForExpression</code> attributes are optional and set to sensible defaults.</p>
</div>
<div class="paragraph">
<p>The <code>@Credentials</code> annotation maps one or more caller names to a password and optional group values. The <code>callerName</code> and <code>password</code> attributes are mandatory. If either one is omitted, a compilation error occurs.</p>
</div>
<div class="paragraph">
<p>The example demonstrates a single caller definition with credential information that uses a plain-text password. However, it is highly recommended that passwords be supplied that uses an Open Liberty–supported encoding mechanism, as illustrated in the next example.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@InMemoryIdentityStoreDefinition</span> (
    value = {
        <span class="annotation">@Credentials</span>(callerName = <span class="string"><span class="delimiter">&quot;</span><span class="content">jasmine</span><span class="delimiter">&quot;</span></span>,  password = <span class="string"><span class="delimiter">&quot;</span><span class="content">{xor}LDo8LTorbg==</span><span class="delimiter">&quot;</span></span>, groups = { <span class="string"><span class="delimiter">&quot;</span><span class="content">caller</span><span class="delimiter">&quot;</span></span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">user</span><span class="delimiter">&quot;</span></span> } ),
        <span class="annotation">@Credentials</span>(callerName = <span class="string"><span class="delimiter">&quot;</span><span class="content">frank</span><span class="delimiter">&quot;</span></span>, groups = { <span class="string"><span class="delimiter">&quot;</span><span class="content">user</span><span class="delimiter">&quot;</span></span> }, password = <span class="string"><span class="delimiter">&quot;</span><span class="content">{hash}ARAAA &lt;sequence shortened&gt; Fyyw==</span><span class="delimiter">&quot;</span></span>),
        <span class="annotation">@Credentials</span>(callerName = <span class="string"><span class="delimiter">&quot;</span><span class="content">sally</span><span class="delimiter">&quot;</span></span>, groups = { <span class="string"><span class="delimiter">&quot;</span><span class="content">user</span><span class="delimiter">&quot;</span></span> }, password = <span class="string"><span class="delimiter">&quot;</span><span class="content">{aes}ARAFIYJ &lt;sequence shortened&gt; WRQNA==</span><span class="delimiter">&quot;</span></span>)
    }
)</code></pre>
</div>
</div>
<div class="paragraph">
<p>Encrypted and encoded passwords can be generated by using the Open Liberty <code>securityUtility</code>, which is included under the <code>wlp/bin/securityUtility</code> path. The following example demonstrates how to encode a text string by using the <code>xor</code> encoding mechanism.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>wlp/bin/securityUtility encode --encoding=xor
Enter text: &lt;enter text to encode&gt;
Re-enter text:
{xor}PTA9Lyg</code></pre>
</div>
</div>
</div>
<div class="sect3">
<h4 id="multiple-hams-2">Multiple HAMs</h4>
<div class="sect4">
<h5 id="application-specification">Application Specification</h5>
<div class="paragraph">
<p>The <a href="https://jakarta.ee/specifications/security/4.0/jakarta-security-spec-4.0#handling-multiple-authentication-mechanisms">Jakarta Security 4.0</a> specification allows multiple HTTP Authentication Mechanisms (HAMs) to be defined within a single application, as shown in the following example:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@BasicAuthenticationMechanismDefinition</span>(realmName=<span class="string"><span class="delimiter">&quot;</span><span class="content">basicAuth</span><span class="delimiter">&quot;</span></span>)

<span class="annotation">@FormAuthenticationMechanismDefinition</span>(
                loginToContinue = <span class="annotation">@LoginToContinue</span>(errorPage = <span class="string"><span class="delimiter">&quot;</span><span class="content">/form-login-error.html</span><span class="delimiter">&quot;</span></span>,
                loginPage = <span class="string"><span class="delimiter">&quot;</span><span class="content">/form-login.html</span><span class="delimiter">&quot;</span></span>))

<span class="annotation">@CustomFormAuthenticationMechanismDefinition</span>(
                loginToContinue = <span class="annotation">@LoginToContinue</span>(errorPage = <span class="string"><span class="delimiter">&quot;</span><span class="content">/custom-login-error.html</span><span class="delimiter">&quot;</span></span>,
                loginPage = <span class="string"><span class="delimiter">&quot;</span><span class="content">/custom-login.html</span><span class="delimiter">&quot;</span></span>))</code></pre>
</div>
</div>
<div class="paragraph">
<p>This example demonstrates how three HTTP Authentication Mechanisms (HAMs) can be defined within a single application.</p>
</div>
<div class="paragraph">
<p>Custom HAMs can also be defined in the same application by implementing the <code>HttpAuthenticationMechanism</code> interface in one or more classes, as shown in the following example:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@ApplicationScoped</span>
<span class="comment">// @Priority is optional and used to control selection priority if multiple custom definitions exist</span>
<span class="annotation">@Priority</span>(<span class="integer">100</span>)
<span class="directive">public</span> <span class="type">class</span> <span class="class">CustomHAM</span> <span class="directive">implements</span> HttpAuthenticationMechanism {

    <span class="annotation">@Override</span>
    <span class="directive">public</span> AuthenticationStatus validateRequest(
        HttpServletRequest request,
        HttpServletResponse response,
        HttpMessageContext httpMessageContext) <span class="directive">throws</span> <span class="exception">AuthenticationException</span> {

            <span class="comment">// implement custom logic here, and return an AuthenticationStatus</span>
            <span class="keyword">return</span> AuthenticationStatus.NOT_DONE;
    }
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>So a single application can have a mix of both annotation-defined HAMs and custom ones. In the previous two snippets of code, a total of four HAMs are defined (three by annotation and one custom one).</p>
</div>
<div class="admonitionblock important">
<table>
<tr>
<td class="icon">
<i class="fa icon-important" title="Important"></i>
</td>
<td class="content">
<code>@Priority</code> must be used to raise or lower the priority of one custom HAM over another. If not specified, then a default priority is assigned. If more than one custom HAM is defined, their priorities need to be explicitly set to unique values. If the priorities are set to the same value or remain unset and inherit the same default value, an error occurs.
</td>
</tr>
</table>
</div>
<hr />
</div>
<div class="sect4">
<h5 id="ham-resolution">HAM resolution</h5>
<div class="paragraph">
<p>An internal implementation of the Jakarta Security 4.0 <code>HttpAuthenticationMechanismHandler</code> interface (the "internal HAM handler") is provided. When an application defines multiple HAMs, this internal handler selects a single HAM to be used in the authentication flow.</p>
</div>
<div class="paragraph">
<p>The order in which HAMs are considered (when present) is as follows:</p>
</div>
<div class="olist arabic">
<ol class="arabic">
<li>
<p>Custom (developer‑provided) HAMs</p>
<div class="ulist">
<ul>
<li>
<p>If multiple custom HAMs are defined, their relative order is resolved by using <code>@Priority</code>.</p>
</li>
</ul>
</div>
</li>
<li>
<p><code>OpenIdAuthenticationMechanismDefinition</code></p>
</li>
<li>
<p><code>CustomFormAuthenticationMechanismDefinition</code></p>
</li>
<li>
<p><code>FormAuthenticationMechanismDefinition</code></p>
</li>
<li>
<p><code>BasicAuthenticationMechanismDefinition</code></p>
</li>
</ol>
</div>
<div class="paragraph">
<p>Given this ordering, the Custom HAM is always selected in the authentication workflow if all five HAM types are defined in the application.</p>
</div>
<div class="admonitionblock important">
<table>
<tr>
<td class="icon">
<i class="fa icon-important" title="Important"></i>
</td>
<td class="content">
A developer must provide a custom implementation of the <code>HttpAuthenticationMechanismHandler</code> interface (a "custom HAM handler") if the internal HAM handler does not meet their requirements. A custom handler always takes precedence over the internal HAM handler, allowing any tailored algorithm to select a single HAM from multiple available mechanisms. Additional information about creating and using a custom HAM handler is provided in a later section.
</td>
</tr>
</table>
</div>
<hr />
</div>
<div class="sect4">
<h5 id="qualifiers">Qualifiers</h5>
<div class="paragraph">
<p>HAMs - whether defined through annotations or as custom defined - can also include an optional class‑level qualifier to simplify HAM injection into a custom HAM handler. For example, if you want to define qualified HAMs, you would first declare qualifier interfaces such as:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Qualifier</span>
<span class="annotation">@Retention</span>(RUNTIME)
<span class="annotation">@Target</span>({TYPE, METHOD, FIELD, PARAMETER})
<span class="directive">public</span> <span class="annotation">@interface</span> Admin {
}</code></pre>
</div>
</div>
<div class="paragraph">
<p><em>(Not shown is the qualifier definitions for <code>User</code>, <code>Fallback</code> which follow an identical pattern, and are used below).</em></p>
</div>
<div class="paragraph">
<p>Then, import and use the qualifiers in your in-built or custom HAM specifications, such as:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@CustomFormAuthenticationMechanismDefinition</span>(&lt;existing details not shown&gt;, qualifiers={Admin.class})

<span class="annotation">@BasicAuthenticationMechanismDefinition</span>(&lt;existing details not shown&gt;, qualifiers={User.class})</code></pre>
</div>
</div>
<div class="paragraph">
<p>and</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@ApplicationScoped</span>
<span class="annotation">@Fallback</span>   <span class="comment">// add custom qualifier to be used during injection</span>
<span class="directive">public</span> <span class="type">class</span> <span class="class">CustomHAM</span> <span class="directive">implements</span> HttpAuthenticationMechanism {

    <span class="annotation">@Override</span>
    <span class="directive">public</span> AuthenticationStatus validateRequest (. . .)  {
         . . .
    }
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>The three HAMs defined in the example are available for injection into a custom HAM handler based on their qualifier names. This scenario represents the typical use case for applications that define multiple HAMs.</p>
</div>
<div class="admonitionblock important">
<table>
<tr>
<td class="icon">
<i class="fa icon-important" title="Important"></i>
</td>
<td class="content">
If qualifiers are specified in any of the built-in HAM definitions, a custom HAM handler must be provided; otherwise, an error is raised. This requirement comes directly from the Jakarta Security specification. Details about creating and using a custom HAM handler are explained in the next section.
</td>
</tr>
</table>
</div>
<hr />
<div class="paragraph">
<p>To implement a custom HAM handler, define a public class that is annotated with <code>@ApplicationScoped</code> that implements the <code>HttpAuthenticationMechanismHandler</code> interface. Also, inject the qualified HAMs by using standard CDI syntax, as shown in the following section:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Default</span>
<span class="annotation">@ApplicationScoped</span>
<span class="directive">public</span> <span class="type">class</span> <span class="class">CustomHAMHandler</span> <span class="directive">implements</span> HttpAuthenticationMechanismHandler {

    <span class="annotation">@Inject</span> <span class="annotation">@Admin</span> <span class="comment">// this will be the FormHAM</span>
    <span class="directive">private</span> HttpAuthenticationMechanism adminHAM;

    <span class="annotation">@Inject</span> <span class="annotation">@User</span> <span class="comment">// this will be the BasicHAM</span>
    <span class="directive">private</span> HttpAuthenticationMechanism userHAM;

    <span class="annotation">@Inject</span> <span class="annotation">@Fallback</span> <span class="comment">// this will be the Custom HAM</span>
    <span class="directive">private</span> HttpAuthenticationMechanism fallbackHAM;

    <span class="directive">public</span> AuthenticationStatus validateRequest(
        HttpServletRequest request,
        HttpServletResponse response,
        HttpMessageContext context
    ) <span class="directive">throws</span> <span class="exception">AuthenticationException</span> {

        <span class="predefined-type">String</span> path = request.getRequestURI();

        <span class="comment">// route to appropriate mechanism based on path, default to my-realm</span>
        <span class="keyword">if</span> (path.startsWith(<span class="string"><span class="delimiter">&quot;</span><span class="content">/admin</span><span class="delimiter">&quot;</span></span>)) { <span class="comment">// FormHAM</span>
            <span class="keyword">return</span> adminHAM.validateRequest(request, response, context);
        } <span class="keyword">else</span> <span class="keyword">if</span> (path.startsWith(<span class="string"><span class="delimiter">&quot;</span><span class="content">/user</span><span class="delimiter">&quot;</span></span>)) { <span class="comment">// BasicHAM</span>
            <span class="keyword">return</span> userHAM.validateRequest(request, response, context);
        } <span class="keyword">else</span> { <span class="comment">// Custom HAM</span>
            <span class="keyword">return</span> fallbackHAM.validateRequest(request, response, context);
    }
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>This custom HAM handler takes priority over the internal HAM handler, allowing a different prioritisation algorithm to be implemented.</p>
</div>
</div>
</div>
<div class="sect3">
<h4 id="getalldeclaredcallerroles-2">getAllDeclaredCallerRoles()</h4>
<div class="paragraph">
<p>To use the new <code>SecurityContext</code> method, inject the <code>SecurityContext</code> implementation into your application and call the method directly, as shown in the following section:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>    <span class="annotation">@Inject</span>
    <span class="directive">private</span> SecurityContext securityContext;

. . .

    Set&lt;<span class="predefined-type">String</span>&gt; allDeclaredCallerRoles = securityContext.getAllDeclaredCallerRoles();

    <span class="predefined-type">System</span>.out.println(<span class="string"><span class="delimiter">&quot;</span><span class="content">All declared caller roles for caller [</span><span class="delimiter">&quot;</span></span>
        + securityContext.getCallerPrincipal().getName()
        + <span class="string"><span class="delimiter">&quot;</span><span class="content">] are </span><span class="delimiter">&quot;</span></span>
        + allDeclaredCallerRoles.toString());</code></pre>
</div>
</div>
</div>
</div>
<div class="sect2">
<h3 id="learn-more">Learn more</h3>
<div class="paragraph">
<p>Further information can be found in the Jakarta Security Specification 4.0:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><a href="https://jakarta.ee/specifications/security/4.0/jakarta-security-spec-4.0#in-memory-annotation">in-memory identity store</a></p>
</li>
<li>
<p><a href="https://jakarta.ee/specifications/security/4.0/jakarta-security-spec-4.0#handling-multiple-authentication-mechanisms">multiple-hams</a></p>
</li>
<li>
<p><a href="https://jakarta.ee/specifications/security/4.0/jakarta-security-spec-4.0#retrieving-and-testing-for-caller-data">getAllDeclaredCallerRoles()</a></p>
</li>
</ul>
</div>
<div class="paragraph">
<p>For more information about the <code>securityUtility</code> command, see the <a href="https://www.ibm.com/docs/en/was-liberty/base?topic=applications-securityutility-command">WAS Liberty base topic</a>.</p>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="java_26">Beta support for Java 26</h2>
<div class="sectionbody">
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
<p>Get started today by downloading the latest release of  <a href="https://developer.ibm.com/languages/java/semeru-runtimes/downloads/">IBM Semeru Runtime 26</a> or <a href="https://adoptium.net/temurin/releases/?version=26">Temurin 26</a>, then download and install the Open Liberty <a href="https://openliberty.io/downloads/#runtime_betas">26.0.0.4-beta</a>. Update your Liberty server&#8217;s <a href="https://openliberty.io/docs/latest/reference/config/server-configuration-overview.html#server-env">server.env</a> file with <code>JAVA_HOME</code> set to your Java 26 installation directory and start testing.</p>
</div>
<div class="paragraph">
<p>For more information on Java 26, see the Java 26 <a href="https://jdk.java.net/26/release-notes">release notes page</a> and <a href="https://docs.oracle.com/en/java/javase/26/docs/api/index.html">API Javadoc page</a>.</p>
</div>
</div>
</div>
<div class="sect1">
<h2 id="mcp">Updates to <code>mcpServer-1.0</code></h2>
<div class="sectionbody">
<div class="paragraph">
<p>The <a href="https://modelcontextprotocol.io/docs/getting-started/intro">Model Context Protocol (MCP)</a> is an open standard that enables AI applications to access real-time information from external sources. The Liberty MCP Server feature <code>mcpServer-1.0</code> allows developers to expose the business logic of their applications, allowing it to be integrated into agentic AI workflows.</p>
</div>
<div class="paragraph">
<p>This beta release of Open Liberty includes updates to the <code>mcpServer-1.0</code> feature including dynamic registration of tools and support for version <code>2025-11-25</code> of the protocol.</p>
</div>
<div class="sect2">
<h3 id="dynamically-register-mcp-tools">Dynamically register MCP tools</h3>
<div class="paragraph">
<p>Tools can now be registered dynamically through an API. This capability allows the set of available tools on the server to be adjusted based on configuration or environment.</p>
</div>
<div class="paragraph">
<p>Tools can be registered by injecting <code>ToolManager</code> and calling its methods to add, remove, and list the available tools on the server. The full Javadoc for <code>ToolManager</code> can be found within the Liberty beta in <code>dev/api/ibm/javadoc/io.openliberty.mcp_1.0-javadoc.zip</code>.</p>
</div>
<div class="paragraph">
<p>Tools can be registered when the application starts through the CDI <code>Startup</code> event. See the following example where the <code>Startup</code> event is used to register a weather forecast tool only if a <code>WeatherClient</code> bean is available.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code>    <span class="annotation">@Inject</span>
    ToolManager toolManager;

    <span class="annotation">@Inject</span>
    Instance&lt;WeatherClient&gt; weatherClientInstance;

    <span class="directive">private</span> <span class="type">void</span> createWeatherTool(<span class="annotation">@Observes</span> Startup startup) {
        <span class="keyword">if</span> (weatherClientInstance.isResolvable()) {
            WeatherClient weatherClient = weatherClientInstance.get();
            toolManager.newTool(<span class="string"><span class="delimiter">&quot;</span><span class="content">getForecast</span><span class="delimiter">&quot;</span></span>)
                       .setTitle(<span class="string"><span class="delimiter">&quot;</span><span class="content">Weather Forecast Provider</span><span class="delimiter">&quot;</span></span>)
                       .setDescription(<span class="string"><span class="delimiter">&quot;</span><span class="content">Get weather forecast for a location</span><span class="delimiter">&quot;</span></span>)
                       .addArgument(<span class="string"><span class="delimiter">&quot;</span><span class="content">latitude</span><span class="delimiter">&quot;</span></span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">Latitude of the location</span><span class="delimiter">&quot;</span></span>, <span class="predefined-constant">true</span>, <span class="predefined-type">Double</span>.class)
                       .addArgument(<span class="string"><span class="delimiter">&quot;</span><span class="content">longitude</span><span class="delimiter">&quot;</span></span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">Longitude of the location</span><span class="delimiter">&quot;</span></span>, <span class="predefined-constant">true</span>, <span class="predefined-type">Double</span>.class)
                       .setHandler(toolArguments -&gt; {
                           <span class="predefined-type">Double</span> latitude = (<span class="predefined-type">Double</span>) toolArguments.args().get(<span class="string"><span class="delimiter">&quot;</span><span class="content">latitude</span><span class="delimiter">&quot;</span></span>);
                           <span class="predefined-type">Double</span> longitude = (<span class="predefined-type">Double</span>) toolArguments.args().get(<span class="string"><span class="delimiter">&quot;</span><span class="content">longitude</span><span class="delimiter">&quot;</span></span>);
                           <span class="predefined-type">String</span> result = weatherClient.getForecast(
                                     latitude,
                                     longitude,
                                     <span class="integer">4</span>,
                                     <span class="string"><span class="delimiter">&quot;</span><span class="content">temperature_2m,snowfall,rain,precipitation,precipitation_probability</span><span class="delimiter">&quot;</span></span>);
                           <span class="keyword">return</span> ToolResponse.success(result);
                       })
                       .register();
        }
    }</code></pre>
</div>
</div>
<div class="paragraph">
<p>Tool registration starts with <code>newTool()</code>, then the information about the tool is added, including its title, description, and arguments. The handler supplies the code to run when the tool is called. It has one parameter, which is a <code>ToolArguments</code> object, which provides access to the arguments and other information about the tool call request. The handler must always create and return a <code>ToolResponse</code>.</p>
</div>
</div>
<div class="sect2">
<h3 id="accept-the-2025-11-25-mcp-protocol">Accept the 2025-11-25 MCP protocol</h3>
<div class="paragraph">
<p>The <code>mcpServer-1.0</code> feature now accepts version <code>2025-11-25</code> of the Model Context Protocol (MCP), although it does not support any new features introduced in this version of the protocol.</p>
</div>
<div class="paragraph">
<p>Supporting MCP 2025-11-25 introduces two notable behavior changes:</p>
</div>
<div class="ulist">
<ul>
<li>
<p><strong>Tool name restrictions</strong>: Tool names are now limited to ASCII letters ('A–Z, a–z'), digits ('0–9'), and the underscore ('_'), hyphen ('-'), and dot ('.') characters.</p>
</li>
<li>
<p><strong>Improved handling of invalid tool arguments</strong>: Invalid arguments passed when invoking a tool are now treated as <em>tool execution errors</em> rather than <em>protocol errors</em>. This change allows the LLM to receive feedback that the tool was called incorrectly and to retry with corrected arguments.</p>
</li>
</ul>
</div>
</div>
<div class="sect2">
<h3 id="paginated-results">Paginated results</h3>
<div class="paragraph">
<p>The result of a <code>tools/list</code> call is now paginated with a page size of 20. This means that if there are more than 20 tools deployed on the server, clients make several small calls to retrieve the list of tools, rather than one large call.</p>
</div>
</div>
<div class="sect2">
<h3 id="bug-fixes">Bug fixes</h3>
<div class="ulist">
<ul>
<li>
<p>During cancellation of a tool call, we check that both the session id and the authenticated user match the session id and the user that made the tool call. Previously only the session id was checked.</p>
</li>
<li>
<p>Messages that are returned to the MCP client no longer contain Open Liberty message codes.</p>
</li>
<li>
<p>Structured content is only returned when client is using protocol version <code>2025-06-18</code> or later.</p>
</li>
</ul>
</div>
</div>
<div class="sect2">
<h3 id="further-information">Further information</h3>
<div class="ulist">
<ul>
<li>
<p>For more information about the <code>mcpServer-1.0</code> feature and how to get started using the beta, see our <a href="https://openliberty.io/blog/2025/10/23/mcp-standalone-blog.html">dedicated blog post</a>.</p>
</li>
<li>
<p>For more information about the Model Context Protocol, see <a href="https://modelcontextprotocol.io/docs/getting-started/intro">modelcontextprotocol.io</a></p>
</li>
</ul>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jakarta_data">Preview of some Jakarta Data 1.1 M2 capability</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Previews some new capability at the Jakarta Data 1.1 Milestone 2 level: <code>Constraint</code> subtype parameters for repository methods that constrain to repository <code>@Find</code> operations and limited use of <code>Restriction</code> with repository <code>@Find</code> operations. Also included from the prior beta are: retrieving a subset/projection of entity attributes and the <code>@Is</code> annotation.</p>
</div>
<div class="paragraph">
<p>Previously, parameter-based <code>@Find</code> reposotory methods could filter results only using equality conditions. This limitation has now been removed, allowing additional filtering options to be defined.</p>
</div>
<div class="paragraph">
<p>Filtering can now be specified in several ways. One approach is to define the repository method parameter as a <code>Constraint</code> subtype, or to indicate the constraint subtype by using the <code>@Is</code> annotation. Alternatively, a repository <code>@Find</code> method can include a special parameter of type <code>Restriction</code>, allowing the application to supply one or more restrictions dynamically at runtime when the method is invoked.</p>
</div>
<div class="paragraph">
<p>In Jakarta Data, you define simple Java objects called <code>entities</code> to represent data, and interfaces called <code>repositories</code> to define data operations. You inject a repository into your application and use it. The implementation of the repository is automatically provided.</p>
</div>
<div class="paragraph">
<p>Start by defining an entity class that corresponds to your data. With relational databases, the entity class corresponds to a database table and the entity properties (public methods and fields of the entity class) generally correspond to the columns of the table. An entity class can be:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>annotated with <code>jakarta.persistence.Entity</code> and related annotations from Jakarta Persistence</p>
</li>
<li>
<p>a Java class without entity annotations, in which case the primary key is inferred from an entity property named <code>id</code> or ending with <code>Id</code> and an entity property named <code>version</code> designates an automatically incremented version column.</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>Here&#8217;s a simple entity:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Entity</span>
<span class="directive">public</span> <span class="type">class</span> <span class="class">Product</span> {
    <span class="annotation">@Id</span>
    <span class="directive">public</span> <span class="type">long</span> id;

    <span class="directive">public</span> <span class="predefined-type">String</span> name;

    <span class="directive">public</span> <span class="type">double</span> price;

    <span class="directive">public</span> <span class="type">double</span> weight;
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>After you define the entity to represent the data, it is usually helpful to have your IDE generate a static metamodel class for it. By convention, static metamodel classes begin with the underscore ('_') character, followed by the entity name.</p>
</div>
<div class="paragraph">
<p>Because this beta is available well before the release of Jakarta Data 1.1, IDE support for this generation cannot be expected yet. However, a static metamodel class can be provided in the same form that an IDE would generate for the <code>Product</code> entity.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@StaticMetamodel</span>(Product.class)
<span class="directive">public</span> <span class="type">interface</span> <span class="class">_Product</span> {
    <span class="predefined-type">String</span> ID = <span class="string"><span class="delimiter">&quot;</span><span class="content">id</span><span class="delimiter">&quot;</span></span>;
    <span class="predefined-type">String</span> NAME = <span class="string"><span class="delimiter">&quot;</span><span class="content">name</span><span class="delimiter">&quot;</span></span>;
    <span class="predefined-type">String</span> PRICE = <span class="string"><span class="delimiter">&quot;</span><span class="content">price</span><span class="delimiter">&quot;</span></span>;
    <span class="predefined-type">String</span> WEIGHT = <span class="string"><span class="delimiter">&quot;</span><span class="content">weight</span><span class="delimiter">&quot;</span></span>;

    NumericAttribute&lt;Product, <span class="predefined-type">Long</span>&gt; id = NumericAttribute.of(
            Product.class, ID, <span class="type">long</span>.class);
    <span class="predefined-type">TextAttribute</span>&lt;Product&gt; name = <span class="predefined-type">TextAttribute</span>.of(
            Product.class, NAME);
    NumericAttribute&lt;Product, <span class="predefined-type">Double</span>&gt; price = NumericAttribute.of(
            Product.class, PRICE, <span class="type">double</span>.class);
    NumericAttribute&lt;Product, <span class="predefined-type">Double</span>&gt; weight = NumericAttribute.of(
            Product.class, WEIGHT, <span class="type">double</span>.class);
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>The first half of the static metamodel class includes constants for each of the entity attribute names so that you don&#8217;t need to otherwise hardcode string values into your application. The second half of the static metamodel class provides a special instance for each entity attribute, from which you can build restrictions and sorting to apply to queries at run time. This capability enables many powerful operations with repository queries, but those features are deferred to a future beta.</p>
</div>
<div class="paragraph">
<p>A repository defines operations that are related to the Product entity. Your repository interface can inherit from built-in interfaces such as <code>BasicRepository</code> and <code>CrudRepository</code> to gain various general-purpose repository methods for inserting, updating, deleting, and querying for entities. In addition to these capabilities, the repository can define custom operations by using the static metamodel and annotations such as @Find or @Delete.</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@Repository</span>(dataStore = <span class="string"><span class="delimiter">&quot;</span><span class="content">java:app/jdbc/my-example-data</span><span class="delimiter">&quot;</span></span>)
<span class="directive">public</span> <span class="type">interface</span> <span class="class">Products</span> <span class="directive">extends</span> CrudRepository&lt;Product, <span class="predefined-type">Long</span>&gt; {

    <span class="comment">// Filtering is pre-defined at development time by the @Is annotation,</span>
    <span class="annotation">@Find</span>
    <span class="annotation">@OrderBy</span>(_Product.PRICE)
    <span class="annotation">@OrderBy</span>(_Product.NAME)
    <span class="predefined-type">List</span>&lt;Product&gt; costingUpTo(<span class="annotation">@By</span>(_Product.PRICE) <span class="annotation">@Is</span>(AtMost.class) <span class="type">double</span> maxPrice);

    <span class="comment">// Constraint types, such as Like, can also pre-define filtering.</span>
    <span class="comment">// Allow additional filtering at run time by including a Restriction,</span>
    <span class="annotation">@Find</span>
    Page&lt;Product&gt; named(<span class="annotation">@By</span>(_Product.Name) Like namePattern,
                        Restriction&lt;Product&gt; filter,
                        Order&lt;Product&gt; sorting,
                        PageRequest pageRequest);

    <span class="comment">// Retrieve a single entity attribute, identified by @Select,</span>
    <span class="annotation">@Find</span>
    <span class="annotation">@Select</span>(_Product.PRICE)
    Optional&lt;<span class="predefined-type">Double</span>&gt; priceOf(<span class="annotation">@By</span>(_Product.ID) <span class="type">long</span> productNum);

    <span class="comment">// Retrieve multiple entity attributes as a Java record,</span>
    <span class="annotation">@Find</span>
    <span class="annotation">@Select</span>(_Product.WEIGHT)
    <span class="annotation">@Select</span>(_Product.NAME)
    Optional&lt;WeightInfo&gt; weightAndNameOf(<span class="annotation">@By</span>(_Product.ID) <span class="type">long</span> productNum);

    <span class="directive">static</span> record WeightInfo(<span class="type">double</span> itemWeight, <span class="predefined-type">String</span> itemName) {}
}</code></pre>
</div>
</div>
<div class="paragraph">
<p>Here is an example of using the repository and static metamodel:</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="annotation">@DataSourceDefinition</span>(name = <span class="string"><span class="delimiter">&quot;</span><span class="content">java:app/jdbc/my-example-data</span><span class="delimiter">&quot;</span></span>,
                      className = <span class="string"><span class="delimiter">&quot;</span><span class="content">org.postgresql.xa.PGXADataSource</span><span class="delimiter">&quot;</span></span>,
                      databaseName = <span class="string"><span class="delimiter">&quot;</span><span class="content">ExampleDB</span><span class="delimiter">&quot;</span></span>,
                      serverName = <span class="string"><span class="delimiter">&quot;</span><span class="content">localhost</span><span class="delimiter">&quot;</span></span>,
                      portNumber = <span class="integer">5432</span>,
                      user = <span class="string"><span class="delimiter">&quot;</span><span class="content">${example.database.user}</span><span class="delimiter">&quot;</span></span>,
                      password = <span class="string"><span class="delimiter">&quot;</span><span class="content">${example.database.password}</span><span class="delimiter">&quot;</span></span>)
<span class="directive">public</span> <span class="type">class</span> <span class="class">MyServlet</span> <span class="directive">extends</span> HttpServlet {
    <span class="annotation">@Inject</span>
    Products products;

    <span class="directive">protected</span> <span class="type">void</span> doGet(HttpServletRequest req, HttpServletResponse resp)
            <span class="directive">throws</span> ServletException, <span class="exception">IOException</span> {
        <span class="comment">// Insert:</span>
        Product prod = ...
        prod = products.insert(prod);

        <span class="comment">// Filter by supplying values only:</span>
        <span class="predefined-type">List</span>&lt;Product&gt; found = products.costingUpTo(<span class="float">50.0</span>, <span class="string"><span class="delimiter">&quot;</span><span class="content">%keyboard%</span><span class="delimiter">&quot;</span></span>);

        <span class="comment">// Compute filtering at run time,</span>
        Page&lt;Product&gt; page1 = products.named(
                        Like.suffix(<span class="string"><span class="delimiter">&quot;</span><span class="content">printer</span><span class="delimiter">&quot;</span></span>),
                        Restrict.all(_Product.price.times(taxRate).plus(shipping).lessThan(<span class="float">250.0</span>),
                                     _Product.weight.times(kgToPounds).between(<span class="float">10.0</span>, <span class="float">20.0</span>)),
                        Order.by(_Product.price.desc(),
                                 _Product.name.asc(),
                                 _Product.id.asc()),
                        PageRequest.ofSize(<span class="integer">10</span>));

        <span class="comment">// Find one entity attribute:</span>
        price = products.priceOf(prod.id).orElseThrow();

        <span class="comment">// Find multiple entity attributes as a Java record:</span>
        WeightInfo info = products.getWeightAndName(prod.id);
        <span class="predefined-type">System</span>.out.println(info.itemName() + <span class="string"><span class="delimiter">&quot;</span><span class="content"> weighs </span><span class="delimiter">&quot;</span></span> + info.itemWeight() + <span class="string"><span class="delimiter">&quot;</span><span class="content"> kg.</span><span class="delimiter">&quot;</span></span>);

        ...
    }
}</code></pre>
</div>
</div>
<div class="sect2">
<h3 id="learn-more-2">Learn more</h3>
<div class="ulist">
<ul>
<li>
<p><a href="https://jakarta.ee/specifications/data/1.1/apidocs">Jakarta Data 1.1 API Javadoc</a></p>
</li>
<li>
<p><a href="https://jakarta.ee/specifications/data/1.1/">Jakarta Data 1.1 overview page</a></p>
</li>
<li>
<p>Maven coordinates for Data 1.1 Milestone 2:</p>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;dependency&gt;</span>
    <span class="tag">&lt;groupId&gt;</span>jakarta.data<span class="tag">&lt;/groupId&gt;</span>
    <span class="tag">&lt;artifactId&gt;</span>jakarta.data-api<span class="tag">&lt;/artifactId&gt;</span>
    <span class="tag">&lt;version&gt;</span>1.1.0-M2<span class="tag">&lt;/version&gt;</span>
<span class="tag">&lt;/dependency&gt;</span></code></pre>
</div>
</div>
<div class="admonitionblock note">
<table>
<tr>
<td class="icon">
<i class="fa icon-note" title="Note"></i>
</td>
<td class="content">
This beta includes only the Data 1.1 features for entity subsets and projections, the @Is annotation, and Constraint subtypes as parameters for repository Find methods. It also includes limited support for Restriction as a special parameter for repository Find methods. The <code>navigate</code> operations are not implemented for restrictions and constraints. Other new Data 1.1 features are not included in this beta.
</td>
</tr>
</table>
</div>
</li>
</ul>
</div>
</div>
</div>
</div>
<div class="sect1">
<h2 id="jandex_index">Support for Reading Jandex Indexes from WEB-INF/classes in Web Modules.</h2>
<div class="sectionbody">
<div class="paragraph">
<p>Previously, Jandex indexes across all module types were read only from <code>META-INF/jandex.idx</code>. This update extends support for web modules to also read indexes from <code>WEB-INF/classes/META-INF/jandex.idx</code>, bringing it in line with industry practices.</p>
</div>
<div class="paragraph">
<p>Jandex indexes are read from the original or the new location only when Jandex use is enabled. For compatibility with earlier versions, reading indexes from the new location must be enabled using a new application property.</p>
</div>
<div class="sect2">
<h3 id="benefits">Benefits</h3>
<div class="ulist">
<ul>
<li>
<p>Improved Compatibility: Improve compatibility with industry standard practices.</p>
</li>
<li>
<p>Faster Startup Times: Continue to benefit from Jandex&#8217;s efficient annotation indexing.</p>
</li>
</ul>
</div>
</div>
<div class="sect2">
<h3 id="target-persona">Target Persona</h3>
<div class="paragraph">
<p>Customers who wish to place Jandex indexes at industry standard locations in their web modules.</p>
</div>
</div>
<div class="sect2">
<h3 id="target-features">Target Features</h3>
<div class="paragraph">
<p>This update applies to the scanning of classes within applications. Class scanning (which includes annotation scanning) is a general purpose function that is used by many features.</p>
</div>
</div>
<div class="sect2">
<h3 id="problem-description">Problem Description</h3>
<div class="paragraph">
<p>To gather information about application classes, including annotation data, the classes must be scanned. This scanning process is expensive because the entire application must be read and processed.</p>
</div>
<div class="paragraph">
<p>To improve performance, class scanning can be performed during application packaging, with the scan results stored in an index within the application. Jandex provides this capability.</p>
</div>
<div class="paragraph">
<p>Jandex scan results are typically written to <code>META-INF/jandex.idx</code>.</p>
</div>
<div class="paragraph">
<p>For JAR files, this path is relative to the root of the JAR. For WAR files, however, the <code>META-INF/jandex.idx</code> path can have two interpretations: it can be relative to the root of the WAR file, or relative to the <code>WEB-INF/classes</code> directory.</p>
</div>
<div class="paragraph">
<p>Before this update, Liberty used the location relative to the root of the WAR file. However, industry-standard practices place the location relative to the <code>WEB-INF/classes</code> directory. This update enables Liberty to also use the <code>WEB-INF/classes</code> location.</p>
</div>
</div>
<div class="sect2">
<h3 id="how-to-use-2">How to Use</h3>
<div class="paragraph">
<p>Support for reading Jandex indexes under <code>WEB-INF/classes</code> is enabled by a new property on the <code>application</code> and <code>applicationManager</code> elements:</p>
</div>
<div class="ulist">
<ul>
<li>
<p>Property: <strong>useJandexUnderClasses</strong></p>
</li>
<li>
<p>Description: Enables use of a Jandex index for WAR <code>WEB-INF/classes</code> under <code>/WEB-INF/classes/META-INF/jandex.idx</code>.</p>
</li>
<li>
<p>Possible values: true | false</p>
</li>
<li>
<p>Default value: false</p>
</li>
</ul>
</div>
<div class="paragraph">
<p>Example: applicationManager</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;applicationManager</span> <span class="attribute-name">useJandex</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">true</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">useJandexUnderClasses</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">true</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>Example: application</p>
</div>
<div class="listingblock">
<div class="content">
<pre class="CodeRay highlight"><code><span class="tag">&lt;application</span> <span class="attribute-name">location</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">TestServlet40.ear</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">useJandex</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">true</span><span class="delimiter">&quot;</span></span> <span class="attribute-name">useJandexUnderClasses</span>=<span class="string"><span class="delimiter">&quot;</span><span class="content">true</span><span class="delimiter">&quot;</span></span><span class="tag">/&gt;</span></code></pre>
</div>
</div>
<div class="paragraph">
<p>When the new property is placed on an application manager element, it applies to all web modules of all applications. When the new property is placed on an application element, the property applies to only that application. If the property is set on both the application manager element and an application element, the value on the application element overrides the value on the application manager element for that application.</p>
</div>
</div>
<div class="sect2">
<h3 id="limitation">Limitation</h3>
<div class="paragraph">
<p>Jandex index support requires explicit enablement. See the <code>useJandex</code> property  on <code>applicationManager</code> and on <code>application</code> elements. The new <code>useJandexUnderClasses</code> property is meaningful only if the <code>useJandex</code> property is <code>true</code>.</p>
</div>
<div class="paragraph">
<p>For compatibility with earlier versions, reads of Jandex from the new location require explicit enablement. See the new <strong>useJandexUnderClasses</strong> property, as documented previously. Explicit enablement is required to prevent applications from accidentally reading an out of date Jandex index from the new location. An out of date Jandex index might cause application errors that are hard to detect.</p>
</div>
<div class="paragraph">
<p>The name of the new property, <strong>useJandexUnderClasses</strong>, is subject to revision.</p>
</div>
</div>
<div class="sect2">
<h3 id="learn-more-3">Learn More</h3>
<div class="ulist">
<ul>
<li>
<p><a href="https://smallrye.io/jandex/jandex/3.5.3/index.html">Jandex Documentation</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/config/applicationManager.html">Application Manager Documentation</a></p>
</li>
<li>
<p><a href="https://openliberty.io/docs/latest/reference/config/application.html">Application Documentation</a></p>
</li>
<li>
<p><a href="https://github.com/OpenLiberty/open-liberty/issues/32438">Look for jandex index under WEB-INF/classes</a></p>
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
          <span class="tag">&lt;version&gt;</span>26.0.0.4-beta<span class="tag">&lt;/version&gt;</span>
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
        classpath 'io.openliberty.tools:liberty-gradle-plugin:4.0.0'
    }
}
apply plugin: 'liberty'
dependencies {
    libertyRuntime group: 'io.openliberty.beta', name: 'openliberty-runtime', version: '[26.0.0.4-beta,)'
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
<p>For more information on using a beta release, refer to the <a href="https://openliberty.io/blog/2026/04/07/docs/latest/installing-open-liberty-betas.html">Installing Open Liberty beta releases</a> documentation.</p>
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
