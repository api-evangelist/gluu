---
title: "Keycloak Roadmap"
url: "https://gluu.org/keycloak-roadmap/"
date: "Fri, 02 Jun 2023 20:24:45 +0000"
author: "Michael Schwartz"
feed_url: "https://gluu.org/feed/"
---
<p>by <a href="https://www.linkedin.com/in/nynymike/">Michael Schwartz</a>, CEO of Gluu</p>
<p>The recent <a href="https://www.cncf.io/blog/2023/04/11/keycloak-joins-cncf-as-an-incubating-project/">announcement</a> that Keycloak is joining the CNCF as an incubating project was welcome news!!! It resolved two important questions. How would Red Hat transfer governance of the project? Who owns the Keycloak trademark? Consequently, Gluu is working to integrate Keycloak into both the Janssen Project and the commercial Gluu Flex distribution.</p>
<p>You may be wondering if Jans Auth Server and Keycloak are both identity providers, how do they connect?  And why are we doing this?</p>
<p>The answer is that there is room in the Janssen Project for lots of identity tools. While Jans Auth Server currently provides a lot of core functionality, other components are also important, like the FIDO and SCIM servers.  The modular Jans Config API and tools provide a single API management plane for all the components.  And finally, Janssen Project includes a setup script that bootstraps new deployments and cloud native assets like Helm charts and a Terraform provider.</p>
<p>At Gluu, we realize that no one open source IDP will rule them all. There are lots of different IDPs that were written to solve specific problems. People are still writing new open source identity providers, like <a href="https://github.com/zitadel/zitadel">Zitadel</a>.  It would be cool if there was a Rust IDP (maybe one day at the Janssen Project?) Janssen Auth Server was designed for FIPS, high concurrency, multi-datacenter, database agnostic, auto-scaling deployments&#8230; customizable with reusable low code technology. There is no way we could have done that, and solved the myriad of other design objectives that various open source IDPs pursue.  Keycloak, is a &#8220;complete, ready-to-run IAM service in a single lightweight container image.&#8221; It supports SAML and &#8220;Realms&#8221;&#8211;features primarily required for enterprise workforce applications and access control, that some in the Janssen community want.</p>
<p>Plus Keycloak users are our kind of people&#8211;they believe that enterprise IAM infrastructure should leverage code developed through an open source community. In other words&#8230; the enlightened.  We want to create a bridge for collaboration.</p>
<p>There are seven integrations we&#8217;re undertaking to leverage the new capabilities from KeyCloak</p>
<ol>
<li>Add Keycloak to Jans Setup, as an optional component</li>
<li>Write a Keycloak authentication provider to achieve SSO between Jans Auth Server and Keycloak client</li>
<li>Update Cache Refresh to sync Keycloak database</li>
<li>Add a Jans Config API endpoint to manage SAML Trust Relations and attribute release policies in Keycloak</li>
<li>Add SAML trust relationship management in the Jans Text UI and command line interface</li>
<li>Add SAML trust relationship management in the Gluu Flex Admin UI</li>
<li>Add Agama Engine support directly into Keycloak</li>
</ol>
<div>
<p>Hopefully, we&#8217;ll see an early release by the end of June that covers at least the first four of these items.</p>
</div><p>The post <a href="https://gluu.org/keycloak-roadmap/">Keycloak Roadmap</a> first appeared on <a href="https://gluu.org">Gluu</a>.</p>
